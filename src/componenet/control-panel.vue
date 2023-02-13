<script setup>
    import { ref, watch } from 'vue';

    const controlBtn = ref(true)
        
        
    const controlPanelBtn = () => {
        const panel = document.querySelector('#panel')
        panel.classList.toggle('open') // giv the panel open class
        controlBtn.value = !controlBtn.value // switch the controlBtn ref value
    }

    // theme mode functions
    const darkMode = (e) => {
        console.log()
        
        e.target.classList.add('active')
    }
    const lightMode = (e) => {
        console.log(e.target)
        e.target.classList.add('active')
        
    }
    const autoMode = (e) => {
        console.log(e.target)
        e.target.classList.add('active')
        
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

        <div class="content">
            <h2 class="panel-title">
                <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-settings" width="30" height="30" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                    <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                    <path d="M10.325 4.317c.426 -1.756 2.924 -1.756 3.35 0a1.724 1.724 0 0 0 2.573 1.066c1.543 -.94 3.31 .826 2.37 2.37a1.724 1.724 0 0 0 1.065 2.572c1.756 .426 1.756 2.924 0 3.35a1.724 1.724 0 0 0 -1.066 2.573c.94 1.543 -.826 3.31 -2.37 2.37a1.724 1.724 0 0 0 -2.572 1.065c-.426 1.756 -2.924 1.756 -3.35 0a1.724 1.724 0 0 0 -2.573 -1.066c-1.543 .94 -3.31 -.826 -2.37 -2.37a1.724 1.724 0 0 0 -1.065 -2.572c-1.756 -.426 -1.756 -2.924 0 -3.35a1.724 1.724 0 0 0 1.066 -2.573c-.94 -1.543 .826 -3.31 2.37 -2.37c1 .608 2.296 .07 2.572 -1.065z"></path>
                    <path d="M12 12m-3 0a3 3 0 1 0 6 0a3 3 0 1 0 -6 0"></path>
                </svg>
                Settings 
            </h2>

            <div class="form-group">
                <h3>Theme Mode</h3>
                <div class="btn-group">
                    <button class="btn mode-btn active" @click="darkMode($event)">
                        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-moon-stars" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                            <path d="M12 3c.132 0 .263 0 .393 0a7.5 7.5 0 0 0 7.92 12.446a9 9 0 1 1 -8.313 -12.454z"></path>
                            <path d="M17 4a2 2 0 0 0 2 2a2 2 0 0 0 -2 2a2 2 0 0 0 -2 -2a2 2 0 0 0 2 -2"></path>
                            <path d="M19 11h2m-1 -1v2"></path>
                        </svg>
                    </button>

                    <button class="btn mode-btn" @click="lightMode($event)">
                        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-sun-high" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                            <path d="M14.828 14.828a4 4 0 1 0 -5.656 -5.656a4 4 0 0 0 5.656 5.656z"></path>
                            <path d="M6.343 17.657l-1.414 1.414"></path>
                            <path d="M6.343 6.343l-1.414 -1.414"></path>
                            <path d="M17.657 6.343l1.414 -1.414"></path>
                            <path d="M17.657 17.657l1.414 1.414"></path>
                            <path d="M4 12h-2"></path>
                            <path d="M12 4v-2"></path>
                            <path d="M20 12h2"></path>
                            <path d="M12 20v2"></path>
                        </svg>
                    </button>

                    <button class="btn mode-btn" @click="autoMode($event)">
                        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-sun-moon" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
                            <path d="M9.173 14.83a4 4 0 1 1 5.657 -5.657"></path>
                            <path d="M11.294 12.707l.174 .247a7.5 7.5 0 0 0 8.845 2.492a9 9 0 0 1 -14.671 2.914"></path>
                            <path d="M3 12h1"></path>
                            <path d="M12 3v1"></path>
                            <path d="M5.6 5.6l.7 .7"></path>
                            <path d="M3 21l18 -18"></path>
                        </svg>
                    </button>
                </div>
            </div>
        </div>

        
    </div>
</template>
