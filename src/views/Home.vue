<template>
  <div class="home">
    <MainMenu></MainMenu>
    <KanbanBoard></KanbanBoard>
  </div>
</template>

<script>
import MainMenu from '@/components/MainMenu.vue'
import KanbanBoard from '@/components/KanbanBoard.vue'

export default {
  name: 'Home',
  components: {
    MainMenu,
    KanbanBoard
  }
}
</script>
