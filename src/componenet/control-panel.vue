<script setup>
    import { ref, watch } from 'vue';

    const controlBtn = ref(true)
        
        
    const controlPanelBtn = () => {
        const panel = document.querySelector('#panel')

        panel.classList.toggle('open') // giv the panel open class

        controlBtn.value = !controlBtn.value // switch the controlBtn ref value
    }
</script>

<template>
    <div id="panel" class="control-panel">
        <!-- open button -->
        <button class="btn panel-button" @click="controlPanelBtn($event)" v-if="controlBtn">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-home-edit" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                <path d="M9 21v-6a2 2 0 0 1 2 -2h2c.645 0 1.218 .305 1.584 .78"></path>
                <path d="M20 11l-8 -8l-9 9h2v7a2 2 0 0 0 2 2h4"></path>
                <path d="M18.42 15.61a2.1 2.1 0 0 1 2.97 2.97l-3.39 3.42h-3v-3l3.42 -3.39z"></path>
            </svg>
        </button>
        <!-- Close button -->
        <button class="btn panel-button close" @click="controlPanelBtn($event)" v-if="controlBtn == false">
            <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-x" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                <path d="M18 6l-12 12"></path>
                <path d="M6 6l12 12"></path>
            </svg>
        </button>

        
    </div>
</template>
