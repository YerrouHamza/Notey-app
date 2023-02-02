<script setup>
  import { ref, watch, onMounted } from 'vue'

  const noteModal = ref(false);
  const noteEditModal = ref(false);
  const noteViewModal = ref(false);
  const noteErrorTextLenght = ref('');
  
  const newNote = ref({
    title: '',
    text: '',
    date: Number
  });
  const notesState = ref([
    {
      noteTitle: 'Hello World',
      noteText: 'Try it, Add new notes',
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
    if(localStorage.length == 0) {
      localStorage.setItem('notes', JSON.stringify(notesState.value));
    } else {
      const storageData = JSON.parse(localStorage.getItem('notes'));
      
      storageData.forEach(state => {
        state.date = new Date(state.date)
      });
  
      notesState.value = storageData;
    }
  });

  // localStorage.clear() // clear the local storage each time you updated the main state
  
  /* watch functions */
  watch(notesState.value, () => { // watch the main note
    storeData();
  })
  
  // open & close modal function
  const openModall = () => {
    newNote.value.date = new Date();

    newNote.value.title = ''
    newNote.value.text = ''
    noteModal.value = true
  }
  const closeModall = () => {
    noteModal.value = false
    noteEditModal.value = false
    noteViewModal.value = false;
    
    newNote.value.title = ''
    newNote.value.text = ''
  }
  
  // add new note function
  const addNewNote = () => {
    notesState.value.push({
      noteTitle: newNote.value.title,
      noteText: newNote.value.text,
      date: newNote.value.date,
      id: Math.floor(Math.random() * 100000000),
      background: "hsl(" + Math.random() * 360 + ", 100%, 75%)"
    })
    noteModal.value = false;
    newNote.value.title = ''
    newNote.value.text = '' 
    
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
    noteModal.value = false;
    noteViewModal.value = false;
    
    noteEditModal.value = true;
    newNote.value.title = item.noteTitle;
    newNote.value.text = item.noteText;
    
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
      noteTitle: newNote.value.title,
      noteText: newNote.value.text,
      background: noteTarget.background,
      date: noteTarget.date,
      id: noteTarget.id
    }
    noteEditModal.value = false;
    
    storeData(); // save data on local storage
  }
  
  // view note
  const viewNote = (item, index) => {
    noteViewModal.value = true;
    
    targetNoteState.value = {
      index: index,
      note: item
    };
  }

</script>

