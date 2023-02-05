<script setup>
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


</script>

<template>
    <textarea
        ref="el"
        @input="resize($event)"
    >{{ props.modelValue }}</textarea>
</template>