<template>
  <div class="stage" @click="active = !active">
    <!-- Фоновые сердца теперь более заметны -->
    <div class="ambient-bg">
      <div v-for="n in 8" :key="n" class="slow-heart" :style="heartPosition(n)">
        <svg viewBox="0 0 32 32" fill="none" stroke="#ffdae3" stroke-width="1.5">
          <path d="M16 28.5s-12-7.2-12-14.5c0-4 3.1-7 7-7 2.4 0 4.5 1.2 5 3.3.5-2.1 2.6-3.3 5-3.3 3.9 0 7 3 7 7 0 7.3-12 14.5-12 14.5z"/>
        </svg>
      </div>
    </div>

    <Transition name="fade-clean" mode="out-in">
      <!-- Состояние 1: Приветствие -->
      <div v-if="!active" key="idle" class="intro">
        <div class="minimal-heart">
          <svg viewBox="0 0 32 32"><path d="M16 28.5s-12-7.2-12-14.5c0-4 3.1-7 7-7 2.4 0 4.5 1.2 5 3.3.5-2.1 2.6-3.3 5-3.3 3.9 0 7 3 7 7 0 7.3-12 14.5-12 14.5z" fill="#ffb1c1"/></svg>
        </div>
        <p class="tap-hint">тыкни лапкой</p>
      </div>

      <!-- Состояние 2: Личное письмо -->
      <div v-else key="letter" class="letter-container">
        <div class="letter-content">
          <header class="letter-header">14 ФЕВРАЛЯ</header>
          <div class="text-block">
            <h1 class="greeting">Любимая Юля,</h1>
            <p class="paragraph">
              В суете дней легко забыть о самом главном, но когда я думаю о тебе, всё остальное уходит на второй план. Твоя нежность — это мой личный уютный мир, в котором мне всегда хорошо.
            </p>
            <p class="paragraph">
              Спасибо тебе за твой смех, за твою поддержку и за то, какая ты настоящая. Я хочу, чтобы ты знала: ты — самое прекрасное, что случилось в моей жизни.
            </p>
            <p class="paragraph">
              Просто будь счастлива, а я сделаю всё, чтобы быть рядом.
            </p>
          </div>
          <footer class="letter-footer">
            <div class="line"></div>
            <span>Твой Егор</span>
          </footer>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const active = ref(false)

// Расставляем сердца по периметру, чтобы не мешать тексту
const heartPosition = (n) => {
  const positions = [
    { left: '5%', top: '10%' }, { left: '85%', top: '15%' },
    { left: '10%', top: '70%' }, { left: '90%', top: '80%' },
    { left: '75%', top: '5%' }, { left: '15%', top: '40%' },
    { left: '80%', top: '50%' }, { left: '40%', top: '90%' }
  ]
  return {
    ...positions[n-1],
    width: (30 + Math.random() * 40) + 'px',
    animationDelay: (n * -2) + 's',
    animationDuration: (8 + Math.random() * 5) + 's'
  }
}
</script>

<style scoped>
.stage {
  height: 100vh; width: 100%;
  background: #ffffff;
  display: flex; align-items: center; justify-content: center;
  overflow: hidden; cursor: pointer; position: relative;
  font-family: 'Inter', sans-serif;
}

/* Фоновые сердца */
.ambient-bg {
  position: absolute; inset: 0; pointer-events: none;
}
.slow-heart {
  position: absolute;
  animation: float-soft infinite ease-in-out;
  opacity: 0.8;
}

@keyframes float-soft {
  0%, 100% { transform: translate(0, 0) rotate(0deg); }
  50% { transform: translate(10px, -20px) rotate(10deg); }
}

/* Стартовое состояние */
.intro { text-align: center; z-index: 5; }
.minimal-heart { width: 40px; margin: 0 auto; }
.tap-hint {
  margin-top: 25px; font-size: 11px; letter-spacing: 5px;
  color: #ffb1c1; text-transform: uppercase;
}

/* Стили письма */
.letter-container {
  max-width: 500px; width: 85%; padding: 20px;
  z-index: 10;
}
.letter-header {
  font-size: 11px; letter-spacing: 4px; color: #ffb1c1;
  margin-bottom: 30px;
}
.greeting {
  font-weight: 300; font-size: 28px; color: #1a1a1a;
  margin-bottom: 20px;
}
.paragraph {
  font-size: 16px; line-height: 1.7; color: #444;
  margin-bottom: 15px; font-weight: 300;
}
.letter-footer {
  margin-top: 40px; display: flex; align-items: center; gap: 15px;
  color: #ffb1c1; font-weight: 400; font-size: 18px;
}
.line { height: 1px; width: 35px; background: #ffdae3; }

/* Плавный переход */
.fade-clean-enter-active, .fade-clean-leave-active {
  transition: all 1s ease-in-out;
}
.fade-clean-enter-from, .fade-clean-leave-to {
  opacity: 0; transform: translateY(10px); filter: blur(4px);
}
</style>
