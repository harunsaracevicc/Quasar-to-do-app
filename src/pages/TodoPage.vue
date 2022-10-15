<template>
  <q-page class="bg-grey-2 column">
    <div class="row q-pa-sm bg-dark">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list separator bordered class="bg-white">
      <q-item
        clickable
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-yellow-2': task.done }"
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            class="no-pointer-events"
            v-model="task.done"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section side>
          <q-btn
            @click.stop="editTask()"
            dense
            flat
            round
            color="primary"
            icon="edit"
          >
          </q-btn>
        </q-item-section>
        <q-item-section class="align-end" v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            dense
            flat
            round
            color="negative"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>

    <div
      class="no-task absolute-center text-center no-task"
      v-if="!tasks.length"
    >
      <q-icon name="check" size="100px"></q-icon>
      <div class="text-h5">No tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "Go to shopping",
        //   done: false,
        // },
        // {
        //   title: "Go to gym",
        //   done: false,
        // },
        // {
        //   title: "Study",
        //   done: false,
        // },
        // {
        //   title: "Making apps",
        //   done: false,
        // },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Do you wanna delete this task?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            message: "Task deleted!",
          });
        });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
    editTask() {
      console.log("Clicked");
    },
  },
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #b1b1b1;
  }
}

.q-btn .q-icon {
  font-size: 20px;
}
.q-item__section--main ~ .q-item__section--side {
  padding-left: 0;
  padding-right: 5px;
}
.no-task {
  color: $primary;
  opacity: 0.7;
}
</style>
