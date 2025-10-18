<script setup lang="ts">
import { ref, onMounted } from 'vue';

const logoRef = ref<HTMLDivElement | null>(null);

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
    const rotateX = -maxAngle * percentY; // 翻转 Y 方向更自然

    logo.style.setProperty('--rotateX', rotateX + 'deg');
    logo.style.setProperty('--rotateY', rotateY + 'deg');
  });

  logo.addEventListener('mouseleave', () => {
    logo.style.setProperty('--rotateX', '0deg');
    logo.style.setProperty('--rotateY', '0deg');
  });
});
</script>

<template>
  <div class="main">
    <div class="logo" ref="logoRef">
      <img class="google" src="./assets/google_logo.svg" alt="logo" />
      <div class="avatar">
        <img src="./assets/Tuning.png" alt="">
      </div>
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
  // 星光漂移层（密集版）
  &::after {
    content: '';
    position: absolute;
    inset: 0;
    background:
      radial-gradient(1.5px 1.5px at 5% 10%, white, transparent),
      radial-gradient(1.2px 1.2px at 15% 35%, #aaf, transparent),
      radial-gradient(1.8px 1.8px at 25% 60%, #aff, transparent),
      radial-gradient(1.2px 1.2px at 35% 20%, #ccf, transparent),
      radial-gradient(1px 1px at 45% 50%, #aaf, transparent),
      radial-gradient(1.5px 1.5px at 55% 80%, #aff, transparent),
      radial-gradient(1.2px 1.2px at 65% 30%, #ccf, transparent),
      radial-gradient(1px 1px at 75% 70%, #aaf, transparent),
      radial-gradient(1.5px 1.5px at 85% 40%, #aff, transparent),
      radial-gradient(1px 1px at 95% 90%, #ccf, transparent);
    background-repeat: no-repeat;
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
    transform-style: preserve-3d;
    transform: rotateX(var(--rotateX)) rotateY(var(--rotateY));
    filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.4));
    animation: idle 6s infinite linear alternate;
  }

  .google:hover {
    transform: scale(1.1) rotateX(var(--rotateX)) rotateY(var(--rotateY)) translateZ(-25px);
    filter: drop-shadow(0 0 6px rgba(0, 255, 255, 0.7)) drop-shadow(0 0 12px rgba(0, 200, 255, 0.5)) drop-shadow(0 0 20px rgba(0, 150, 255, 0.4));
  }
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
  0% {
    transform: rotateX(0deg) rotateY(0deg);
  }

  10% {
    transform: rotateX(0deg) rotateY(0deg);
  }

  20% {
    transform: rotateX(0deg) rotateY(0deg);
  }

  25% {
    transform: rotateX(5deg) rotateY(-5deg);
  }

  30% {
    transform: rotateX(8deg) rotateY(-8deg);
  }

  35% {
    transform: rotateX(5deg) rotateY(-5deg);
  }

  40% {
    transform: rotateX(0deg) rotateY(0deg);
  }

  45% {
    transform: rotateX(5deg) rotateY(5deg);
  }

  50% {
    transform: rotateX(8deg) rotateY(8deg);
  }

  55% {
    transform: rotateX(5deg) rotateY(5deg);
  }

  60% {
    transform: rotateX(0deg) rotateY(0deg);
  }

  65% {
    transform: rotateX(-5deg) rotateY(5deg);
  }

  70% {
    transform: rotateX(-8deg) rotateY(8deg);
  }

  75% {
    transform: rotateX(-5deg) rotateY(5deg);
  }

  80% {
    transform: rotateX(0deg) rotateY(0deg);
  }

  85% {
    transform: rotateX(-5deg) rotateY(-5deg);
  }

  90% {
    transform: rotateX(-8deg) rotateY(-8deg);
  }

  95% {
    transform: rotateX(-5deg) rotateY(-5deg);
  }

  100% {
    transform: rotateX(0deg) rotateY(0deg);
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
