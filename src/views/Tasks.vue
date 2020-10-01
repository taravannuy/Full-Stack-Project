<template>
  <div>
    <h1>RemindUS</h1>
    <table id="tasks" class="ui celled compact table">
      <thead>
        <tr>
         <th style="background-color:#00ffff;">  <i class="calendar plus icon"></i>Task</th>
          <th style="background-color:#00ffff"> <i class="info circle icon"></i>Detail</th>
                    <th style="background-color:#00ffff"> <i class="lock open icon"></i></th>
                   <th style="background-color:#00ffff"> <i class="edit icon"></i></th>
                    <th style="background-color:#00ffff"> <i class="trash icon"></i></th>
 


          <!-- <th colspan="3"></th> -->
        </tr>
      </thead>
      <tr v-for="(task, i) in tasks" :key="i">
        <td>{{ task.task1 }}</td>
        <td>{{ task.task2 }}</td>
        <td width="75" class="center aligned">
          <router-link :to="{ name: 'show', params: { id: task._id }}">Show</router-link>
        </td>
        <td width="75" class="center aligned">
          <router-link :to="{ name: 'edit', params: { id: task._id }}">Edit</router-link>
        </td>
        <td width="75" class="center aligned" @click.prevent="onDestroy(task._id)">
          <a :href="`/tasks/${task._id}`">Delete</a>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import { api } from '../helpers/helpers';
export default {
  name: 'tasks',
  data() {
    return {
      tasks: []
    };
  },
  methods: {
    async onDestroy(id) {
      const sure = window.confirm('Are you sure?');
      if (!sure) return;
      await api.deletetask(id);
      this.flash('task deleted sucessfully!', 'success');
      const newtasks = this.tasks.filter(task => task._id !== id);
      this.tasks = newtasks;
    }
  },
  async mounted() {
    this.tasks = await api.gettasks();
  }
};
</script>
<style scoped>
  td{background-color:#00ffff;}
</style>