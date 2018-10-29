<template>
  <v-ons-page>
    <custom-toolbar :back-label="'Page 1'">ログイン成功!</custom-toolbar>
    <p style="text-align: center">ログインに成功したこのページにきます</p>
  </v-ons-page>
</template>

<script>
  import customToolbar from './CustomToolbar';
  import app from './App';

  export default {
    methods: {
      pop() {
        this.pageStack.pop();
      } 
    },
    props: ['pageStack'],
    components: { customToolbar }
  }
</script>
