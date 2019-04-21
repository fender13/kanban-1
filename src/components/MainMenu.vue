<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand href="#">KamVan Board</b-navbar-brand>

      <b-navbar-toggle target="nav_collapse"/>

      <b-collapse is-nav id="nav_collapse">
        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <div>
            <b-button @click="show=true" v-b-modal.modalxl variant="primary">New Task</b-button>

            <b-modal v-model="show" id="modalxl" size="xl" title="Add New Task" @ok="handleOk">
              <form @submit.prevent="handleSubmit">
                <b-row class="mb-3">
                  <b-col cols="2">Title</b-col>
                  <b-col>
                    <b-form-input type="text" placeholder="Enter your task" v-model="title"/>
                  </b-col>
                </b-row>

                <b-row class="mb-3">
                  <b-col cols="2">Description</b-col>
                  <b-col>
                    <b-form-textarea
                      id="textarea-small"
                      size="sm"
                      placeholder="Small textarea"
                      v-model="description"
                    />
                  </b-col>
                </b-row>

                <b-row class="mb-3">
                  <b-col cols="2">Point</b-col>
                  <b-col>
                    <b-form-input type="number" placeholder="Enter your task" v-model="point"/>
                  </b-col>
                </b-row>

                <b-row class="mb-3">
                  <b-col cols="2">Assigned To</b-col>
                  <b-col>
                    <b-form-input type="text" placeholder="Enter your task" v-model="assignTo"/>
                  </b-col>
                </b-row>
              </form>
            </b-modal>
          </div>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
import firebase from "@/database/firebase.js"
import router from "../router.js"

export default {
  name: "MainMenu",
  data() {
    return {
      show: false,
      title: "",
      description: "",
      point: 0,
      assignTo: "",
      status: "backLog"
    };
  },
  methods: {
    handleOk(evt) {
      // Prevent modal from closing
      evt.preventDefault();
      if (!this.title) {
        alert("Please enter your title");
      } else if (!this.description) {
        alert("Please enter your desription");
      } else if (!this.point) {
        alert("Please enter your point");
      } else if (!this.assignTo) {
        alert("Please enter your assignTo");
      } else {
        this.handleSubmit();
      }
    },
    handleSubmit() {
      firebase
        .collection("TodoList")
        .add({
          title: this.title,
          description: this.description,
          point: this.point,
          assignTo: this.assignTo,
          status: this.status,
          createdAt: new Date()
        })
        .then((data) => {
          this.show = false
          this.title = ""
          this.description = ""
          this.point = 0
          this.assignTo = ""
        })
        .catch((error) => {
          console.log(error);
        });
    }
  }
};
</script>
