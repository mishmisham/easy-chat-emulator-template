<template>
    <div class="chat-input">
        <textarea-input
            v-model="userMessage"
            @focus="onInputFocus"
            @blur="onInputBlur"
            @input="onStartWrite"
            placeholder="Введите сообщение и нажмите Enter"
        />

        <button
            @click="newMessage"
            class="chat-input_send">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
                <path d="M15.379,19.1403 L12.108,12.5993 L19.467,5.2413 L15.379,19.1403 Z M4.86,8.6213 L18.76,4.5343 L11.401,11.8923 L4.86,8.6213 Z M3.359,8.0213 C2.923,8.1493 2.87,8.7443 3.276,8.9483 L11.128,12.8733 L15.053,20.7243 C15.256,21.1303 15.852,21.0773 15.98,20.6413 L20.98,3.6413 C21.091,3.2623 20.739,2.9093 20.359,3.0213 L3.359,8.0213 Z"/>
            </svg>
        </button>
    </div>
</template>

<script setup>
import { ref, defineEmits, onMounted, nextTick, watch } from 'vue';
import TextareaInput from '@/components/input/text-input/TextareaInput.vue';

const emit = defineEmits([
    'onStartWrite',
    'onStopWrite',
    'newMessage'
]);

const userMessage = ref('');
const userMessageBackup = ref('');
const shiftPressed = ref(false);

const newMessage = async () => {
    if (!userMessage.value) {
        return;
    }

    emit('newMessage', userMessage.value);
    await nextTick();
    userMessage.value = '';
}

const onStartWrite = (newValue) => {
    setTimeout(() => {
        userMessageBackup.value = newValue;
    }, 500);

    emit('onStartWrite');

    setTimeout(() => {
        if (userMessage.value !== userMessageBackup.value) {
            return;
        }

        emit('onStopWrite');
    }, 1000);
}

const onInputFocus = () => {
    document.addEventListener('keyup', onEnterSendMessage);
}

const onInputBlur = () => {
    document.removeEventListener('keyup', onEnterSendMessage);
}

const onEnterSendMessage = (e) => {
    if (e.key === 'Enter' && !shiftPressed.value) {
        newMessage();
        emit('onStopWrite');
    }
}

watch([userMessage], (newValue, oldValue) => {
    if (newValue[0] && newValue[0] !== oldValue[0]) {
        onStartWrite(newValue[0]);
    }
});

onMounted(() => {
    document.addEventListener('keydown', e=>{
        if (e.shiftKey) {
            shiftPressed.value = true;
        }
    });
    document.addEventListener('keyup', e=>{
        if (e.shiftKey) {
            shiftPressed.value = false;
        }
    });
});

</script>
<style lang="scss">
@import './chat-input.scss';
</style>