<script setup>
import { ref, computed } from 'vue'

const accepted = ref(false)
const clickCount = ref(0)
const yesButtonSize = ref(16)
const noButtonSize = ref(16)
const yesButtonPadding = ref(16)
const noButtonPadding = ref(16)

const noButtonTexts = [
  'Non',
  'Tu es sûre ?',
  'Vraiment ?',
  'Réfléchis bien...',
  'Allez, dis oui !',
  "S'il te plaît...",
  'Juste un petit oui ?',
  'Tu vas me briser le cœur 💔',
  'Un dernier essai ?',
  'Ok... 🥺',
]

const encouragementMessages = [
  'Oh non... essaie encore ? 🥺',
  'Mon cœur se brise un peu... 💔',
  'Peut-être que le bouton OUI est plus attirant maintenant ? 😊',
  'Le bouton OUI devient de plus en plus tentant, non ? 💚',
  'Allez, tu sais que tu veux dire oui ! 😄',
  'Regarde comme le bouton OUI est beau maintenant ! ✨',
  'Le bouton NON devient tout petit... comme mes espoirs... 😢',
  "C'est presque impossible de cliquer sur NON maintenant ! 😏",
  'OUI OUI OUI OUI OUI ! 💕',
  "S'il te plaît, mon cœur ne peut plus attendre... 💖",
]

const noButtonText = computed(() => {
  return noButtonTexts[Math.min(clickCount.value, noButtonTexts.length - 1)]
})

const currentEncouragementMessage = computed(() => {
  return encouragementMessages[Math.min(clickCount.value - 1, encouragementMessages.length - 1)]
})

const rejectProposal = () => {
  clickCount.value++

  // Le bouton Non rétrécit
  noButtonSize.value = Math.max(8, noButtonSize.value - 2)
  noButtonPadding.value = Math.max(4, noButtonPadding.value - 2)

  // Le bouton Oui grossit
  yesButtonSize.value += 4
  yesButtonPadding.value += 4
}

const acceptProposal = () => {
  accepted.value = true
}

const reset = () => {
  accepted.value = false
  clickCount.value = 0
  yesButtonSize.value = 16
  noButtonSize.value = 16
  yesButtonPadding.value = 16
  noButtonPadding.value = 16
}

const confettiColors = ['#ff6b9d', '#c44569', '#f8b500', '#4834df', '#ff6348']

const confettiStyle = (index) => {
  return {
    left: Math.random() * 100 + '%',
    animationDelay: Math.random() * 3 + 's',
    backgroundColor: confettiColors[Math.floor(Math.random() * confettiColors.length)],
  }
}
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-pink-100 via-purple-100 to-red-100">
    <!-- Page de la question -->
    <div v-if="!accepted" class="flex flex-col items-center justify-center min-h-screen p-4">
      <!-- GIF -->
      <div class="mb-8 float">
        <img
          src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExcHBkdDR6bnZ5aWRvZ3N4Y3M4ZTZkYnRhZnFxbGZ3YmNlbWN0czl1aSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/MDJ9IbxxvDUQM/giphy.gif"
          alt="Cute Valentine"
          class="rounded-3xl shadow-2xl w-64 h-64 object-cover"
        />
      </div>

      <!-- Message -->
      <h1 class="text-4xl md:text-6xl font-bold text-pink-600 mb-4 text-center heartbeat">
        Veux-tu être ma Valentine ? 💖
      </h1>

      <p class="text-xl text-purple-700 mb-4 text-center">S'il te plaît dis oui... 🥺</p>

      <p v-if="clickCount > 0" class="mt-4 mb-8 text-purple-600 italic text-center max-w-md">
        {{ currentEncouragementMessage }}
      </p>

      <!-- Boutons -->
      <div class="flex gap-4 items-end">
        <button
          @click="acceptProposal"
          :style="{ fontSize: yesButtonSize + 'px', padding: yesButtonPadding + 'px' }"
          class="bg-green-500 hover:bg-green-600 text-white font-bold rounded-full shadow-lg transform transition-all duration-300 hover:scale-105"
        >
          Oui ! 💕
        </button>

        <button
          @click="rejectProposal"
          :style="{ fontSize: noButtonSize + 'px', padding: noButtonPadding + 'px' }"
          class="bg-red-500 hover:bg-red-600 text-white font-bold rounded-full shadow-lg transform transition-all duration-300 hover:scale-105"
        >
          {{ noButtonText }}
        </button>
      </div>
    </div>

    <!-- Page de succès -->
    <div
      v-else
      class="flex flex-col items-center justify-center min-h-screen p-4 relative overflow-hidden"
    >
      <!-- Confettis -->
      <div v-for="i in 50" :key="i" class="confetti" :style="confettiStyle(i)"></div>

      <div class="text-center z-10">
        <h1 class="text-6xl md:text-8xl font-bold text-pink-600 mb-8 heartbeat">Yaaay ! 🎉💖🎊</h1>

        <div class="mb-8">
          <img
            src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExbTd6dGN2OGZvN3ppZ3h6YnF1cGI2ZGt1YnZ3aWIyNzltNjJpZWg3ZiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/R6gvnAxj2ISzJdbA63/giphy-downsized-large.gif"
            alt="Celebration"
            class="rounded-3xl shadow-2xl mx-auto w-80 h-80 object-cover"
          />
        </div>

        <div class="bg-white/90 backdrop-blur-sm rounded-3xl p-8 shadow-2xl max-w-2xl mx-auto">
          <p class="text-3xl md:text-4xl font-bold text-purple-700 mb-4">
            Tu as fait de moi la personne la plus heureuse ! 💝
          </p>
          <p class="text-xl text-pink-600 mb-4">
            Cette Saint-Valentin sera magique grâce à toi... ✨
          </p>
          <p class="text-lg text-gray-700">Je savais que tu finirais par dire oui ! 😊</p>
          <div class="mt-8 text-6xl">💕 💖 💗 💓 💞</div>
        </div>

        <!-- <button
          @click="reset"
          class="mt-8 bg-purple-500 hover:bg-purple-600 text-white font-bold py-3 px-8 rounded-full shadow-lg transform transition-all duration-300 hover:scale-105"
        >
          Recommencer 🔄
        </button> -->
      </div>
    </div>
  </div>
</template>

<style scoped>
.button {
  cursor: pointer;
}
</style>
