<template>
    <div class="input-group">
        <input type="text" class="form-control" placeholder="Nhập thông tin cần tìm" :value="modelValue"
            @input="updateModelValue" @keyup.enter="submit" />
        <div class="input-group-append">
            <button class="btn btn-outline-secondary" type="button" @click="submit">
                <i class="fas fa-search"></i> Tìm kiếm
            </button>
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

// Định nghĩa props
const props = defineProps({
    modelValue: {
        type: String,
        default: ''
    }
});

// Định nghĩa emit để truyền sự kiện ra ngoài
const emit = defineEmits(['submit', 'update:modelValue']);

// Hàm cập nhật modelValue
const updateModelValue = (e) => {
    emit('update:modelValue', e.target.value);
};

// Hàm xử lý submit
const submit = () => {
    emit('submit');
};
</script>
