<script setup>
import { computed } from 'vue'
import { useStore } from 'vuex'
import MainSection from '@/components/MainSection.vue'

const props = defineProps({
  bg: {
    type: String,
    required: true,
    validator: value => ['login', 'error'].includes(value)
  }
})

const store = useStore()

const darkMode = computed(() => store.state.darkMode)

const componentClass = computed(() => {
  const bgs = {
    login: 'bg-gradient-to-tr from-purple-400 via-pink-500 to-red-500',
    loginDark: 'bg-gradient-to-tr from-purple-900 via-pink-900 to-red-900',
    error: 'bg-gradient-to-tr from-pink-400 via-red-500 to-yellow-500',
    errorDark: 'bg-gradient-to-tr from-pink-900 via-red-900 to-yellow-900',
    leadertask: 'bg-orange-100 bg-no-repeat bg-left-bottom md:bg-[url("@/assets/images/pic.png")]'
  }

  const bgKey = darkMode.value ? `${props.bg}Dark` : props.bg

  return bgs[bgKey] ?? ''
})

</script>

<template>
  <main-section
    :class="componentClass"
    class="flex flex-col"
  >
    <slot
      card-class="w-8/12 md:w-5/12 lg:w-4/12 xl:w-3/12 shadow bg-white dark:bg-white"
      card-rounded="rounded-3xl"
    />
  </main-section>
</template>
