<script setup>
  // import the component
  import textareaAutosize from './componenet/textarea-autosize.vue';
  import noData from './componenet/no-data.vue';

  import { ref, watch, onMounted } from 'vue'

  const noteModal = ref(false);
  const noteEditModal = ref(false);
  const noteViewModal = ref(false);
  const confirmModal = ref(false);
  
  const notesState = ref([
    {
      noteTitle: 'Hello World',
      noteText: 'Try it, Add new notes',
      background: '#ffe4c4',
      id: 0,
      date: new Date()
    }
  ]);
  const newNote = ref({});
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
    noteModal.value = false;
    noteEditModal.value = false;
    noteViewModal.value = false;
    confirmModal.value = false
    
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
    newNote.value.title = ''
    newNote.value.text = '' 
    
    storeData(); // save data on local storage
    closeModall() // close modals
  }
  
  
  // delete the note
  const deleteNote = (item) => {
    targetNoteState.value = item;
    confirmModal.value = true;
    
  }
  
  const confirmDelet = () => {
    const newNotesState = notesState.value.filter((items) => items !== targetNoteState.value );
    notesState.value = newNotesState;

    storeData(); // save data on local storage
    closeModall() // call function to close the modal
  }
  
  // edit the note (click on the edit button)
  const editNote = (item, index) => {
    noteEditModal.value = true;
    
    targetNoteState.value = {
      index: index,
      note: item
    };

    noteModal.value = false;
    noteViewModal.value = false;
  }
  
  // updated the note
  const updateNote = () => {
    const noteIndex = targetNoteState.value.index; // get the note index
    const noteTarget = targetNoteState.value.note; // get the note content

    notesState.value[noteIndex] = {
      noteTitle: noteTarget.noteTitle,
      noteText: noteTarget.noteText,
      background: noteTarget.background,
      date: noteTarget.date,
      id: noteTarget.id
    }

    storeData(); // save data on local storage
    closeModall() // close modals
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
  <!-- Modals -->
  <Teleport to="body">
    <!-- confirm modal -->
    <div class="modal" v-if="confirmModal">
      <div class="modal-container modal-confirm">
        <div class="modal-body">
          <h2 class="title">Are you sure?</h2>
          <p class="text">This action will delete this note one time</p>
        </div>
        
        <!-- close button -->
        <button class="btn btn-close" @click="closeModall">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-x" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M18 6l-12 12"></path>
            <path d="M6 6l12 12"></path>
          </svg>
        </button>
        
        <!-- Delete button -->
        <button class="btn btn-bottom btn-delete" @click="confirmDelet">
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
    
    <!-- add note modal -->
    <div class="modal" v-if="noteModal">
      <!-- Modal Conetent -->
      <div class="modal-container">
        <div class="modal-body">
          <textareaAutosize class="form-control note-title" name="noteTitle" id="addNoteTitle" v-model="newNote.title" placeholder="Note Title" autofocus></textareaAutosize>
          <textareaAutosize class="form-control note-text" name="noteText" id="addNoteText" v-model="newNote.text" placeholder="Write your note here"></textareaAutosize>
        </div>
        
        <!-- Close button-->
        <button class="btn btn-close" @click="closeModall">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-x" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M18 6l-12 12"></path>
            <path d="M6 6l12 12"></path>
          </svg>
        </button>

        <!-- Add note button -->
        <button class="btn btn-add btn-primary btn-bottom" @click="addNewNote">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-check" width="30" height="30" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M5 12l5 5l10 -10"></path>
          </svg>
        </button>
      </div>
      <!-- Modal Info -->
      <div class="modal-info">
        <div class="date">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-clock" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M12 12m-9 0a9 9 0 1 0 18 0a9 9 0 1 0 -18 0"></path>
            <path d="M12 7l0 5l3 3"></path>
          </svg> {{newNote.date.toDateString('en-MR')}} 
          <span>
            ( {{ newNote.date.toLocaleTimeString('en-US') }} )
          </span>
        </div>
      </div>
    </div>
    
    <!-- update note modal -->    
    <div class="modal" v-if="noteEditModal">
      <div class="modal-container">
        <div class="modal-body">
          <textareaAutosize class="form-control note-title" name="noteTitle" id="updateNoteTitle" v-model="targetNoteState.note.noteTitle" placeholder="Note Title" autofocus></textareaAutosize>
          <textareaAutosize class="form-control note-text" name="noteText" id="updateNoteText" v-model="targetNoteState.note.noteText" placeholder="Write your note here"></textareaAutosize>
        </div>
        
        <!-- Close button -->
        <button class="btn btn-close" @click="closeModall">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-x" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M18 6l-12 12"></path>
            <path d="M6 6l12 12"></path>
          </svg>
        </button>
        
        <!-- Update Button -->
        <button class="btn btn-update btn-primary btn-bottom" @click="updateNote()">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-check" width="30" height="30" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M5 12l5 5l10 -10"></path>
          </svg>
        </button>
      </div>
      <!-- Modal Info -->
      <div class="modal-info">
        <div class="date">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-clock" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M12 12m-9 0a9 9 0 1 0 18 0a9 9 0 1 0 -18 0"></path>
            <path d="M12 7l0 5l3 3"></path>
          </svg> {{targetNoteState.note.date.toDateString('en-MR')}} 
          <span>
            ( {{ targetNoteState.note.date.toLocaleTimeString('en-US') }} )
          </span>
        </div>
      </div>
    </div>
    
    <!-- View note modal -->
    <div class="modal" v-if="noteViewModal">
      <!-- Modal Conetent -->
      <div class="modal-container view-note-modal" :style="{backgroundColor: targetNoteState.note.background}">
        <div class="modal-body">
          <h1 v-if="targetNoteState.note.noteTitle">{{ targetNoteState.note.noteTitle }}</h1>
          <p v-if="targetNoteState.note.noteText">{{ targetNoteState.note.noteText }}</p>
        </div>
        
        <!-- close button -->
        <button class="btn btn-close" @click="closeModall">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-x" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M18 6l-12 12"></path>
            <path d="M6 6l12 12"></path>
          </svg>
        </button>
        
        <!-- Edit button -->
        <button class="btn btn-icon btn-bottom" @click="editNote(targetNoteState.note, targetNoteState.index)" :style="{backgroundColor: targetNoteState.note.background}">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-edit" width="20" height="20" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M7 7h-1a2 2 0 0 0 -2 2v9a2 2 0 0 0 2 2h9a2 2 0 0 0 2 -2v-1"></path>
            <path d="M20.385 6.585a2.1 2.1 0 0 0 -2.97 -2.97l-8.415 8.385v3h3l8.385 -8.415z"></path>
            <path d="M16 5l3 3"></path>
          </svg>          
        </button>
      </div>
      <!-- Modal Info -->
      <div class="modal-info">
        <div class="date">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-clock" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M12 12m-9 0a9 9 0 1 0 18 0a9 9 0 1 0 -18 0"></path>
            <path d="M12 7l0 5l3 3"></path>
          </svg> {{targetNoteState.note.date.toDateString('en-MR')}} 
          <span>
            ( {{ targetNoteState.note.date.toLocaleTimeString('en-US') }} )
          </span>
        </div>
      </div>
    </div>
  </Teleport>
  
  <main>
    <header class="head">
      <h2 class="title">Notey</h2>
      <buttom class="btn add-note btn-primary" @click="openModall" >
        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-plus" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
          <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
          <path d="M12 5l0 14"></path>
          <path d="M5 12l14 0"></path>
        </svg>
      </buttom>
    </header>

    <!--  -->

    <!-- Note cards -->
    <div class="cards">
      <noData />
      <div v-for="(note, index) in notesState" :key="note.id" class="card" :style="{backgroundColor: note.background}">
        <div class="card-content">
          <h3 class="card-title" v-if="note.noteTitle">
            {{ note.noteTitle }}
          </h3>
          <p class="card-text" v-if="note.noteText">
            {{ note.noteText }}
          </p>
        </div>
        <div class="card-info">
          <small class="cards-date">
            {{ note.date.toLocaleDateString('en-MR') }} 
            <span>
              ({{ note.date.toLocaleTimeString('en-US') }})
            </span>
          </small>
        </div>

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
  

</style>
