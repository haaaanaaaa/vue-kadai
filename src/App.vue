<script setup>
import {Icon} from "@iconify/vue";
import Modal from "./components/Modal.vue";
import {ref} from "vue";

// refもreactiveと同様にリアクティブ変数を生成するvueの機能です。
const defaultModalShow = ref(false);
const customModalShow = ref(false);
</script>

<template>
  <nav class="basis-60 h-full p-3 border-r shadow">
    <h3>サイドメニューです</h3>
  </nav>
  <div class="flex-1 h-full flex flex-col">
    <header class="basis-20 p-3 bg-blue-50">
      <h1>モーダルサンプル</h1>
      <h3>ヘッダーです</h3>
    </header>
    <main class="flex-1 p-3 bg-purple-50">
      <h2 class="flex flex-row items-center gap-1.5">
        <Icon icon="ic:baseline-apple" class="text-sm"/>
        <Icon icon="ic:baseline-apple"/>
        <Icon icon="ic:baseline-apple" class="text-lg"/>
        メインです
      </h2>
      <div class="flex flex-row gap-5">
        <button class="bg-blue-500 hover:bg-blue-400 text-white rounded px-4 py-2" @click="defaultModalShow = true">デフォルトモーダルを開く</button>
        <button class="bg-green-500 hover:bg-green-400 text-white rounded px-4 py-2" @click="customModalShow = true">カスタムモーダルを開く</button>
      </div>
    </main>
  </div>
  <Modal v-model="defaultModalShow"/>
  <Modal v-model="customModalShow">
    <template v-slot:header>
      <h4 class="bg-black text-lg text-white text-center">
        カスタムモーダルのヘッダーです
      </h4>
    </template>
    <template v-slot:main>
      <div class="h-full bg-amber-50 text-center">
        カスタムモーダルのメインです
      </div>
    </template>
    <template v-slot:footer>
      <div class="text-center">
        <button class="bg-red-500 hover:bg-red-400 text-white rounded px-4 py-2" @click="customModalShow = false">カスタムモーダルを閉じる</button>
      </div>
    </template>
  </Modal>
</template>

