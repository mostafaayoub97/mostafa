<template>
  <div class="container">
    <div class="page-header">
      <h1>Vue.js 2 & Firebase <small>Sample Application by CodingTheSmartWay.com</small></h1>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Add New Books</h3>
      </div>
      <div class="panel-body">
         <form id="form" class="form-inline" v-on:submit.prevent="addBook()">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>
          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
          </div>
          <div class="form-group">
            <label for="bookUrl">Url:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Add Book">
        </form>
      </div>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Book List</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Author</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books" >
              <td><a v-bind:href="book.url">{{book.title}}</a></td>
              <td>{{book.author}}</td>
              <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
<script>

import Firebase from 'firebase'
import toastr from 'toastr'
 var firebaseConfig = {
    apiKey: "AIzaSyAqL_jwQfbfQbU9slnRjC0APNUvn_Bcnrw",
    authDomain: "book-app-8627c.firebaseapp.com",
    databaseURL: "https://book-app-8627c.firebaseio.com",
    projectId: "book-app-8627c",
    storageBucket: "",
    messagingSenderId: "326435208346",
    appId: "1:326435208346:web:e722f2762aea4712"
  };
  
let app = Firebase.initializeApp(firebaseConfig)
let db = app.database()
let booksRef = db.ref('books')
export default {
  name:'book',
 
  
  data () {
    return {
      books:[],
      newBook: {
          title: '',
          author: '',
          url: 'http://'
      },
     
    }
  },

   firebase: {
    books: booksRef
        },
  
   methods: {
      addBook: function () {
        booksRef.push(this.newBook);
        this.newBook.title = '';
        this.newBook.author = '';
        this.newBook.url = 'http://';
      },
      removeBook: function (book) {
        booksRef.child(book['.key']).remove()
        toastr.success('Book removed successfully')
      }
    },
  
 
}
</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
