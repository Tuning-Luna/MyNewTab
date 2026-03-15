<script setup lang="ts">
import { ref, onMounted } from 'vue';

const logoRef = ref<HTMLDivElement | null>(null);
const typingRef = ref<HTMLDivElement | null>(null);

const TEXT = "Always Learning, Always building.";
const TYPING_SPEED = 130;

function typeWriter(el: HTMLElement) {
  let index = 0;
  let pausing = false;

  setInterval(() => {
    if (pausing) return;
    el.textContent = TEXT.slice(0, index++);
    if (index > TEXT.length) {
      pausing = true;
      setTimeout(() => { index = 0; pausing = false; }, 2000);
    }
  }, TYPING_SPEED);
}

onMounted(() => {
  const logo = logoRef.value;
  if (!logo) return;

  logo.addEventListener('mousemove', (e) => {
    const { left, top, width, height } = logo.getBoundingClientRect();
    const px = (e.clientX - left - width / 2) / (width / 2);
    const py = (e.clientY - top - height / 2) / (height / 2);
    logo.style.setProperty('--rotateX', `${-30 * py}deg`);
    logo.style.setProperty('--rotateY', `${30 * px}deg`);
  });

  logo.addEventListener('mouseleave', () => {
    logo.style.setProperty('--rotateX', '0deg');
    logo.style.setProperty('--rotateY', '0deg');
  });

  if (typingRef.value) typeWriter(typingRef.value);
});

function goGoogle() {
  window.location.href = 'https://www.google.com';
}
</script>

<template>
  <div class="main">
    <div class="logo" ref="logoRef">
      <img class="google" src="./assets/google_logo.svg" alt="logo" @click="goGoogle" />
      <div class="avatar">
        <img src="./assets/Tuning.png" alt="" />
      </div>
    </div>
    <div class="typing" ref="typingRef"></div>
  </div>
</template>

<style scoped lang="scss">
.main {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: url('./assets/earch.jpg') no-repeat center center / cover;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding-top: 20vh;
  perspective: 1000px;

  &::before {
    content: '';
    position: absolute;
    inset: 0;
    background:
      radial-gradient(circle at 30% 30%, rgba(0, 80, 255, 0.25), transparent 50%),
      radial-gradient(circle at 70% 70%, rgba(255, 0, 150, 0.2), transparent 50%);
    animation: nebulaMove 20s linear infinite alternate;
    z-index: 0;
  }

  &::after {
    content: '';
    position: absolute;
    inset: 0;
    background:
      radial-gradient(4px 4px at 5% 10%, rgba(220, 235, 255, 1), transparent),
      radial-gradient(3px 3px at 15% 35%, rgba(255, 240, 250, 0.98), transparent),
      radial-gradient(4.5px 4.5px at 25% 60%, rgba(210, 230, 255, 1), transparent),
      radial-gradient(3px 3px at 35% 20%, rgba(245, 230, 255, 0.98), transparent),
      radial-gradient(2.5px 2.5px at 45% 50%, rgba(235, 245, 255, 0.95), transparent),
      radial-gradient(4px 4px at 55% 80%, rgba(255, 245, 250, 1), transparent),
      radial-gradient(3px 3px at 65% 30%, rgba(220, 235, 255, 1), transparent),
      radial-gradient(2.5px 2.5px at 75% 70%, rgba(240, 250, 255, 0.95), transparent),
      radial-gradient(4px 4px at 85% 40%, rgba(255, 240, 250, 1), transparent),
      radial-gradient(2.5px 2.5px at 95% 90%, rgba(230, 240, 255, 0.95), transparent),
      radial-gradient(3.2px 3.2px at 10% 50%, rgba(210, 230, 255, 1), transparent),
      radial-gradient(4px 4px at 20% 80%, rgba(230, 245, 255, 1), transparent),
      radial-gradient(2.8px 2.8px at 30% 15%, rgba(255, 235, 250, 0.98), transparent),
      radial-gradient(3.5px 3.5px at 40% 65%, rgba(210, 230, 255, 1), transparent),
      radial-gradient(4.3px 4.3px at 50% 25%, rgba(220, 235, 255, 1), transparent),
      radial-gradient(2.5px 2.5px at 60% 75%, rgba(255, 240, 250, 0.95), transparent),
      radial-gradient(4px 4px at 70% 10%, rgba(210, 230, 255, 1), transparent),
      radial-gradient(3px 3px at 80% 55%, rgba(230, 245, 255, 1), transparent),
      radial-gradient(4.5px 4.5px at 90% 30%, rgba(255, 245, 250, 1), transparent),
      radial-gradient(3px 3px at 0% 70%, rgba(220, 235, 255, 1), transparent),
      radial-gradient(2.8px 2.8px at 12% 90%, rgba(235, 230, 255, 0.95), transparent),
      radial-gradient(3.5px 3.5px at 22% 5%, rgba(255, 235, 245, 0.98), transparent),
      radial-gradient(4px 4px at 32% 45%, rgba(210, 230, 255, 1), transparent),
      radial-gradient(2.5px 2.5px at 42% 85%, rgba(220, 235, 255, 1), transparent),
      radial-gradient(4.3px 4.3px at 52% 20%, rgba(255, 240, 250, 1), transparent),
      radial-gradient(3px 3px at 62% 60%, rgba(230, 245, 255, 1), transparent),
      radial-gradient(4px 4px at 72% 95%, rgba(255, 245, 250, 1), transparent),
      radial-gradient(3.5px 3.5px at 82% 35%, rgba(220, 235, 255, 1), transparent),
      radial-gradient(4.5px 4.5px at 92% 75%, rgba(235, 245, 255, 1), transparent);
    background-repeat: no-repeat;
    filter: brightness(1.8) blur(0.3px);
    animation: starsMove 25s linear infinite;
    z-index: 0;
  }
}

