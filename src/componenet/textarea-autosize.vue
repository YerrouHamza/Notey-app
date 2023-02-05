<script setup>
    import { onMounted } from "vue";
    
    const props = defineProps({
        modelValue: String
    })

    const emit = defineEmits(['update:modelValue'])

    const resizeFunction = (e) => {
        // function for re size the textarea height
        e.target.style.height = `auto`;
        e.target.style.height = `${e.target.scrollHeight}px`;
    }

    const resize = (e) => {
        emit('update:modelValue', e.target.value) // emit the update function

        resizeFunction(e) // call the resize textarea height function
    }
    const targetElement = document.getElementsByTagName('textarea');

    onMounted(() => {
        targetElement[0].style.height = `auto`;
        targetElement[0].style.height = `${targetElement[0].scrollHeight}px`;
        targetElement[1].style.height = `auto`;
        targetElement[1].style.height = `${targetElement[1].scrollHeight}px`;
    
        // targetElement.map(item => {
        //     item.style.height = `auto`;
        //     item.style.height = `${item.scrollHeight}px`;
        // })
    });

</script>

<template>
    <textarea
        ref="el"
        @input="resize($event)"
        
    >{{ props.modelValue }}</textarea>
</template>