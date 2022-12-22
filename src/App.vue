<script setup>
import Modal from "./components/Modal.vue";
import {Icon} from "@iconify/vue";
import {reactive, ref} from "vue";
import {projects} from "./utils/dummy-data.js";
import DropdownMenu from '@innologica/vue-dropdown-menu'

// refもreactiveと同様にリアクティブ変数を生成するvueの機能です。


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
  <div id="container">
    <div id="sidebar">
      <div id="sidebar-top">
        <div class="search-box">
            <input type="submit" name="submit" value="" />
            <input
              type="search"
              name="search"
              placeholder="ノート検索（全体）"
              color="#A5A5A5"
            />
          </div>
          <div id="sidebar-top-button">
            <a href="#" class="sidebar-top-button-item">
              <div class="sidebar-top-button-icon"></div>
              <p class="icon-title">通知</p>
            </a>
​
            <a href="#" class="sidebar-top-button-item">
              <div class="sidebar-top-button-icon"></div>
              <p class="icon-title">お気に入り</p></a
            >
            <a href="#" class="sidebar-top-button-item">
              <div class="sidebar-top-button-icon"></div>
              <p class="icon-title">タスク管理</p></a
            >
          </div>
      </div>
      <div id="sidebar-bottom">
        <div class="sidebar-bottom-title">グループ</div>
        <a href="#" class="group-box">
            <div class="group-name">aaa</div>
          </a>
          <div><a href="#" class="group-link">グループ詳細・管理 </a></div>
          <div> <a href="#" class="group-link">新着ノート </a></div>
          <div class="folder-title">
            <p class="sidebar-bottom-title">フォルダ一覧</p>
          <button class="sidebar-button" @click="openNew">フォルダ作成</button>
        </div>

   <ul class="folder-box-section">
     <li class="border-l-8 border border-blue-200 cursor-pointer" :class="project.isImportant ? 'border-l-orange-500' : 'border-l-blue-500'" v-for="(project, index) of dummyProjects" :key="project.name + index">

         <p class="folder-box-title">{{ project.name }}</p>
         <Icon v-if="project.isPrivate" icon="ic:baseline-key"/>
     </li>
   </ul>

      </div>
    </div>
    <div id="main">
      <div id="main-header">
        <p>Qiita記事を表示</p>
      </div>
      <div id="main-body">
        <nav>
          <li><button @click="openNew">タスク作成</button></li>
          <li></li>
          <li></li>
          <li></li>
        </nav>
        <div id="main-box">
          <div class="main-box-header"></div>
          <div class="main-box-body"></div>
        </div>
      </div>
    </div>
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

</template>
<style>
#container {
  display: flex;
  width: 100%;
  height: 100%;
}
#sidebar {
  width: 220px;
  height: 100%;
  filter: drop-shadow(2px 0px 10px rgba(75, 75, 75, 0.25));
}
#sidebar-top {
  position: relative;
  height: 206px;
  width: 100%;
  background-color: #f1f7ff;
}
.search-box {
  position: absolute;
  display: flex;
  width: 196px;
  height: 32px;
  left: 12px;
  top: 72px;
}
input[type="submit"] {
  height: 32px;
  width: 32px;
  background: #0f50ba;
  border: 1px solid #0f50ba;
  border-radius: 3px 0px 0px 3px;
}
input[type="search"] {
  height: 32px;
  width: 166px;
  background-color: #ffffff;
  border: 1px solid #0f50ba;
  font-size: 10px;
  border-radius: 0px 3px 3px 0px;
}
#sidebar-top-button {
  position: absolute;
  display: flex;
  height: 68px;
  width: calc(64px * 3);
  top: 120px;
  left: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.sidebar-top-button-item {
  width: 64px;
  height: 68px;
  display: block;

}
.sidebar-top-button-item > p {
  color: #545963;
}
.sidebar-top-button-icon {
  width: 44px;
  height: 44px;
  background-color: #ffffff;
  border-radius: 22px;
  border: 1px solid #bfcfe3;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
}
.sidebar-top-button-icon:hover {
  background-color: #0f50ba;
}
.icon-title {
  text-align: center;
  color: #545963;
  font-size: 10px;
  font-weight: 700;
  height: 24px;
  align-items: center;
  display: flex;
  justify-content: center;
}
#sidebar-bottom {
  position: relative;
  height: 724px;
  width: 100%;
  background-color: #e7f1ff;
}
.sidebar-bottom-title {
  width: 196px;
  height: 24px;
  top: 10px;
  left: 12px;
  font-weight: 700;
  font-size: 10px;
  vertical-align: middle;
  display: flex;
  align-items: center;
}
.group-box {
  display: block;
  left: 12px;
  top: 44px;
  width: 196px;
  height: 40px;
  border: 1px solid #0f50ba;
  border-radius: 3px;
  margin-left: 12px;
}
.group-name {
  padding-left: 12px;
  display: flex;
  align-items: center;
  position: relative;
  height: 100%;
  font-weight: 700;
  font-size: 14px;
  color: #0f50ba;
  border-radius: 3px;
  background-color: #ffffff;
  
}
.group-name::after {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  right: 0px;
  top: 0px;
  width: 30px;
  height: 100%;
  background: #d7e8ff;
  border-radius: 0px 3px 3px 0px;
  content: ">";
}
.group-link {
  
  top: 94px;
  display: block;
  align-items: center;
  width: 220px;
  height: 30px;
  font-size: 12px;
  font-weight: 400;
  color: #0f50ba;
}
.sidebar-button {
  width: 93px;
  height: 20px;
  background: #D7E8FF;
  border: 1px solid #BFCFE3;
  border-radius: 10px;
  font-weight: 700;
  font-size: 10px;
  text-align: center;
  color: #0F50BA;
}
.folder-title {
  display: flex;
}
.folder-box-section {
  flex: 1 1 0%;
  overflow-y: auto;
}
.folder-box-section > li {
  width: 196px;
  height: 42px;
  background: #FFFFFF;
/* border-color: #BFCFE3; */
border-radius: 3px;
margin: 12px 12px 8px 12px;
align-items: center;
display: flex;
justify-content: center;

}
.folder-box-title {
  width: 152px;
height: 24px;
font-weight: 400;
font-size: 14px;
color: #0F50BA;




}


#main {
  width: calc(100% - 220px);
  height: 100%;
}
#main-header {
  width: 100%;
  height: 50px;
  background-color: #f1f7ff;
  border-bottom: 1px solid #bfcfe3;
}
#main-body {
  position: relative;
  height: calc(100% - 50px);
  padding: 27px 16px 32px 32px;
}
#main-box {
  position: absolute;
  width: 1172px;
  height: 747px;
  top: 101px;
  background-color: #ffffff;
  border: 1px solid #bfcfe3;
  border-radius: 3px 3px 0px 0px;
}
.main-box-header {
  height: 36px;
  width: 100%;
  background-color: #ebebeb;
}
</style>

