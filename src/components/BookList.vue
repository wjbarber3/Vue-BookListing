<template>
  <div>
    <h1>{{title}}</h1>
    <div v-if="loading">Loading Content</div>
    <ul>
      <book-item
        v-for="(book, index) in books"
        v-bind:key="book.title"
        :book="book"
        :index="index"
        @removeBook="deleteBook"></book-item>
      <book-form @addBook="appendBook"></book-form>
    </ul>
    <!-- <button>Save</button> -->
  </div>
</template>

<script>

import BookItem from './BookItem';
import BookForm from './BookForm';

export default {
  name: 'BookList',
  data() {
    return {
      title: 'All Books',
      books: [],
      loading: false,
      newContent: [
        {
          "title": "Wind Up Bird Chronical",
          "author": "Haruki Murakami"
        },
        {
          "title": "In The Miso Soup",
          "author": "Ryu Murakami"
        },
      ]
    }
  },
  components: {
    BookItem,
    BookForm
  },
  methods: {
    appendBook(bookTitle, bookAuthor) {
      this.books.push({
        title: bookTitle,
        author: bookAuthor
      })
      let xmlhttp = new XMLHttpRequest();   // new HttpRequest instance 
      xmlhttp.open("PUT", "http://localhost:3000/books");
      xmlhttp.setRequestHeader("Content-Type", "application/json;charset=UTF-8");
      xmlhttp.send(this.books);
    },
    deleteBook(index) {
      this.books.splice(index, 1);
    },
    getAllBooks() {
      this.loading = true;
      fetch('http://localhost:3000/books')
      .then(response => response.json())
      .then(data => {
        this.books = data;
        this.loading = false;
      });
    }
  },
  created: function() {
    this.getAllBooks();
  },
}

</script>

<style>
  h1, h2 {
    font-weight: normal;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
</style>