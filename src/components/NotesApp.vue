<template>
  <nav class="navbar bg-body-tertiary shadow-sm p-3 mb-5 bg-body-tertiary" style="background-color: #2a4d69">
    <div class="container-fluid">
      <a class="navbar-brand text-white" href="">
        Notes App
      </a> 
      <!-- Button to show/hide the  add note form -->
      <button type="button" class="btn btn-outline-light  mx-3 px-3" v-on:click="showAddForm">+</button>
    </div>
  </nav>
  <div>
    <!-- Add note form card -->
    <div class="card shadow border-dark addCard" v-show="showForm">
      <div class="card-header cardHeader">
        Add Note
        <!-- Cancel button to hide the add note form -->
        <i class="far fa-times-circle position-absolute top-0 end-0 m-2 p-1" v-on:click="cancelAddNote"></i>
      </div>
      <div class="card-body text-dark">
        <form>
          <!-- title input field -->
          <div class="form-group m-1">
            <label>Title</label>
            <input autofocus type="text" class="form-control m-1" v-model="title" placeholder="Enter a title...">
          </div>
          <!-- Note textarea field -->
          <div class="form-group">
            <label>Note</label>
            <textarea class="form-control m-1" v-model="noteContent" placeholder="Write your note here..."></textarea>
          </div>
          <!-- Add note button -->
          <div class="form-group">
            <button type="button" class="btn btn-outline-secondary m-2" v-on:click="addNote">Add</button>
          </div>
        </form>
      </div>
    </div>
    <!-- List of notes -->
    <div class=" m-5 notesLayout flex-wrap d-flex">
      <!-- Note card -->
      <div class="card shadow border-dark mb-3 m-3 d-flex noteCard " v-for="(note, index) in notes" :key="index">
        <!-- Note header -->
        <div class="card-header d-flex align-items-center justify-content-between cardHeader">
          <!-- Note title -->
          <div>
            {{ note.title }}
          </div>
          <!-- Edit and delete icons -->
          <div class="icons">
            <i class="fas fa-edit p-2" v-on:click="editNote(index)"></i>
            <i class="fas fa-trash" v-on:click="deleteNote(index)"></i>
          </div>
        </div>
        <!-- Note body -->
        <div class="card-body text-dark overflow-auto">
          <!-- Note text (truncated if too long) -->
          <p class="card-text" v-show="!note.showFullNote">{{ note.noteContent.substring(0, 50) }}{{ note.noteContent.length > 50 ? '...' : '' }}</p>
          <p class="card-text" v-show="note.showFullNote">{{ note.noteContent }}</p>
          <!-- Show more/less icons -->
          <div class="iconTextWrapper" v-on:click="note.showFullNote = !note.showFullNote">
            <i  :class="{'fas fa-chevron-down': !note.showFullNote, 'fas fa-chevron-up': note.showFullNote}"  ></i>
            <small>{{ note.showFullNote ? 'Show Less' : 'Show More' }}</small> 
          </div>
          <!-- Edit note form -->
          <div class="form-group">
            <form v-show="note.editVisible">
              <hr>
              <!-- title input field -->
              <div class="form-group m-1">
                <label>title</label>
                <input type="text" class="form-control m-1" v-model="title" placeholder="Enter a title...">
              </div>
              <!-- Note textarea field -->
              <div class="form-group">
                <label>Note</label>
                <textarea class="form-control m-1" v-model="noteContent" placeholder="Write your note here..."></textarea>
              </div>
              <!-- Save note button -->
              <div class="form-group">
                <button type="button" class="btn btn-outline-secondary m-2" v-on:click="updateNote(index) ">Save</button>
                <button type="button" class="btn btn-outline-danger m-2" v-on:click="cancelEditNote(index) ">Cancel</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'notesApp',
    data() {
      return {
        showForm: false, // Whether or not to show the add note form
        editVisible: false, // Whether or not to show the edit note form
        title: '', // title of the note
        noteContent: '', // Text of the note
        notes: [], // Array to store all the notes
        editingIndex: null, // Index of the currently edited note
        showFullNote: false, // Whether or not to show the full text of the note
        editFlag: true, // to control edit icon
      }
    },
    methods: {
      onClear(){
        this.title = '';
        this.noteContent = '';
        this.editingIndex = null;
      },
      // Show the add note form
      showAddForm() {
        this.showForm = true;
        this.onClear();
        document.body.classList.add('blurrBackground');
      },
      // Cancel adding a note
      cancelAddNote() {
        this.showForm = false;
        this.onClear();
        document.body.classList.remove('blurrBackground');
      },
      // Add a new note
      addNote() {
        document.body.classList.remove('blurrBackground');
        const note = {
          title: this.title,
          noteContent: this.noteContent,
          editVisible: false,
          showFullNote: false,
        };
        this.notes.push(note);
        this.showForm = false;
        this.title = '';
        this.noteContent = '';
      },
      // Show the edit note form
      editNote(index) {
        if(this.editFlag){
          this.notes[index].editVisible = true;
          this.title = this.notes[index].title;
          this.noteContent = this.notes[index].noteContent;
          this.editingIndex = index;
        }
        this.editFlag = false
      },
      // To cancel editing a note
      cancelEditNote(index){
        this.editFlag = true
        this.notes[index].editVisible = false;
        
      },
      // Update an existing note
      updateNote(index) {
        this.notes[this.editingIndex].title = this.title;
        this.notes[this.editingIndex].noteContent = this.noteContent;
        this.isVisible = false;
        this.onClear();
        this.notes[index].editVisible = false;
        this.editFlag = true
      },
      // Delete a note
      deleteNote(index) {
        console.log(index)
        this.notes.splice(index, 1);
      },
      removeblurrBackground() {
        document.body.classList.remove('blurrBackground');
        this.showForm = false;
        this.title = '';
        this.noteContent = '';
      },
    
    },
  };
</script>

<style scoped>
  .icons {
    margin-right: 10px;
  }
  .icons i {
    color: #2a4d69;
    cursor: pointer;
  }
  .icons i:hover {
    color: #dc3545;
  }
  .iconTextWrapper {
    display: flex;
    flex-direction: column; 
  }
  .notesLayout{
    display: flex;
    flex-direction: row;
    align-items: start;
  }
  .addCard{
    z-index: 101;
    position: fixed;   
    top: 30%;
    left: 32%;
    width: 30rem;
  }
  .noteCard{
    width: 300px;
    height: 220px;
  }
  .cardHeader{
    background-color:#adcbe3a0;
  }
  nav{
    margin-bottom: 50px;
  }
  .blurrBackground .notesLayout {
    filter: blur(5px); 
  }
</style>