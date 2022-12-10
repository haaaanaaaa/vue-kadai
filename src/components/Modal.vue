<script setup>
defineProps({
  modelValue: Boolean
});

const emits = defineEmits(['update:modelValue']);
</script>

<template>
  <teleport to="body">
    <transition name="modal">
      <div v-if="modelValue" class="modal-mask">
        <div class="modal-container">
          <div class="basis-8">
            <slot name="header">モーダルのヘッダーです</slot>
          </div>
          <div class="flex-1 overflow-y-auto">
            <slot name="main">モーダルのメインです</slot>
          </div>
          <hr>
          <div class="basis-10">
            <slot name="footer">
              <div class="text-right">
                <button class="bg-gray-500 hover:bg-gray-400 text-white rounded px-4 py-2" @click="emits('update:modelValue', false)">
                  閉じる
                </button>
              </div>
            </slot>
          </div>
        </div>
      </div>
    </transition>
  </teleport>
</template>


<style scoped>
.modal-mask {
  @apply fixed top-0 left-0 w-full h-screen flex justify-center items-center bg-black bg-opacity-50;
  transition: opacity 0.3s ease;
}

.modal-container {
  @apply h-96 w-1/2 p-3 flex flex-col gap-3 bg-white;
  transition: all 0.3s ease;
}

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  transform: scale(1.1);
}
</style>