<template>
  <div>
    <!-- Button to show/hide the  add note form -->
    <button type="button" class="btn btn-primary btn-lg" v-on:click="Show">+</button>
    <!-- Add note form card -->
    <div class="card shadow-sm border-dark mb-3 mx-auto m-3" style="max-width: 30rem;" v-show="ShowForm">
      <div class="card-header">
        Add Note
        <!-- Cancel button to hide the add note form -->
        <i class="far fa-times-circle position-absolute top-0 end-0 m-2 p-1" v-on:click="CancelNote"></i>
      </div>
      <div class="card-body text-dark">
        <form>
          <!-- Title input field -->
          <div class="form-group m-1">
            <label>Title</label>
            <input autofocus type="text" class="form-control m-1" v-model="Title" placeholder="Enter a title...">
          </div>
          <!-- Note textarea field -->
          <div class="form-group">
            <label>Note</label>
            <textarea class="form-control m-1" v-model="Note" placeholder="Write your note here..."></textarea>
          </div>
          <!-- Add note button -->
          <div class="form-group">
            <button type="button" class="btn btn-primary m-2" v-on:click="AddNote()">Add</button>
          </div>
        </form>
      </div>
    </div>
    <!-- Heading for the list of notes -->
    <h1 class="px-5 m-3 text-start">Notes Added</h1>
    <hr class=" m-3">
    <!-- List of notes -->
    <!--  -->
    <div class="d-flex flex-wrap m-5">
      <!-- Note card -->
      <div class="card border-dark mb-3 m-3" style="width: 300px;" v-for="(note, index) in Notes" :key="index" v-show="note.ShowNote">
        <!-- Note header -->
        <div class="card-header d-flex align-items-center justify-content-between" style="background-color: rgba(76, 78, 87, 0.198);">
          <!-- Note title -->
          <div>
            {{ note.Title }}
          </div>
          <!-- Edit and delete icons -->
          <div class="icons">
            <i class="fas fa-edit p-2" v-on:click="EditNote(index)"></i>
            <i class="fas fa-trash" v-on:click="DeleteNote(index)"></i>
          </div>
        </div>
        <!-- Note body -->
        <div class="card-body text-dark">
          <!-- Note text (truncated if too long) -->
          <p class="card-text" v-show="!note.showAll">{{ note.Note.substring(0, 100) }}{{ note.Note.length > 100 ? '...' : '' }}</p>
          <p class="card-text" v-show="note.showAll">{{ note.Note }}</p>
          <!-- Show more/less icons -->
          <div class="icon-text-wrapper" v-on:click="note.showAll = !note.showAll">
            <i :class="{'fas fa-chevron-down': !note.showAll, 'fas fa-chevron-up': note.showAll}"  ></i>
            <small>{{ note.showAll ? 'Show Less' : 'Show More' }}</small> 
          </div>
          <!-- Edit note form -->
          <div class="form-group">
            <form v-show="note.EditVisible">
              <hr>
              <!-- Title input field -->
              <div class="form-group m-1">
                <label>Title</label>
                <input type="text" class="form-control m-1" v-model="Title" placeholder="Enter a title...">
              </div>
              <!-- Note textarea field -->
              <div class="form-group">
                <label>Note</label>
                <textarea class="form-control m-1" v-model="Note" placeholder="Write your note here..."></textarea>
              </div>
              <!-- Save note button -->
              <div class="form-group">
                <button type="button" class="btn btn-primary m-2" v-on:click="UpdateNote(index) ">Save</button>
                <button type="button" class="btn btn-danger m-2" v-on:click="CancelEditNote(index) ">Cancel</button>
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
    name: 'NotesApp',
    data() {
      return {
        ShowForm: false, // Whether or not to show the add note form
        EditVisible: false, // Whether or not to show the edit note form
        Title: '', // Title of the note
        Note: '', // Text of the note
        Notes: [], // Array to store all the notes
        EditingIndex: null, // Index of the currently edited note
        ShowNote: true, // Whether or not to show the note card
        ShowAll: false, // Whether or not to show the full text of the note
        EditFlag: true // to control edit icon
      }
    },
    methods: {
      // Show the add note form
      Show() {
        this.ShowForm = true;
        this.Title = '';
        this.Note = '';
        this.EditingIndex = null;
      },
      // Cancel adding a note
      CancelNote() {
        this.ShowForm = false;
        this.Title = '';
        this.Note = '';
        this.EditingIndex = null;
      },
      // Add a new note
      AddNote() {
        const note = {
          Title: this.Title,
          Note: this.Note,
          EditVisible: false,
          ShowNote: true,
          ShowAll: false,
        };
        this.Notes.push(note);
        this.ShowForm = false;
        this.Title = '';
        this.Note = '';
      },
      // Show the edit note form
      EditNote(index) {
        if(this.EditFlag){
          this.Notes[index].EditVisible = true;
          this.Title = this.Notes[index].Title;
          this.Note = this.Notes[index].Note;
          this.EditingIndex = index;
        }
        this.EditFlag = false
      },
      // To cancel editing a note
      CancelEditNote(index){
        this.EditFlag = true
        this.Notes[index].EditVisible = false;
      },
      // Update an existing note
      UpdateNote(index) {
        this.Notes[this.EditingIndex].Title = this.Title;
        this.Notes[this.EditingIndex].Note = this.Note;
        this.isVisible = false;
        this.Title = '';
        this.Note = '';
        this.EditingIndex = null;
        this.Notes[index].EditVisible = false;
        this.EditFlag = true
      },
      // Delete a note
      DeleteNote(index) {
        console.log(index)
        this.Notes.splice(index, 1);
      },
    
    },
  };
</script>

<style scoped>
  .icons {
    margin-right: 10px;
  }

  .icons i {
    color: #6c757d;
    cursor: pointer;
  }

  .icons i:hover {
    color: #dc3545;
  }
  .icon-text-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>