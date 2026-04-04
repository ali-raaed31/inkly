<script setup lang="ts">
const header = ref(null)
const { height } = useElementSize(header)

onMounted(() => {
  document.documentElement.style.setProperty('--header-height', `${height.value}px`)
})
</script>

<template>
  <header
    ref="header"
    class="px-5 py-3 flex items-center justify-between border-b border-neutral-200 dark:border-neutral-800 bg-white dark:bg-neutral-900"
  >
    <div class="flex items-center gap-3">
      <!-- Dark logo shown in light mode; light logo shown in dark mode -->
      <img
        src="https://r2.alnaser-law.com/icon-dark.png"
        alt="Al-Naser Law"
        class="h-8 w-auto dark:hidden"
      >
      <img
        src="https://r2.alnaser-law.com/icon-light.png"
        alt="Al-Naser Law"
        class="h-8 w-auto hidden dark:block"
      >
      <div class="flex flex-col leading-tight">
        <span class="text-sm font-semibold text-neutral-900 dark:text-white">Al-Naser Law</span>
        <span class="text-xs text-neutral-500 dark:text-neutral-400">Email Signature Generator</span>
      </div>
    </div>
  </header>
</template>
