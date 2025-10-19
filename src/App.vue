<script setup lang="ts">
import { ref, onMounted } from 'vue';

const logoRef = ref<HTMLDivElement | null>(null);
const typingRef = ref<HTMLDivElement | null>(null);

const text = "Always Learning,Always building.";
const typingSpeed = 130; // 打字间隔，单位 ms

function typeWriter(el: HTMLElement) {
  let index = 0;

  function type() {
    if (index <= text.length) {
      el.textContent = text.slice(0, index);
      index++;
      setTimeout(type, typingSpeed);
    } else {
      setTimeout(() => {
        index = 0;
        el.textContent = '';
        type();
      }, 2000); // 打字完成后等待 1 秒再重新开始
    }
  }

  type();
}

onMounted(() => {
  if (!logoRef.value) return;

  const logo = logoRef.value;

  logo.addEventListener('mousemove', (e) => {
    const rect = logo.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    const centerX = rect.width / 2;
    const centerY = rect.height / 2;
    const percentX = (x - centerX) / centerX;
    const percentY = (y - centerY) / centerY;

    const maxAngle = 30;
    const rotateY = maxAngle * percentX;
    const rotateX = -maxAngle * percentY;

    logo.style.setProperty('--rotateX', rotateX + 'deg');
    logo.style.setProperty('--rotateY', rotateY + 'deg');
  });

  logo.addEventListener('mouseleave', () => {
    logo.style.setProperty('--rotateX', '0deg');
    logo.style.setProperty('--rotateY', '0deg');
  });

  if (typingRef.value) {
    typeWriter(typingRef.value);
  }
});
</script>

<template>
  <div class="main">
    <div class="logo" ref="logoRef">
      <img class="google" src="./assets/google_logo.svg" alt="logo" />
      <div class="avatar">
        <img src="./assets/Tuning.png" alt="">
      </div>
      <!-- 打字机文本 -->
      <!-- <div class="typing" ref="typingRef"></div> -->
    </div>
  </div>

</template>

<style scoped lang="scss">
.main {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: url('./assets/earch.jpg') no-repeat center center;
  background-size: cover;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding-top: 20vh;
  perspective: 1000px;

  // 星云流动层
  &::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(circle at 30% 30%, rgba(0, 80, 255, 0.25), transparent 50%),
      radial-gradient(circle at 70% 70%, rgba(255, 0, 150, 0.2), transparent 50%);
    animation: nebulaMove 20s linear infinite alternate;
    z-index: 0;
  }

  // 星光漂移层
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
    /* 提高亮度，减少模糊 */
    opacity: 1;
    /* 完全不透明，恢复亮度 */
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
  transition: transform 0.3s ease;

  .google {
    width: 400px;
    height: auto;
    transition: all 0.4s ease;
    transform: rotateX(var(--rotateX)) rotateY(var(--rotateY));
    filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.4));
    animation: idle 10s linear infinite;
    transform-origin: center;
    transform-style: preserve-3d;
    will-change: transform;
  }

  .google:hover {
    transform: scale(1.1) rotateX(var(--rotateX)) rotateY(var(--rotateY)) translateZ(-25px);
    filter: drop-shadow(0 0 6px rgba(0, 255, 255, 0.7)) drop-shadow(0 0 12px rgba(0, 200, 255, 0.5)) drop-shadow(0 0 20px rgba(0, 150, 255, 0.4));
  }
}

.typing {
  position: absolute;
  top: 100%; // 放在头像下方
  left: 50%;
  transform: translate(-50%, 300px); // 下移 20px，居中
  font-size: 2.8rem;
  color: #1f9797;
  font-family: monospace;
  white-space: nowrap;
  overflow: hidden;
}

// 星云流动动画
@keyframes nebulaMove {
  0% {
    background-position: 0% 0%, 100% 100%;
  }

  100% {
    background-position: 100% 100%, 0% 0%;
  }
}

// 星光缓慢漂移
@keyframes starsMove {
  0% {
    background-position: 0 0, 0 0, 0 0;
  }

  100% {
    background-position: 1000px 600px, -800px 600px, 600px -400px;
  }
}


@keyframes idle {

  // 左上角
  0% {
    transform: rotateX(10deg) rotateY(-10deg);
  }

  // 过渡
  12% {
    transform: rotateX(0deg) rotateY(-10deg);
  }

  // 左下角
  25% {
    transform: rotateX(-10deg) rotateY(-10deg);
  }

  // 过渡
  38% {
    transform: rotateX(-10deg) rotateY(0deg);
  }

  // 右下角
  50% {
    transform: rotateX(-10deg) rotateY(10deg);
  }

  // 过渡
  62% {
    transform: rotateX(0deg) rotateY(10deg);
  }

  // 右上角
  75% {
    transform: rotateX(10deg) rotateY(10deg);
  }

  // 过渡
  87% {
    transform: rotateX(10deg) rotateY(0deg);
  }

  // 回到左上角
  100% {
    transform: rotateX(10deg) rotateY(-10deg);
  }
}

.avatar {
  position: absolute;
  bottom: -240px; // 调整到 logo 下方或者你想要的位置
  left: 50%;
  transform: translateX(-50%);
  width: 200px; // 头像大小
  height: 200px;
  object-fit: cover;
  border-radius: 50%; // 圆形
  overflow: visible;
  z-index: 3;

  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    border: 4px solid #00ffff; // 边框颜色
    box-shadow: 0 0 15px rgba(0, 255, 255, 0.4);
    display: block;
  }

  // 涟漪效果
  &::after {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    width: 120%;
    height: 120%;
    transform: translate(-50%, -50%);
    border-radius: 50%;
    border: 2px solid rgba(0, 255, 255, 0.5);
    animation: ripple 2s infinite linear;
    pointer-events: none;
  }
}

@keyframes ripple {
  0% {
    transform: translate(-50%, -50%) scale(0.8);
    opacity: 1;
  }

  70% {
    transform: translate(-50%, -50%) scale(1.2);
    opacity: 0.2;
  }

  100% {
    transform: translate(-50%, -50%) scale(1.5);
    opacity: 0;
  }
}
</style>