.logo {
  position: relative;
  z-index: 2;
  --rotateX: 0deg;
  --rotateY: 0deg;
  transform-style: preserve-3d;

  .google {
    width: 400px;
    height: auto;
    transition: all 0.4s ease;
    transform: rotateX(var(--rotateX)) rotateY(var(--rotateY));
    filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.4));
    animation: idle 10s linear infinite;
    transform-style: preserve-3d;
    will-change: transform;

    &:hover {
      transform: scale(1.1) rotateX(var(--rotateX)) rotateY(var(--rotateY)) translateZ(-25px);
      filter:
        drop-shadow(0 0 6px rgba(0, 255, 255, 0.7)) drop-shadow(0 0 12px rgba(0, 200, 255, 0.5)) drop-shadow(0 0 20px rgba(0, 150, 255, 0.4));
    }
  }
}

// ✅ 移到 .main 下，用 absolute 定位相对于 .main，位置固定可靠
.typing {
  position: absolute;
  bottom: 12vh;
  left: 50%;
  transform: translateX(-50%);
  font-size: 2.8rem;
  color: #1f9797;
  font-family: monospace;
  white-space: nowrap;
  z-index: 2;
}

.avatar {
  position: absolute;
  bottom: -240px;
  left: 50%;
  transform: translateX(-50%);
  width: 200px;
  height: 200px;
  border-radius: 50%;
  overflow: visible;
  z-index: 3;


  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    border: 4px solid #00ffff;
    box-shadow: 0 0 15px rgba(0, 255, 255, 0.4);
    display: block;
    object-fit: cover; // ✅ 保持比例，裁剪填满圆形区域
    object-position: center; // 裁剪居中，可按需调整
  }

  // ✅ 用 inset 负值扩展，圆心始终与父元素中心对齐
  &::after {
    content: '';
    position: absolute;
    inset: -12px; // 向外扩展固定像素，不受宽高百分比影响
    border-radius: 50%;
    border: 2px solid rgba(0, 255, 255, 0.5);
    animation: ripple 2s infinite linear;
    pointer-events: none;
  }
}

@keyframes nebulaMove {
  from {
    background-position: 0% 0%, 100% 100%;
  }

  to {
    background-position: 100% 100%, 0% 0%;
  }
}

@keyframes starsMove {
  from {
    background-position: 0 0, 0 0, 0 0;
  }

  to {
    background-position: 1000px 600px, -800px 600px, 600px -400px;
  }
}

@keyframes idle {
  0% {
    transform: rotateX(10deg) rotateY(-10deg);
  }

  12% {
    transform: rotateX(0deg) rotateY(-10deg);
  }

  25% {
    transform: rotateX(-10deg) rotateY(-10deg);
  }

  38% {
    transform: rotateX(-10deg) rotateY(0deg);
  }

  50% {
    transform: rotateX(-10deg) rotateY(10deg);
  }

  62% {
    transform: rotateX(0deg) rotateY(10deg);
  }

  75% {
    transform: rotateX(10deg) rotateY(10deg);
  }

  87% {
    transform: rotateX(10deg) rotateY(0deg);
  }

  100% {
    transform: rotateX(10deg) rotateY(-10deg);
  }
}

@keyframes ripple {
  0% {
    inset: 0px;
    opacity: 1;
  }

  100% {
    inset: -40px;
    opacity: 0;
  }
}
</style>
