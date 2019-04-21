<template>
  <b-container class="bv-example-row container">
    <b-row>
      <b-col>
        <div class="main-card">
          <b-card-group deck>
            <b-card
              border-variant="danger"
              header="Back-Log"
              header-bg-variant="danger"
              header-text-variant="white"
              align="center"
            >
              <div v-for="(list, index) in todoList" :key="index">
                <div v-if="list.status == 'backLog'">
                  <div class="card mb-3">
                    <div class="card-block">
                      <div class="todo-head">
                        <p class="card-title">{{ list.title }}</p>
                      </div>
                      <p class="card-title">Point: {{ list.point }}</p>
                      <p class="card-title">Assign To: {{ list.assignTo }}</p>
                      <div class="job-status">
                        <b-row>
                          <b-col></b-col>
                          <b-col>
                            <a href="#" v-on:click.prevent="deleteData(list.id)">Delete</a>
                          </b-col>
                          <b-col>
                            <a href="#" v-on:click.prevent="updateToDoing(list.id)">To-Do</a>
                          </b-col>
                        </b-row>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </b-card>
          </b-card-group>
        </div>
      </b-col>
      <b-col>
        <div class="main-card">
          <b-card-group deck>
            <b-card
              border-variant="warning"
              header="To-Do"
              header-bg-variant="warning"
              header-text-variant="white"
              align="center"
            >
              <div v-for="(list, index) in todoList" :key="index">
                <div v-if="list.status == 'todo'">
                  <div class="card mb-3">
                    <div class="card-block">
                      <div class="todo-head">
                        <p class="card-title">{{ list.title }}</p>
                      </div>
                      <p class="card-title">Point: {{ list.point }}</p>
                      <p class="card-title">Assign To: {{ list.assignTo }}</p>
                      <div class="job-status">
                        <b-row>
                          <b-col>
                            <a href="#" v-on:click.prevent="updateToBackLog(list.id)">Back-Log</a>
                          </b-col>
                          <b-col>
                            <a href="#" v-on:click.prevent="deleteData(list.id)">Delete</a>
                          </b-col>
                          <b-col>
                            <a href="#" v-on:click.prevent="updateToOnGoing(list.id)">On-Going</a>
                          </b-col>
                        </b-row>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </b-card>
          </b-card-group>
        </div>
      </b-col>
      <b-col>
        <div class="main-card">
          <b-card-group deck>
            <b-card
              border-variant="primary"
              header="On Going"
              header-bg-variant="primary"
              header-text-variant="white"
              align="center"
            >
              <div v-for="(list, index) in todoList" :key="index">
                <div v-if="list.status == 'ongoing'">
                  <div class="card mb-3">
                    <div class="card-block">
                      <div class="todo-head">
                        <p class="card-title">{{ list.title }}</p>
                      </div>
                      <p class="card-title">Point: {{ list.point }}</p>
                      <p class="card-title">Assign To: {{ list.assignTo }}</p>
                      <div class="job-status">
                        <b-row>
                          <b-col>
                            <a href="#" v-on:click.prevent="updateToDoing(list.id)">To-Do</a>
                          </b-col>
                          <b-col>
                            <a href="#" v-on:click.prevent="deleteData(list.id)">Delete</a>
                          </b-col>
                          <b-col>
                            <a href="#" v-on:click.prevent="updateToDone(list.id)">Done</a>
                          </b-col>
                        </b-row>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </b-card>
          </b-card-group>
        </div>
      </b-col>
      <b-col>
        <div class="main-card">
          <b-card-group deck>
            <b-card
              border-variant="success"
              header="Done"
              header-bg-variant="success"
              header-text-variant="white"
              align="center"
            >
              <div v-for="(list, index) in todoList" :key="index">
                <div v-if="list.status == 'done'">
                  <div class="card mb-3">
                    <div class="card-block">
                      <div class="todo-head">
                        <p class="card-title">{{ list.title }}</p>
                      </div>
                      <p class="card-title">Point: {{ list.point }}</p>
                      <p class="card-title">Assign To: {{ list.assignTo }}</p>
                      <div class="job-status">
                        <b-row>
                          <b-col>
                            <a href="#" v-on:click.prevent="updateToOnGoing(list.id)">On-Going</a>
                          </b-col>
                          <b-col>
                            <a href="#" v-on:click.prevent="deleteData(list.id)">Delete</a>
                          </b-col>
                          <b-col>
                            <a href="#"></a>
                          </b-col>
                        </b-row>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </b-card>
          </b-card-group>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import firebase from "@/database/firebase.js";
export default {
  name: "KanbanBoard",
  data() {
    return {
      tampil: false,
      todoList: []
    };
  },
  created: function() {
    return this.getTodo();
  },
  methods: {
    getTodo() {
      firebase
        .collection("TodoList")
        .orderBy("createdAt", "desc")
        .onSnapshot(snapshot => {
          this.todoList = [];
          snapshot.forEach(change => {
            this.todoList.push({ id: change.id, ...change.data() });
          });
        });
    },
    updateToBackLog(id) {
      firebase
        .collection("TodoList")
        .doc(id)
        .update({ status: "backLog" });
    },
    updateToDoing(id) {
      firebase
        .collection("TodoList")
        .doc(id)
        .update({ status: "todo" });
    },
    updateToOnGoing(id) {
      firebase
        .collection("TodoList")
        .doc(id)
        .update({ status: "ongoing" });
    },
    updateToDone(id) {
      firebase
        .collection("TodoList")
        .doc(id)
        .update({ status: "done" });
    },
    deleteData(id) {
      firebase
        .collection("TodoList")
        .doc(id)
        .delete();
    }
  }
};
</script>

<style scoped>
.container {
  margin-top: 5%;
}

h5 {
  text-align: left;
}

.todo-head {
  border-bottom: 0.5px solid black;
  margin-top: 3.5px;
  margin-bottom: 3.5px;
}

p {
  margin: auto;
  text-align: left;
  font-size: 14px;
}

.job-status {
  margin-top: 3px;
}

.job-status a {
  font-size: 12px;
}
</style>
