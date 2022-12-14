<script setup>
import Modal from "./components/Modal.vue";
import {Icon} from "@iconify/vue";
import {reactive, ref} from "vue";
import {projects} from "./utils/dummy-data.js";

// refもreactiveと同様にリアクティブ変数を生成するvueの機能です。
const customModalShow = ref(false);

// サイドメニュー/プロジェクト
const dummyProjects = reactive(projects);
const newProjectModalShow = ref(false);
const projectNameError = ref('');

const newProject = reactive({
  name: '',
  isImportant: false,
  isPrivate: false
});
const openNew = () => {
  projectNameError.value = '';
  newProject.name = '';
  newProject.isImportant = newProject.isPrivate = false;
  newProjectModalShow.value = true;
};
const createProject = () => {
  projectNameError.value = '';
  if (newProject.name.trim() === '') {
    projectNameError.value = '入力してください';
  }
  dummyProjects.push({name: newProject.name, isImportant: newProject.isImportant, isPrivate: newProject.isPrivate});
  newProjectModalShow.value = false;
};
</script>

<template>
  <nav class="basis-60 h-full border-r p-3 shadow flex flex-col">
    <h3 class="mb-5">サイドメニューです</h3>
    <div>
      <input type="text">
    </div>
    <div class="flex flex-row items-center mb-5">
      <div class="flex-1">
        <button class="hover:bg-gray-50 border rounded-full w-16 h-16">
          <Icon class="mx-auto" icon="ic:outline-home" width="32" height="32"/>
        </button>
      </div>
      <div class="flex-1">
        <button class="hover:bg-gray-50 border rounded-full w-16 h-16">
          <Icon class="mx-auto" icon="ic:outline-add-alert" width="32" height="32"/>
        </button>
      </div>
      <div class="flex-1">
        <button class="hover:bg-gray-50 border rounded-full w-16 h-16">
          <Icon class="mx-auto" icon="ic:outline-settings" width="32" height="32"/>
        </button>
      </div>
    </div>
    <div class="flex flex-row items-center justify-between mb-3">
      <h4>プロジェクト管理</h4>
      <button class="bg-blue-500 hover:bg-blue-400 text-white text-sm rounded p-2" @click="openNew">追加</button>
    </div>
    <ul class="flex-1 overflow-y-auto">
      <li v-for="(project, index) of dummyProjects" :key="project.name + index">
        <div class="flex flex-row items-center mb-2 border border-2 border-l-8 rounded-r px-2 py-4 cursor-pointer"
             :class="project.isImportant ? 'border-l-orange-500' : 'border-l-blue-500'">
          <span class="flex-1">{{ project.name }}</span>
          <Icon v-if="project.isPrivate" icon="ic:baseline-key"/>
        </div>
      </li>
    </ul>
  </nav>
  <div class="flex-1 h-full flex flex-col">
    <header class="basis-20 p-3 bg-blue-50">
      <h1>モーダルサンプル</h1>
      <h3>ヘッダーです</h3>
    </header>
    <main class="flex-1 p-3 bg-purple-50 overflow-y-auto">
      <h2 class="flex flex-row items-center gap-1.5">
        <Icon icon="ic:baseline-apple" class="text-sm"/>
        <Icon icon="ic:baseline-apple"/>
        <Icon icon="ic:baseline-apple" class="text-lg"/>
        メインです
      </h2>
      <button class="bg-green-500 hover:bg-green-400 text-white rounded px-4 py-2" @click="customModalShow = true">カスタムモーダルを開く</button>
    </main>
  </div>
  <Modal v-model="newProjectModalShow">
    <template v-slot:header>
      <h4 class="text-lg font-bold px-2">新しいプロジェクトを作成する</h4>
    </template>
    <template v-slot:main>
      <div class="h-full px-2">
        <div class="flex flex-col gap-5">
          <div class="flex flex-col gap-1">
            <label for="project-name">プロジェクト名</label>
            <input type="text" id="project-name" v-model="newProject.name"
                   class="w-full h-12 border border-gray-300 rounded px-2 leading-loose outline outline-0 focus-visible:outline-2 focus-visible:outline-blue-500"
                   :class="{'border-red-400 focus-visible:outline-red-400': projectNameError}"/>
            <span v-if="projectNameError" class="text-red-500 text-sm">{{ projectNameError }}</span>
          </div>
          <div class="flex flex-row items-center gap-1">
            <input type="checkbox" id="project-is-important" class="border border-gray-300 rounded"
                   :checked="newProject.isImportant" v-model="newProject.isImportant">
            <label for="project-is-important">このプロジェクトは重要ですか</label>
          </div>
          <div class="flex flex-row items-center gap-1">
            <input type="checkbox" id="project-is-private" class="border border-gray-300 rounded"
                   :checked="newProject.isPrivate" v-model="newProject.isPrivate">
            <label for="project-is-private">このプロジェクトを公開しますか</label>
          </div>
        </div>
      </div>
    </template>
    <template v-slot:footer>
      <div class="flex flex-row gap-2 justify-end">
        <button class="bg-blue-500 hover:bg-blue-400 text-white rounded px-4 py-2" @click="createProject">作成</button>
        <button class="bg-gray-500 hover:bg-gray-400 text-white rounded px-4 py-2" @click="newProjectModalShow = false">閉じる</button>
      </div>
    </template>
  </Modal>
  <Modal v-model="customModalShow">
  </Modal>
</template>

