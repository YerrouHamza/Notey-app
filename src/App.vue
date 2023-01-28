<script setup>
  import { ref, watch, onMounted } from 'vue'

  const noteModal = ref(false);
  const noteEditModal = ref(false);
  const noteViewModal = ref(false);
  const noteErrorTextLenght = ref('');
  
  const newNote = ref();
  const notesState = ref([
    {
      value: 'Hello World',
      background: '#ffe4c4',
      id: 0,
      date: new Date()
    }
  ]);
  
  const targetNoteState = ref({}); // storage the target note {index, Target Note} 
  
  
  // function: save data on local storage
  const storeData = () =>{
    return localStorage.setItem('notes', JSON.stringify(notesState.value));
  }


  // function: before pqge load
  onMounted(() => {
    const storageData = JSON.parse(localStorage.getItem('notes'));
    
    storageData.forEach(state => {
      state.date = new Date(state.date)
    });

    notesState.value = storageData;
  });
  
  /* watch functions */
  watch(notesState.value, () => { // watch the main note
    storeData();
  })
  
  // open & close modal function
  const openModall = () => {
    newNote.value = ''
    noteModal.value = true
  }
  const closeModall = () => {
    noteModal.value = false
    noteEditModal.value = false
    noteViewModal.value = false;
    
    newNote.value = ''
  }
  
  // add new note function
  const addNewNote = () => {
    // newNote.value = '';
    notesState.value.push({
      value: newNote.value,
      date: new Date(),
      id: Math.floor(Math.random() * 100000000),
      background: "hsl(" + Math.random() * 360 + ", 100%, 75%)"
    })
    noteModal.value = false;
    newNote.value = '';    
    
    storeData(); // save data on local storage
  }
  
  
  // delete the note
  const deleteNote = (item) => {
    // notesState.value.splice(item.id, 1);
    const newNotesState = notesState.value.filter((items) => items !== item );
    notesState.value = newNotesState;

    storeData(); // save data on local storage
  }
  
  // edit the note (click on the edit button)
  const editNote = (item, index) => {
    noteEditModal.value = true;
    newNote.value = item.value;

    targetNoteState.value = {
      index: index,
      note: item
    };
  }
  
  // updated the note
  const updateNote = () => {
    const noteIndex = targetNoteState.value.index; // get the note index
    const noteTarget = targetNoteState.value.note; // get the note content

    notesState.value[noteIndex] = {
      value: newNote.value,
      background: noteTarget.background,
      date: noteTarget.date,
      id: noteTarget.id
    }
    noteEditModal.value = false;

    storeData(); // save data on local storage
  }

  // view note
  const viewNote = (item) => {
    noteViewModal.value = true

    targetNoteState.value = {
      value: item.value,
      background: item.background,
      date: item.date,
      id: item.id
    };
  }

</script>

