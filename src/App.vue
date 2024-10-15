<script setup lang="ts">
import { ref, watch, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import TableOfContents from './components/TableOfContents.vue';
import ChapterContent from './components/ChapterContent.vue';
import novelData from './assets/novel.json';

const route = useRoute();
const router = useRouter();

const isSidebarOpen = ref(false);
const currentChapter = ref(1);

const updateChapter = (chapterNumber: string | string[] | undefined) => {
  const newChapter = chapterNumber ? parseInt(chapterNumber as string, 10) : 1;
  if (newChapter >= 1 && newChapter <= novelData.chapters.length) {
    currentChapter.value = newChapter;
  } else {
    router.replace('/1');
  }
};

watch(() => route.params.chapterNumber, updateChapter, { immediate: true });

onMounted(() => {
  updateChapter(route.params.chapterNumber);
});

const toggleSidebar = () => {
  isSidebarOpen.value = !isSidebarOpen.value;
};

const selectChapter = (chapterNumber: number) => {
  router.push(`/${chapterNumber}`);
  isSidebarOpen.value = false;
};
</script>

<template>
  <div class="novel-website">
    <button @click="toggleSidebar" class="sidebar-toggle" :class="{ 'open': isSidebarOpen }">
      <span class="dots"></span>
    </button>
    <TableOfContents
      :isOpen="isSidebarOpen"
      :currentChapter="currentChapter"
      @selectChapter="selectChapter"
    />
    <ChapterContent :chapterNumber="currentChapter" />
  </div>
</template>

<style>
.novel-website {
  display: flex;
  height: 100vh;
}

.sidebar-toggle {
  position: fixed;
  top: 20px;
  left: 20px;
  z-index: 1000;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #ffffff;
  border: none;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.3s ease;
}

.sidebar-toggle:hover {
  background-color: #f0f0f0;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.sidebar-toggle .dots {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 4px;
  height: 16px;
}

.sidebar-toggle .dots::before,
.sidebar-toggle .dots::after,
.sidebar-toggle .dots {
  content: '';
  width: 4px;
  height: 4px;
  background-color: #333;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.sidebar-toggle:hover .dots::before,
.sidebar-toggle:hover .dots::after,
.sidebar-toggle:hover .dots {
  background-color: #000;
}

.sidebar-toggle.open {
  left: 270px;
}
</style>