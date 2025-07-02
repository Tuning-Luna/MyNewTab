<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref, nextTick, } from 'vue'

const isSearchShow = ref(false)
const inputRef = ref<HTMLInputElement | null>(null)
const activeEngineIndex = ref(0)

const searchEngines = [
  { name: 'Bing', url: 'https://www.bing.com/search?q=' },
  { name: 'Google', url: 'https://www.google.com/search?q=' },
  { name: 'Bilibili', url: 'https://search.bilibili.com/all?keyword=' }
]

function handleKeyDown(e: KeyboardEvent) {
  // 如果输入框聚焦状态下
  if (document.activeElement === inputRef.value) {
    if (e.key === 'ArrowUp') {
      e.preventDefault()
      activeEngineIndex.value = (activeEngineIndex.value - 1 + searchEngines.length) % searchEngines.length
    }
    else if (e.key === 'ArrowDown') {
      e.preventDefault()
      activeEngineIndex.value = (activeEngineIndex.value + 1) % searchEngines.length
    }
    else if (e.key === 'Enter') {
      e.preventDefault()
      const keyword = inputRef.value?.value.trim()
      if (keyword) {
        // 保存上一次搜索记录
        // localStorage.setItem('search', keyword)
        const engine = searchEngines[activeEngineIndex.value]
        const url = engine.url + encodeURIComponent(keyword)
        window.location.href = url
      }
    }
    return
  }

  // 如果没有聚焦，按下 q 唤起搜索框
  if (e.key.toLowerCase() === 'q') {
    e.preventDefault()
    isSearchShow.value = !isSearchShow.value

    if (isSearchShow.value) {
      nextTick(() => {
        // 聚焦
        if (inputRef.value) {
          inputRef.value?.focus()
          // 如果有，恢复上一次的搜索记录
          // inputRef.value.value = localStorage.getItem('search') || ''
        }
      })
    }
  }
}


onMounted(() => {
  window.addEventListener('keydown', handleKeyDown)
})




onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeyDown)
})
</script>




<template>
  <div class="main">
    <transition name="fade">

      <div class="search" v-if="isSearchShow">

        <div class="engine-select" ref="engineSelectRef">
          <transition name="slide-fade" mode="out-in">
            <span :key="searchEngines[activeEngineIndex].name">
              {{ searchEngines[activeEngineIndex].name }}
            </span>
          </transition>
        </div>


        <input type="text" ref="inputRef" @blur="isSearchShow = false" />
      </div>
    </transition>
  </div>
</template>



<style scoped lang="scss">
.main {
  position: relative;
  width: 100vw;
  height: 100vh;
  background-image: url('./assets/Makima.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  overflow: hidden;
  color: white;

  &::before {
    content: '';
    position: absolute;
    inset: 0;
    background-color: rgba(0, 0, 0, 0);
    transition: background-color 0.3s ease-in-out;
    z-index: 1;
  }

  &:focus-within::before {
    background-color: rgba(0, 0, 0, 0.4);
  }

  .search {
    position: relative;
    z-index: 2;
    margin: 200px auto;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 12px;

    .engine-select {
      padding: 0 16px;
      background-color: rgba(255, 255, 255, 0.2);
      border-radius: 20px;
      height: 40px;
      display: flex;
      align-items: center;
      font-weight: bold;
      font-size: 20px;

    }

    input[type='text'] {
      background-color: rgba(#fff, 0.5);
      width: 500px;
      height: 40px;
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 20px;
      font-size: 14px;
      transition: all 0.3s ease-in-out;
      color: #fff;

      &:focus {
        outline: none;
        width: 625px;
        height: 50px;
        padding: 12px;
        padding-left: 15px;
        font-size: 20px;
        box-shadow: 0px 0px 14px 3px #409EFF;
      }
    }
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.3s ease;
  }

  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }

  .slide-fade-enter-active,
  .slide-fade-leave-active {
    transition: all 0.1s ease;
    display: inline-block;
  }

  .slide-fade-enter-from {
    opacity: 0;
    transform: translateY(20px);
  }

  .slide-fade-leave-to {
    opacity: 0;
    transform: translateY(-20px);
  }
}
</style>