<template>
  <!-- add note modal -->
  <Teleport to="body">
    <div class="modals" v-if="noteModal">
      <div class="add-note-modal">
        <textarea class="form-control" name="newNote" id="newNote" rows="10" v-model="newNote" autofocus></textarea>
        <p class="error-missage" v-if="noteErrorTextLenght">{{ noteErrorTextLenght }}</p>
        <button class="btn btn-full btn-primary" @click="addNewNote">Add Note</button>
        <button class="btn btn-close" @click="closeModall">x</button>
      </div>
    </div>
  </Teleport>

  <!-- update note modal -->
  <Teleport to="body">
    <div class="modals" v-if="noteEditModal">
      <div class="add-note-modal">
        <textarea class="form-control" name="newNote" id="newNote" rows="10" v-model="newNote" autofocus></textarea>
        <button class="btn btn-full btn-primary" @click="updateNote()">Update Note</button>
        <button class="btn btn-close" @click="closeModall">x</button>
      </div>
    </div>
  </Teleport>
  
  <!-- View note modal -->
  <Teleport to="body">
    <div class="modals" v-if="noteViewModal">
      <div class="view-note-modal" :style="{backgroundColor: targetNoteState.background}">
        <p>{{ targetNoteState.value }}</p>
        <button class="btn btn-close" @click="closeModall">x</button>
      </div>
    </div>
  </Teleport>
  
  <main>
    <header class="head">
      <h2 class="title">Note</h2>
      <buttom class="btn add-note btn-primary" @click="openModall" >+</buttom>
    </header>
    <!-- Note cards -->
    <div class="cards">
      <div v-for="(note, index) in notesState" :key="note.id" class="card" :style="{backgroundColor: note.background}">
        <p class="card-text">
          {{ note.value }}
        </p>
        <small class="cards-date">
          {{ note.date.toLocaleDateString('en-MR') }} 
          <span>
            ({{ note.date.toLocaleTimeString('en-US') }})
          </span>
        </small>
        <div class="icons-btn-group">
          <button class="btn-icon" @click="deleteNote(note)" :style="{backgroundColor: note.background}">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-trash-x" width="20" height="20" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
              <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
              <path d="M4 7h16" />
              <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />
              <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" />
              <path d="M10 12l4 4m0 -4l-4 4" />
            </svg>             
          </button>

          <button class="btn-icon" @click="editNote(note, index)" :style="{backgroundColor: note.background}">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-edit" width="20" height="20" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
              <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
              <path d="M7 7h-1a2 2 0 0 0 -2 2v9a2 2 0 0 0 2 2h9a2 2 0 0 0 2 -2v-1"></path>
              <path d="M20.385 6.585a2.1 2.1 0 0 0 -2.97 -2.97l-8.415 8.385v3h3l8.385 -8.415z"></path>
              <path d="M16 5l3 3"></path>
            </svg>          
          </button>

          <button class="btn-icon" @click="viewNote(note)" :style="{backgroundColor: note.background}">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-eye" width="20" height="20" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
              <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
              <circle cx="12" cy="12" r="2"></circle>
              <path d="M22 12c-2.667 4.667 -6 7 -10 7s-7.333 -2.333 -10 -7c2.667 -4.667 6 -7 10 -7s7.333 2.333 10 7"></path>
            </svg>          
          </button>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    width: 1000px;
    margin: 30px auto;
  }

  .head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 30px;
  }

  .head .title { /* the header titles */
    font-size: 3rem;
    font-weight: 600;
    margin: 0;
    font-family: var(--ft-family-primary);
  }

/* Buttons style */
  .btn {
    cursor: pointer;
    transition: all .3s ease-in-out;
    padding: 10px 20px;
  }
  .btn:hover {
    opacity: .8;
  }
  .btn:active {
    transform: scale(.9);
  }

  .btn-full {
    display: block;
    width: 100%;
  }

  .btn-full:active {
    transform: scale(1);
  }

  .btn-close {
    position: absolute;
    top: -20px;
    right: -20px;
    border-radius: 50%;
    border: none;
    width: 40px;
    height: 40px;
    font-size: 20px !important;
    padding: 0;
  }
  .btn-close:hover {
    background-color: rgb(229, 44, 44);
    opacity: 1;
    color: white;
  }

  .btn-primary {
    background-color: var(--cl-primary);
    color: black;
  }
  
  .add-note { /* add notes btn */
    font-size: 1.3rem;
    width: 50px;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    
    padding: 0;

    border-radius: 50%;
  }

  .icons-btn-group {
    text-align: center;
    position: absolute;
    width: 100%;
    left: 50%;
    top: 50%;
    z-index: 3;
    transform: translate(-50%);

    transition: display 1s ease-in-out;
    display: none;
  }

  .btn-icon {
    background-color: var(--cl-primary);
    border: none;
    border-radius: 50%;
    height: 35px;
    width: 35px;
    cursor: pointer;
    color: black;
    font-size: 0;
    margin: 0 2px;
  }
  .btn-icon:hover {
    opacity: .9;
  }

/* style for the modals */
  .modals {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content:center;
    align-items: center;
  }

  .modals::after {
    content: '';
    position: absolute;
    inset: 0;
    height: 100vh;
    width: 100vw;
    background-color: var(--color-text);
    opacity: .5;
    z-index: 2;
  }

  .add-note-modal {
    width: 800px;
    background-color: var(--color-background);
    padding: 25px;
    
    position: relative;
    inset: 0;
    z-index: 3;
  }

  .add-note-modal .btn {
    font-size: 1.4rem;
  }

  .view-note-modal {
    height: 90vh;
    width: 800px;
    padding: 25px;
    color: black !important;
    position: relative;
    inset: 0;
    z-index: 3;
  }

  /* inputes style */
  .form-control {
    width: 100%;
    font-size: 1.3rem;
  }
  .form-control:focus, .form-control:active, .form-control:focus-visible {
    border: 2px solid var(--cl-primary) !important;
    outline: none;
    resize: none;
  } 

</style>
