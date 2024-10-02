<template>
<div class="chat-status">
    <span class="chat-status_inner">
        {{ statusText }}
    </span>
</div>
</template>
<script setup>
import { defineProps, computed } from 'vue';
import { everyComponentProps } from '../everyComponentProps';

const props = defineProps({
    ...everyComponentProps
});

const statusText = computed(() => {
    const currentWriteList = props.chat.userChatActions
        .filter(id=>id !== props.user.id)
        .map(id=>{
            const user = props.users.find(user=>user.id === id);
            return user.name;
        });

    if (currentWriteList.length) {
        const endText = currentWriteList.length > 1 ? 'печатают' : 'печатает';
        return currentWriteList.join(', ') + ' ' + endText;
    }
   
    return '';
});


</script>
<style lang="scss">
@import './chat-status.scss';
</style>