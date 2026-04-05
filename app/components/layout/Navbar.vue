<script setup lang="ts">
const COMPANY_ICON_URL = 'https://r2.alnaser-law.com/icon-dark.png'
const COMPANY_NAME = 'Sama Al Naser Law Firm'
const COMPANY_URL = 'https://alnaser-law.com'

const header = ref(null)
const { height } = useElementSize(header)

onMounted(() => {
  document.documentElement.style.setProperty('--header-height', `${height.value}px`)
})
</script>

<template>
  <header
    ref="header"
    class="px-4 py-3 flex items-center justify-between border-b border-neutral-200 dark:border-neutral-800 bg-white dark:bg-neutral-900"
  >
    <NuxtLink :to="COMPANY_URL" target="_blank" class="flex items-center gap-3">
      <img
        :src="COMPANY_ICON_URL"
        :alt="COMPANY_NAME"
        class="h-10 w-10 rounded-md object-cover"
      >
      <div class="flex flex-col">
        <span class="text-sm font-semibold text-neutral-900 dark:text-white">{{ COMPANY_NAME }}</span>
        <span class="text-xs text-neutral-500 dark:text-neutral-400">Email Signature Generator</span>
      </div>
    </NuxtLink>

    <NuxtLink :to="COMPANY_URL" target="_blank">
      <UButton
        color="neutral"
        variant="ghost"
        icon="i-lucide-external-link"
        size="xs"
        label="Website"
      />
    </NuxtLink>
  </header>
</template>
