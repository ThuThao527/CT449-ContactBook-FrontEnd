<template>
<ul class="list-group">
<li class="list-group-item"
    v-for="(contact, index) in contacts"
    :key="contact._id"
    :class="{active: index === activeIndex}"
    @click="updateActiveIndex(index)">
    {{ contct.name }}
</li>
</ul>
</template>

<script setup>
import (defineProps, defineEmits) from 'vue';

const props = defineProps({
    contacts: {
        type : Array,
        default: () => []
    },
    activeIndex:{
        type: Number,
        default: -1
    }
});

const emit = defineEmits(['update:activeIndex']);

const updateActivateIndex = (index) =>{
    emit('update:activateIndex', index);
};
</script>