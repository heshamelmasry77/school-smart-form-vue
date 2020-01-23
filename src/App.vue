<template>
    <div id="app">
        <SchoolForm/>
    </div>
</template>

<script>
  import SchoolForm from './components/SchoolForm.vue';
  import './App.scss';

  export default {
    name: 'app',
    components: {
      SchoolForm,
    },
  };
</script>