<template>
  <!-- add note modal -->
  <Teleport to="body">
    <div class="modals" v-if="noteModal">
      <div class="modal">
        <textarea class="form-control note-title" name="noteTitle" id="addNoteTitle" rows="10" v-model="newNote.title" placeholder="Note Title" autofocus></textarea>
        <textarea class="form-control note-text" name="noteText" id="addNoteText" rows="10" v-model="newNote.text" placeholder="Write your note here"></textarea>
        <p>{{newNote.date.toDateString('en-MR')}} 
          <span>
            ( {{ newNote.date.toLocaleTimeString('en-US') }} )
          </span>
        </p>
        
        <button class="btn btn-add btn-primary" @click="addNewNote">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-check" width="30" height="30" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M5 12l5 5l10 -10"></path>
          </svg>
        </button>
        <button class="btn btn-close" @click="closeModall">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-x" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M18 6l-12 12"></path>
            <path d="M6 6l12 12"></path>
          </svg>
        </button>
      </div>
    </div>
  </Teleport>

  <!-- update note modal -->
  <Teleport to="body">
    <div class="modals" v-if="noteEditModal">
      <div class="modal">
        <textarea class="form-control note-title" name="noteTitle" id="updateNoteTitle" rows="10" v-model="newNote.title" placeholder="Note Title" autofocus></textarea>
        <textarea class="form-control note-text" name="noteText" id="updateNoteText" rows="10" v-model="newNote.text" placeholder="Write your note here"></textarea>
        <button class="btn btn-update btn-primary" @click="updateNote()">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-check" width="30" height="30" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M5 12l5 5l10 -10"></path>
          </svg>
        </button>
        <button class="btn btn-close" @click="closeModall">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-x" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M18 6l-12 12"></path>
            <path d="M6 6l12 12"></path>
          </svg>
        </button>
      </div>
    </div>
  </Teleport>
  
  <!-- View note modal -->
  <Teleport to="body">
    <div class="modals" v-if="noteViewModal">
      <div class="modal view-note-modal" :style="{backgroundColor: targetNoteState.note.background}">
        <h1 v-if="targetNoteState.note.noteTitle">{{ targetNoteState.note.noteTitle }}</h1>
        <p v-if="targetNoteState.note.noteText">{{ targetNoteState.note.noteText }}</p>
        <button class="btn btn-close" @click="closeModall">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-x" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M18 6l-12 12"></path>
            <path d="M6 6l12 12"></path>
          </svg>
        </button>

          <button class="btn btn-icon" @click="editNote(targetNoteState.note, targetNoteState.index)" :style="{backgroundColor: targetNoteState.note.background}">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-edit" width="20" height="20" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
              <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
              <path d="M7 7h-1a2 2 0 0 0 -2 2v9a2 2 0 0 0 2 2h9a2 2 0 0 0 2 -2v-1"></path>
              <path d="M20.385 6.585a2.1 2.1 0 0 0 -2.97 -2.97l-8.415 8.385v3h3l8.385 -8.415z"></path>
              <path d="M16 5l3 3"></path>
            </svg>          
          </button>
      </div>
    </div>
  </Teleport>
  
  <main>
    <header class="head">
      <h2 class="title">Note</h2>
      <buttom class="btn add-note btn-primary" @click="openModall" >
        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-plus" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
          <path d="M12 5l0 14"></path>
          <path d="M5 12l14 0"></path>
        </svg>
      </buttom>
    </header>
    <!-- Note cards -->
    <div class="cards">
      <div v-for="(note, index) in notesState" :key="note.id" class="card" :style="{backgroundColor: note.background}">
        <div class="card-content">
          <h3 class="card-title" v-if="note.noteTitle">
            {{ note.noteTitle }}
          </h3>
          <p class="card-text" v-if="note.noteText">
            {{ note.noteText }}
          </p>
        </div>
        <small class="cards-date">
          {{ note.date.toLocaleDateString('en-MR') }} 
          <span>
            ({{ note.date.toLocaleTimeString('en-US') }})
          </span>
        </small>
        <div class="icons-btn-group">

          <!-- View button -->
          <button class="btn-icon" @click="viewNote(note, index)" :style="{backgroundColor: note.background}">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-eye" width="20" height="20" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
              <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
              <circle cx="12" cy="12" r="2"></circle>
              <path d="M22 12c-2.667 4.667 -6 7 -10 7s-7.333 -2.333 -10 -7c2.667 -4.667 6 -7 10 -7s7.333 2.333 10 7"></path>
            </svg>          
          </button>

          <!-- Edit button -->
          <button class="btn-icon" @click="editNote(note, index)" :style="{backgroundColor: note.background}">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-edit" width="20" height="20" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
              <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
              <path d="M7 7h-1a2 2 0 0 0 -2 2v9a2 2 0 0 0 2 2h9a2 2 0 0 0 2 -2v-1"></path>
              <path d="M20.385 6.585a2.1 2.1 0 0 0 -2.97 -2.97l-8.415 8.385v3h3l8.385 -8.415z"></path>
              <path d="M16 5l3 3"></path>
            </svg>          
          </button>

          <!-- Delete button -->
          <button class="btn-icon delete-btn" @click="deleteNote(note)" :style="{backgroundColor: note.background}">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-trash-x" width="20" height="20" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
              <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
              <path d="M4 7h16" />
              <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />
              <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" />
              <path d="M10 12l4 4m0 -4l-4 4" />
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
  }

</style>
