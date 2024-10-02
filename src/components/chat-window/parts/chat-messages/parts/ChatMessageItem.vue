<template>
<div :class="{
        'chat-message-item': true,
        'chat-message-item--current': message[0].isCurrentUser,
        'chat-message-item--companion': !message[0].isCurrentUser,
    }">
    <div class="chat-message-item_content">
        <div class="chat-message-item_content-header">
            <div class="chat-message-item_content-user">
                <div class="chat-message-item_content-user-avatar">
                    <div class="chat-message-item_content-user-avatar-image">
                        <img :src="'/assets/'+message[0].user.avatar" />
                    </div>
                    <div v-if="message[0].user.online"
                        class="chat-message-item_content-user-online"></div>
                </div>
                <div class="chat-message-item_content-user-name">
                    {{ message[0].isCurrentUser ? 'Вы' : message[0].user.name }}
                </div>
            </div>
        </div>

        <div class="chat-message-item_content-body">
            <div 
                v-for="(item, key) of message"
                :key="key"
                class="chat-message-item_content-body-text"> 
                {{ item.message.text }}
            </div>
        </div>

        <div class="chat-message-item_content-footer">
            <div class="chat-message-item_content-footer-date">
                {{ getTime(message[message.length - 1].message.timestamp) }}
            </div>
        </div>
    </div>
</div>
</template>
<script setup>
import { defineProps, defineEmits, computed } from 'vue';
    
    const props = defineProps({
        message: {
            type: Array,
            default: () => ([])
        }
    });

    const getTime = (timestamp) => {
        const date = new Date(timestamp);
        const M = date.getMinutes().toString().padStart(2, '0');
        const H = date.getHours().toString().padStart(2, '0');
        const today = new Date();
        const isToday = today.getMonth() === date.getMonth()
                            && today.getDate() === date.getDate()
                            && today.getFullYear() === date.getFullYear();
        
        const time = `${H}:${M}`;
        if (isToday) {
            return time;
        }
        const d = date.getDate().toString().padStart(2, '0');
        const m = (1 + date.getMonth()).toString().padStart(2, '0');
       
        const yearPart = today.getFullYear() === date.getFullYear() ? '' : '.'+date.getFullYear();

        return `${d}.${m}${yearPart} ${time}`
    }
   
</script>
<style lang="scss">
    @import './chat-message-item.scss';
</style>