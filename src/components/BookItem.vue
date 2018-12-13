<template>
  <li>{{book.title}}: {{book.author}} <span v-on:click="bookDelete(index)">x</span></li>
</template>

<script>
  
  export default {
    name: 'BookItem',
    props: {
      book: {},
      index: Number
    },
    methods: {
      bookDelete(index) {
        this.$emit('removeBook', index)
      }
    }
  }
</script>

<style>
  li {
    display: block;
    margin: 0 10px;
  }
</style>