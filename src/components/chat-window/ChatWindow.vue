<template>
    <div class="chat-window">
        <div class="chat-window_header">
            <chat-status
                :user="user"
                :chat="chat"
                :users="users"
            />
        </div>
        <div
            ref="chatContentWrapper"
            class="chat-window_body"
        >
            <div
                class="chat-window_body-inner"
                ref="chatContent"
            >
                <chat-messages
                    :user="user"
                    :chat="chat"
                    :users="users"
                />
            </div>
        </div>
        <div class="chat-window_footer">
            <chat-input
                @newMessage="newMessage"
                @onStartWrite="onStartWrite"
                @onStopWrite="onStopWrite"
            />
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits, nextTick, useTemplateRef } from 'vue';
import ChatStatus from './parts/chat-status/ChatStatus.vue';
import ChatMessages from './parts/chat-messages/ChatMessages.vue';
import ChatInput from './parts/chat-input/ChatInput.vue';
import { everyComponentProps } from './parts/everyComponentProps'

const props = defineProps({
    ...everyComponentProps
});

const emit = defineEmits(['updateChat']);
const chatContent = useTemplateRef('chatContent');
const chatContentWrapper = useTemplateRef('chatContentWrapper');

const onStartWrite = () => {
    const chat = {
        ...props.chat,
        userChatActions: [...new Set([...props.chat.userChatActions, props.user.id])]
    }
    
    emit('updateChat', chat);
}

const onStopWrite = () => {
    const chat = {
        ...props.chat,
        userChatActions: [...new Set(props.chat.userChatActions.filter(id=>id !== props.user.id))]
    }
    emit('updateChat', chat);
}

const newMessage = async (message) => {
    const chat = {
        ...props.chat,
        messages: [...props.chat.messages, {
            from: props.user.id,
            text: message,
            timestamp: new Date().getTime()
        }]
    }

    emit('updateChat', chat);

    await nextTick();

    chatContentWrapper.value.scrollTo({
        top: chatContent.value.getBoundingClientRect().height,
        left: 0,
        behavior: "smooth",
    });
}

</script>
<style lang="scss">
@import './chat-window.scss';
</style>