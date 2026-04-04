<script setup lang="ts">
import type { SignatureFormData } from '~~/types'

const data = ref<SignatureFormData>({
  fullName: '',
  jobTitle: '',
  phone: '',
  linkedin: '',
})

const colorMode = useColorMode()
const isMobile = ref(false)
const showSidebar = ref(true)

onMounted(() => {
  const checkMobile = () => {
    isMobile.value = window.innerWidth < 1024
    showSidebar.value = !isMobile.value
  }
  checkMobile()
  window.addEventListener('resize', checkMobile)
  return () => window.removeEventListener('resize', checkMobile)
})
</script>

<template>
  <div class="flex h-full flex-col lg:flex-row">
    <!-- Sidebar -->
    <div
      v-if="showSidebar"
      class="w-full lg:w-80 h-full overflow-y-auto border-r border-neutral-200 dark:border-neutral-800 bg-gray-50 dark:bg-neutral-900 flex flex-col"
    >
      <div class="sticky top-0 z-10 px-4 py-3 border-b border-neutral-200 dark:border-neutral-800 flex items-center justify-between bg-gray-50 dark:bg-neutral-900">
        <h2 class="text-sm font-semibold">
          Your Details
        </h2>
        <UButton
          icon="i-lucide-x"
          color="neutral"
          variant="ghost"
          class="lg:hidden"
          size="xs"
          @click="showSidebar = false"
        />
      </div>
      <div class="p-4">
        <SettingsOption v-model:data="data" />
      </div>
    </div>

    <!-- Mobile FAB to reopen sidebar -->
    <div class="lg:hidden fixed bottom-4 right-4 z-20">
      <UButton
        v-if="!showSidebar"
        icon="i-lucide-pencil"
        color="neutral"
        variant="solid"
        size="lg"
        class="shadow-lg"
        @click="showSidebar = true"
      />
    </div>

    <!-- Preview panel -->
    <div class="relative flex-1 h-full overflow-auto bg-neutral-100 dark:bg-neutral-950 flex items-center justify-center p-8">
      <div class="absolute right-4 top-4 z-10">
        <UTooltip :text="colorMode.value === 'light' ? 'Switch to dark mode' : 'Switch to light mode'">
          <UButton
            :icon="colorMode.value === 'dark' ? 'i-lucide-sun' : 'i-lucide-moon'"
            color="neutral"
            variant="ghost"
            size="xs"
            @click="colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'"
          />
        </UTooltip>
      </div>
      <div class="w-full max-w-2xl">
        <p class="text-xs text-neutral-500 dark:text-neutral-400 mb-4 text-center uppercase tracking-widest font-medium">
          Signature Preview
        </p>
        <div class="bg-white dark:bg-neutral-900 rounded-xl border border-neutral-200 dark:border-neutral-800 p-8 shadow-sm">
          <SignaturePreview :data />
        </div>
      </div>
    </div>
  </div>
</template>
