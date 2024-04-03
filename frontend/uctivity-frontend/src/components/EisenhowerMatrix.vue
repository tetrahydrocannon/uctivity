<template>
  <div class="eisenhower-matrix">
    <div class="quadrant" v-for="(quadrant, index) in quadrants" :key="index">
      <div class="quadrant-header">
        <h2>{{ quadrant.name }}</h2>
        <button @click="addTask(index)">Add Task</button>
      </div>
      <div class="scrollable-list">
        <draggable class="task-list" v-model="quadrant.tasks" group="tasks" @start="dragging = true" @end="dragging = false" :ref="'taskList' + index">
          <template #item="{ index }">
            <div class="task">
              <input v-model="quadrant.tasks[index].name" class="task-input" />
            </div>
          </template>
        </draggable>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable';

export default {
  components: {
    draggable,
  },
  data() {
    return {
      dragging: false,
      quadrants: [
        { name: 'Urgent & Important', tasks: [] },
        { name: 'Important, Not Urgent', tasks: [] },
        { name: 'Urgent, Not Important', tasks: [] },
        { name: 'Neither Urgent Nor Important', tasks: [] },
      ],
    };
  },
  methods: {
    addTask(quadrantIndex) {
      const newTask = { name: `Task ${this.quadrants[quadrantIndex].tasks.length + 1}` };
      this.quadrants[quadrantIndex].tasks.push(newTask);
      this.$nextTick(() => {
        const taskList = this.$refs[`taskList${quadrantIndex}`][0];
        taskList.scrollTop = taskList.scrollHeight;
      });
    },
  },
};
</script>

<style>
.eisenhower-matrix {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  gap: 20px;
  height: 50vh; /* Occupy 50% of the viewport height */
}

.quadrant {
  border: 1px solid #ccc;
  padding: 10px;
  display: flex;
  flex-direction: column;
  height: calc(50vh / 2 - 10px); /* Set fixed height for quadrants */
  width: calc(50vw - 20px); /* Set fixed width for quadrants */
}

.quadrant-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.scrollable-list {
  flex-grow: 1;
  overflow-y: auto;
  border: 1px solid #ccc; /* Add border to distinguish the box */
  margin-top: 10px;
  padding: 5px;
}

.task-list {
  min-height: 100%; /* Ensure task list fills the scrollable box */
}

.task {
  border: 1px solid #ccc;
  padding: 5px;
  margin: 5px 0;
}

.task-input {
  width: 100%;
  border: none;
  background-color: transparent;
}

.task-input:focus {
  outline: none;
}
</style>
