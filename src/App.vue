<template>
  <main class="flex flex-col justify-center h-full mx-auto max-w-600px">
    <section class="flex flex-col items-center leading-loose text-center">
      <div class="text-3xl">
        <span class="i-twemoji-christmas-tree"></span>
        {{ t('happyHolidays') }}
        <span class="i-twemoji-world-map"></span>
      </div>
      <i18n-t keypath="christmasIsComing" tag="span">
        <template #time>
          <span>
            {{ t('day', days) }}
          </span>
        </template>
        <template #date>
          {{ d(christmasDate, 'long') }}
        </template>
      </i18n-t>
      <div>
        <button @click="next" class="icon-button">
          <span class="i-carbon-language"></span>
        </button>
      </div>
      <div>
        <span>
          {{ t('language') }}
        </span>
      </div>
    </section>
  </main>
</template>

<script setup>
import { computed, watchEffect, ref } from 'vue'
import { useI18n } from 'vue-i18n'

const { t, d, locale, availableLocales } = useI18n()

const christmasDate = new Date('2022/12/25')
const days = computed(() => {
  const count = christmasDate.getTime() - new Date()
  return Math.floor(count / (1000 * 60 * 60 * 24))
})

const listCycle = list => {
  const i = ref(0)

  const next = () => {
    if (i.value >= list.length - 1) {
      i.value = 0
    } else {
      i.value++
    }
  }

  const current = computed(() => list[i.value])

  return {
    next,
    current,
  }
}
const { current, next } = listCycle(availableLocales)
watchEffect(() => (locale.value = current.value))
</script>

<style scoped>
.icon-button {
  @apply text-xl
    w-32px
    h-32px
    rounded-full
    border-1
    border-transparent
    bg-transparent
    cursor-pointer
    duration-300
    hover:ring-2
    hover:border-green-500
    hover:ring-green-500
    hover:ring-opacity-40
    hover:text-green-600;
}
</style>
