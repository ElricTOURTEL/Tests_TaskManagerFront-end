<template>
  <div>
    <Hello/>
    <Tasks/>
  </div>
</template>

<script>
import Hello from '../components/Hello.vue';
import Tasks from '../components/TasksList.vue';
export default {
  components: { Hello, Tasks }  
};
</script>

