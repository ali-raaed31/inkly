<script setup lang="ts">
import type { SignatureFormData, SignatureOptions } from '~~/types'

defineModel<SignatureOptions>('options', { required: true })
const data = defineModel<SignatureFormData>('data', { required: true })

const items = [
  {
    icon: 'i-lucide-user',
    slot: 'information' as const
  },
  {
    icon: 'i-simple-icons-linkedin',
    slot: 'socials' as const
  },
]

const uid = Math.random().toString(36).substring(2, 15)
</script>

<template>
  <div>
    <UTabs :items class="w-full">
      <template #information>
        <div class="mt-4 space-y-4">
          <div>
            <label :for="`fullName-${uid}`" class="block text-sm font-medium mb-1">Full Name</label>
            <UInput
              :id="`fullName-${uid}`"
              v-model="data.fullName"
              type="text"
              icon="i-lucide-user"
            />
          </div>

          <div>
            <label :for="`jobTitle-${uid}`" class="block text-sm font-medium mb-1">Job Title</label>
            <UInput
              :id="`jobTitle-${uid}`"
              v-model="data.jobTitle"
              type="text"
              icon="i-lucide-briefcase"
            />
          </div>

          <div>
            <label :for="`phone-${uid}`" class="block text-sm font-medium mb-1">Phone</label>
            <UInput
              :id="`phone-${uid}`"
              v-model="data.phone"
              type="tel"
              icon="i-lucide-phone"
            />
          </div>

          <div>
            <label :for="`email-${uid}`" class="block text-sm font-medium mb-1">Email</label>
            <UInput
              :id="`email-${uid}`"
              v-model="data.email"
              type="email"
              icon="i-lucide-mail"
            />
          </div>
        </div>
      </template>

      <template #socials>
        <div class="mt-4">
          <label :for="`linkedin-${uid}`" class="block text-sm font-medium mb-1">LinkedIn URL</label>
          <UInput
            :id="`linkedin-${uid}`"
            v-model="data.socials[0].url"
            type="url"
            icon="i-simple-icons-linkedin"
            placeholder="https://www.linkedin.com/in/your-profile"
            class="w-full"
          />
        </div>
      </template>
    </UTabs>
  </div>
</template>
