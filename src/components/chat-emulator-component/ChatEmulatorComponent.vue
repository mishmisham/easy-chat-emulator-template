<template>
    <section class="chat-emulator-component">
        <chat-window
            @updateChat="updateChat"
            v-for="(user, key) of users"
            :user="user"
            :users="users"
            :chat="chat"
            :key="key"
        />
    </section>
</template>

<script setup>
import { reactive } from 'vue';
import { userMock } from './userMock.js';
import { chatMock } from './chatMock.js';
import ChatWindow from '@/components/chat-window/ChatWindow';

const users = reactive(userMock);
const chat = reactive(chatMock);

const updateChat = (updatedChat) => {
    Object.assign(chat, updatedChat);
}

</script>

<style lang="scss">
@import './chat-emulator-component.scss';
</style>