<template>
    <div class="home-wrapper">
        <Background />
        <div class="content-container">
            <div class="logo">
                <img :src="vibox" alt="logo">
            </div>
            <Transition name="fade">
                <div class="welcome-text" v-if="showText">
                    <span class="letter" v-for="(letter, index) in text" :key="index"
                        :style="`--delay: ${index * 0.1}s`">
                        {{ letter === ' ' ? '&nbsp;' : letter }}
                    </span>
                </div>
            </Transition>
            <Transition name="fade" appear>
                <Button class="welcome-button" label="GO" :route="isAuthenticated ? '/Vibe' : '/Login'"
                    @navigate="navigateToLogin" />
            </Transition>
        </div>
        <div class="music-button-container">
            <Music />
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';
import Background from '@/components/Background.vue';
import Button from '@/components/button.vue';
import Music from '@/components/music.vue';
import vibox from '@/assets/vibox.png'
import { useAuthStore } from '@/stores/authStore';
import { useRouter } from 'vue-router';

const text = ref('Welcome to Vibebox'.split(''));
const showText = ref(false);
const authStore = useAuthStore();
const router = useRouter();
const isAuthenticated = computed(() => !!authStore.token);

const navigateToLogin = (route) => {
    router.push(route);
};

onMounted(() => {
    showText.value = true;
});
</script>

<style scoped>
body,
html {
    margin: 0;
    padding: 0;
    overflow: hidden;
}

.home-wrapper {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
}

.content-container {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
}

.logo {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: -50px;
}

.logo img {
    width: 200px;
    height: auto;
    max-width: 100%;
}

.music-button-container {
    position: absolute;
    bottom: 20px;
    right: 20px;
    z-index: 10;
}

.welcome-text {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    max-width: 90vw;
    text-align: center;
    line-height: 1.2;
}

.welcome-button {
    opacity: 0;
    animation: fadeIn 0.5s ease forwards 1s;
}

.letter {
    display: inline-block;
    color: #fff;
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: bold;
    text-transform: uppercase;
    opacity: 0;
    transform: translateY(20px);
    animation:
        fadeIn 0.5s ease forwards,
        neonGlow 1.5s ease-in-out infinite alternate,
        blink 2s infinite;
    animation-delay: var(--delay);
    margin: 0 2px;
}

/* Animations */
@keyframes fadeIn {
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

@keyframes neonGlow {
    from {
        text-shadow:
            0 0 5px #fff,
            0 0 10px #fff,
            0 0 20px #1a5e9c,
            0 0 30px #1a5e9c;
    }

    to {
        text-shadow:
            0 0 10px #fff,
            0 0 20px #fff,
            0 0 30px #1a5e9c,
            0 0 40px #1a5e9c;
    }
}

@keyframes blink {

    0%,
    100% {
        opacity: 1;
    }

    50% {
        opacity: 0.7;
    }
}

.fade-enter-active {
    transition: opacity 1s ease;
}

.fade-enter-from {
    opacity: 0;
}

/* Responsive */
@media (max-width: 768px) {
    .letter {
        font-size: clamp(1.5rem, 8vw, 2rem);
        margin: 0 1px;
    }

    .content-container {
        gap: 1.5rem;
    }

    .music-button-container {
        bottom: 15px;
        right: 15px;
    }
}
</style>