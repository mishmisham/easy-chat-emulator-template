<template>
<div class="chat-messages">
    <chat-message-item
        v-for="(message, i) in messages"
        :key="i"
        :message="message"
    />
</div>
</template>
<script setup>
import { defineProps, defineEmits, computed } from 'vue';
import ChatMessageItem from './parts/ChatMessageItem.vue';
import { everyComponentProps } from '../everyComponentProps';

const props = defineProps({
    ...everyComponentProps
});

const messages = computed(() => {
    const sorted = [...props.chat.messages].sort((a,b) => a.timestamp - b.timestamp);
    
    const chunked = [];

    sorted.forEach((message, i) => {
        const user = props.users.find(user => user.id === message.from);
        const patched = {
            message,
            user,
            isCurrentUser: user.id === props.user.id
        };

        const previous = chunked[chunked.length - 1];
        const messageFromPreviousUser = previous && previous[0].user.id === user.id;
        if (messageFromPreviousUser) {
            previous.push(patched);
        } else {
            chunked.push([patched]);
        }
    });

    return chunked;
})

</script>
<style lang="scss">
@import './chat-messages.scss';
</style>