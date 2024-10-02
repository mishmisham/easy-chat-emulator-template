<template>
<div class="input-textarea">
    <div class="input-textarea_inner">
        <div
            class="input-textarea_sizer"
            ref="sizer"
        >{{ model }}</div>
        <textarea
            v-model="model"
            @input="emitInput"
            @focus="emitFocus"
            @blur="emitBlur"
            :disabled="disabled"
            :placeholder="placeholder"
            :rows="rows"
            :spellcheck="false"
            :style="areaStyle"
            ref="inputTextarea"
            class="input-textarea_input"
        />
    </div>
</div>
</template>
<script setup>
import {
    defineProps,
    defineModel,
    defineEmits,
    defineExpose,
    useTemplateRef,
    computed,
    ref,
    onUpdated,
    onMounted
} from 'vue';

const props = defineProps({
    placeholder: {
        type: String,
        default: ''
    },
    disabled: {
        type: Boolean,
        default: false
    },
    rows: {
        type: Number,
        default: 5
    }
});

const emit = defineEmits(['update:value', 'input', 'focus', 'blur']);
const inputTextarea = useTemplateRef('inputTextarea');
const sizer = useTemplateRef('sizer');
const model = defineModel({ default: '' });
const textareaHeight = ref(100);

const areaStyle = computed(() => {
    return `height:${textareaHeight.value}px;`;
});

const refreshTextareaSize = () => {
    if (!sizer.value) {
        return;
    }

    const { height } = sizer.value.getBoundingClientRect();
    textareaHeight.value = height;
}

const focus = () => {
    if (!inputTextarea.value) {
        return;
    }
    inputTextarea.value.focus();
}

const blur = () => {
    if (!inputTextarea.value) {
        return;
    }
    inputTextarea.value.blur();
}

const emitFocus = () => emit('focus');
const emitBlur = () => emit('blur');
const emitInput = (e) => emit('input', e);

onMounted(() => refreshTextareaSize());
onUpdated(() => refreshTextareaSize());

defineExpose({
    focus,
    blur
});
</script>
<style lang="scss">
@import './textarea-input.scss';
</style>