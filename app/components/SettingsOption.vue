<script setup lang="ts">
import type { SignatureFormData } from '~~/types'

const data = defineModel<SignatureFormData>('data', { required: true })

const items = [
  { icon: 'i-lucide-user', slot: 'info' as const },
  { icon: 'i-lucide-link', slot: 'links' as const },
]

const infoFields = [
  { name: 'fullName', label: 'Full Name', type: 'text', icon: 'i-lucide-user',      placeholder: 'Jane Smith' },
  { name: 'jobTitle', label: 'Job Title', type: 'text', icon: 'i-lucide-briefcase', placeholder: 'Attorney' },
  { name: 'phone',    label: 'Phone',     type: 'tel',  icon: 'i-lucide-phone',      placeholder: '+1 (xxx) xxx-xxxx' },
]
</script>

<template>
  <div>
    <UTabs :items class="w-full">
      <template #info>
        <div class="mt-4 space-y-4">
          <div v-for="field in infoFields" :key="field.name">
            <label :for="field.name" class="block text-sm font-medium mb-1.5">{{ field.label }}</label>
            <UInput
              :id="field.name"
              v-model="data[field.name as keyof SignatureFormData]"
              :type="field.type"
              :icon="field.icon"
              :placeholder="field.placeholder"
            />
          </div>
        </div>
      </template>

      <template #links>
        <div class="mt-4 space-y-4">
          <div>
            <label for="linkedin" class="block text-sm font-medium mb-1.5">LinkedIn URL</label>
            <UInput
              id="linkedin"
              v-model="data.linkedin"
              type="url"
              icon="i-simple-icons-linkedin"
              placeholder="https://linkedin.com/in/yourname"
            />
          </div>
        </div>
      </template>
    </UTabs>
  </div>
</template>
