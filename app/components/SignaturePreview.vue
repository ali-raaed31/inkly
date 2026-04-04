<script setup lang="ts">
import type { SignatureFormData } from '~~/types'

defineProps<{ data: SignatureFormData }>()

const disclaimer = `CONFIDENTIALITY NOTICE: This email and any files transmitted with it are confidential and intended solely for the use of the individual or entity to whom they are addressed. If you have received this email in error, please notify the sender immediately and permanently delete this message. Al-Naser Law does not accept liability for any damage caused by any virus transmitted through this email.`

const toast = useToast()
const signatureContainer = ref<HTMLElement>()

const { copy, copied } = useClipboard({
  source: () => signatureContainer.value!.innerHTML,
  copiedDuring: 1500,
})

function copyToClipboard() {
  try {
    copy()
    toast.add({
      title: 'Signature copied to clipboard!',
      description: 'You can now paste it in your email client.',
      icon: 'i-heroicons-clipboard-document-check',
      color: 'success',
      duration: 2500,
    })
  } catch {
    toast.add({
      title: 'Failed to copy',
      icon: 'i-heroicons-exclamation-circle',
      color: 'error',
      duration: 2500,
    })
  }
}
</script>

<template>
  <div class="w-full">
    <div ref="signatureContainer" class="py-2">
      <ClientOnly>
        <table cellpadding="0" cellspacing="0" border="0" style="font-family: Arial, Helvetica, sans-serif;">
          <tbody>
            <!-- Main row -->
            <tr>
              <!-- Firm logo -->
              <td style="padding: 6px 8px 6px 0; vertical-align: middle;">
                <a href="https://alnaser-law.com" target="_blank" style="display: block; line-height: 0; text-decoration: none;">
                  <img
                    src="https://r2.alnaser-law.com/icon-dark.png"
                    alt="Al-Naser Law"
                    width="56"
                    height="56"
                    style="display: block; width: 56px; height: 56px; object-fit: contain;"
                  >
                </a>
              </td>

              <!-- Gold divider -->
              <td style="padding: 0 8px; vertical-align: middle;">
                <div style="width: 1px; height: 64px; background-color: #c8a96e;" />
              </td>

              <!-- Contact info -->
              <td style="padding: 6px 0; vertical-align: middle;">
                <table cellpadding="0" cellspacing="0" border="0">
                  <tbody>
                    <tr v-if="data.fullName">
                      <td style="padding: 0 0 2px 0; font-size: 15px; font-weight: 700; color: #1a2e4a; white-space: nowrap;">
                        {{ data.fullName }}
                      </td>
                    </tr>
                    <tr v-if="data.jobTitle">
                      <td style="padding: 0 0 6px 0; font-size: 12px; color: #c8a96e; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; white-space: nowrap;">
                        {{ data.jobTitle }}
                      </td>
                    </tr>
                    <tr v-if="data.linkedin || data.phone">
                      <td style="padding: 0;">
                        <table cellpadding="0" cellspacing="0" border="0">
                          <tbody>
                            <tr>
                              <td v-if="data.linkedin" style="padding: 0; white-space: nowrap;">
                                <a
                                  :href="data.linkedin"
                                  target="_blank"
                                  style="text-decoration: none; color: #0A66C2; font-size: 12px; font-weight: 600;"
                                >
                                  <img
                                    src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/LinkedIn_logo_initials.png/20px-LinkedIn_logo_initials.png"
                                    alt="LinkedIn"
                                    width="13"
                                    height="13"
                                    style="display: inline-block; vertical-align: middle; margin-right: 4px;"
                                  >
                                  <span style="vertical-align: middle;">LinkedIn</span>
                                </a>
                              </td>
                              <td v-if="data.linkedin && data.phone" style="padding: 0 8px; color: #c8a96e; font-size: 12px;">|</td>
                              <td v-if="data.phone" style="padding: 0; white-space: nowrap; color: #555555; font-size: 12px;">
                                {{ data.phone }}
                              </td>
                            </tr>
                          </tbody>
                        </table>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </td>
            </tr>

            <!-- Footer row: Legal500 + disclaimer -->
            <tr>
              <td colspan="3" style="padding-top: 14px; border-top: 1px solid #e5e5e5;">
                <a
                  href="https://www.legal500.com/firms/247355-sama-al-nasser-law-firm/c-iraq/about"
                  target="_blank"
                  style="display: inline-block; line-height: 0; text-decoration: none; margin-bottom: 8px;"
                >
                  <img
                    src="https://r2.alnaser-law.com/legal-500.png"
                    alt="Legal 500"
                    width="49"
                    height="54"
                    style="display: block; width: 49px; height: 54px; object-fit: contain;"
                  >
                </a>
                <p style="margin: 0; font-size: 9px; line-height: 1.4; color: #aaaaaa; font-family: Arial, Helvetica, sans-serif;">
                  {{ disclaimer }}
                </p>
              </td>
            </tr>
          </tbody>
        </table>

        <template #fallback>
          <div class="h-24 animate-pulse bg-neutral-200 rounded-md" />
        </template>
      </ClientOnly>
    </div>

    <div class="flex justify-center mt-6">
      <UButton
        size="lg"
        color="primary"
        variant="solid"
        :icon="copied ? 'i-heroicons-clipboard-document-check' : 'i-heroicons-clipboard-document'"
        :label="copied ? 'Copied!' : 'Copy Signature'"
        @click="copyToClipboard()"
      />
    </div>
  </div>
</template>
