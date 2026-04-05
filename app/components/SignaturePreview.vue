<script setup lang="ts">
import type { Signature } from '~~/types'

const COMPANY_URL = 'https://alnaser-law.com'
const COMPANY_ADDRESS = 'Al Mi\'dan Street, Manawi Basha, Basra.'
const LEGAL_500_BADGE_IMAGE_URL = 'https://r2.alnaser-law.com/legal-500.png'
const LEGAL_500_BADGE_URL = 'https://www.legal500.com/firms/247355-sama-al-nasser-law-firm/c-iraq/about'
const DISCLAIMER_TEXT = 'This email and any attachments are confidential and may be legally privileged. If you are not the intended recipient, please notify the sender immediately, delete this message, and do not disclose or copy its contents. The sender accepts no liability for any errors, omissions, or viruses transmitted via this communication.'
const DIVIDER_COLOR = '#d4d4d8'
const NAME_COLOR = '#137399'
const COMPANY_NAME_COLOR = '#af8c66'
const DISCLAIMER_COLOR = '#8b949e'

const props = defineProps<Signature & { theme?: string }>()
const { data, options } = toRefs(props)

const toast = useToast()
const signatureContainer = ref<HTMLElement>()
const copied = ref(false)
let copiedTimeout: ReturnType<typeof setTimeout> | undefined
const { width } = useWindowSize()

const linkedinUrl = computed(() => {
  const linkedin = data.value.socials.find((social) => social.type === 'linkedin')

  return linkedin?.url.trim() || ''
})

const emailAddress = computed(() => data.value.email.trim())
const isSmallViewport = computed(() => width.value < 640)
const phoneHref = computed(() => `tel:${data.value.phone.replace(/\s+/g, '')}`)
const emailHref = computed(() => `mailto:${emailAddress.value}`)

function markCopied() {
  copied.value = true

  if (copiedTimeout) {
    clearTimeout(copiedTimeout)
  }

  copiedTimeout = setTimeout(() => {
    copied.value = false
  }, 1000)
}

async function copyRichHtml() {
  if (
    !signatureContainer.value
    || typeof window === 'undefined'
    || !window.isSecureContext
    || typeof ClipboardItem === 'undefined'
    || !navigator.clipboard
    || typeof navigator.clipboard.write !== 'function'
  ) {
    return false
  }

  const html = signatureContainer.value.innerHTML
  const text = signatureContainer.value.innerText

  await navigator.clipboard.write([
    new ClipboardItem({
      'text/html': new Blob([html], { type: 'text/html' }),
      'text/plain': new Blob([text], { type: 'text/plain' }),
    }),
  ])

  return true
}

function copyFromSelectionFallback() {
  if (!signatureContainer.value || typeof window === 'undefined') {
    return false
  }

  const selection = window.getSelection()

  if (!selection) {
    return false
  }

  const previousRanges = Array.from({ length: selection.rangeCount }, (_, index) => selection.getRangeAt(index).cloneRange())
  selection.removeAllRanges()

  const range = document.createRange()
  range.selectNodeContents(signatureContainer.value)
  selection.addRange(range)

  const copiedSuccessfully = document.execCommand('copy')

  selection.removeAllRanges()
  previousRanges.forEach(savedRange => selection.addRange(savedRange))

  return copiedSuccessfully
}

async function copyToClipboard() {
  try {
    const richCopyWorked = await copyRichHtml()

    if (!richCopyWorked) {
      const fallbackWorked = copyFromSelectionFallback()

      if (!fallbackWorked) {
        throw new Error('Copy failed')
      }
    }

    markCopied()
    toast.add({
      title: 'Signature copied to clipboard!',
      description: 'You can now paste it in your email client.',
      icon: 'i-heroicons-clipboard-document-check',
      color: 'green',
      timeout: 2000,
    })
  } catch {
    toast.add({
      title: 'Failed to copy',
      icon: 'i-heroicons-exclamation-circle',
      color: 'red',
      timeout: 2000,
    })
  }
}

onBeforeUnmount(() => {
  if (copiedTimeout) {
    clearTimeout(copiedTimeout)
  }
})
</script>

<template>
  <div class="w-full">
    <div ref="signatureContainer" class="py-2">
      <ClientOnly>
        <div>
          <table :style="options.color.transparent ? {} : { backgroundColor: `${options.color.background}` }" style="width: 100%; max-width: 540px; border-collapse: collapse;">
            <tbody>
              <tr>
                <td
                  style="padding: 4px 0; vertical-align: top;"
                  :style="[
                    { width: `${options.image.size + 8}px` },
                    options.image.align === 'top' ? { verticalAlign: 'top' } : {},
                    options.image.align === 'center' ? { verticalAlign: 'middle' } : {},
                    options.image.align === 'bottom' ? { verticalAlign: 'bottom' } : {},
                  ]"
                >
                  <a :href="COMPANY_URL" target="_blank" rel="noopener noreferrer" style="text-decoration: none;">
                    <img
                      :src="data.image"
                      alt="Sama Al Naser Law Firm"
                      :width="options.image.size + 8"
                      :height="options.image.size + 8"
                      :style="[
                        options.image.form === 'rectangle' ? { width: `${options.image.size + 8}px` } : {},
                        options.image.form === 'square' ? { width: `${options.image.size + 8}px`, height: `${options.image.size + 8}px` } : {},
                        options.image.form === 'circle' ? { width: `${options.image.size + 8}px`, height: `${options.image.size + 8}px`, borderRadius: `${options.image.size + 8}px` } : {},
                        { display: 'block', objectFit: 'cover' },
                        options.image.border ? {
                          border: `${options.image.borderWidth}px ${options.image.borderStyle} ${options.image.borderColor}`,
                        } : {},
                      ]"
                    >
                  </a>
                </td>

                <td :style="{ width: `${options.gap.image}px`, fontSize: '0', lineHeight: '0' }">
                  &nbsp;
                </td>

                <td style="width: 1px; background-color: #d4d4d8; font-size: 0; line-height: 0;" :style="{ backgroundColor: DIVIDER_COLOR }">
                  &nbsp;
                </td>

                <td :style="{ width: `${options.gap.image}px`, fontSize: '0', lineHeight: '0' }">
                  &nbsp;
                </td>

                <td
                  style="padding: 2px 0 0; vertical-align: top;"
                  :style="[
                    { fontSize: `${options.size.subtitle}px` }
                  ]"
                  :class="[
                    options.font.family === 'inter' ? 'font-inter' :
                    options.font.family === 'sf' ? 'font-sf' :
                    options.font.family === 'roboto' ? 'font-roboto' :
                    'font-arial'
                  ]"
                >
                  <table :style="{ fontSize: `${options.size.subtitle}px`, borderCollapse: 'collapse' }">
                    <tbody>
                      <tr v-if="data.fullName">
                        <td
                          :style="[
                            {
                              fontSize: `${options.size.title}px`,
                              color: NAME_COLOR,
                              fontWeight: options.font.titleWeight,
                              lineHeight: '22px',
                              paddingBottom: '2px',
                            }
                          ]"
                        >
                          {{ data.fullName }}
                        </td>
                      </tr>

                      <tr v-if="data.jobTitle">
                        <td :style="{ lineHeight: '18px', paddingBottom: '4px' }">
                          {{ data.jobTitle }}
                        </td>
                      </tr>

                      <tr>
                        <td :style="{ color: COMPANY_NAME_COLOR, fontSize: '12px', fontWeight: '700', letterSpacing: '0.08em', textTransform: 'uppercase', lineHeight: '16px', paddingBottom: '2px' }">
                          {{ data.company }}
                        </td>
                      </tr>

                      <tr>
                        <td :style="{ lineHeight: '18px', paddingBottom: '2px' }">
                          {{ COMPANY_ADDRESS }}
                        </td>
                      </tr>

                      <tr v-if="!isSmallViewport">
                        <td :style="{ fontSize: `${options.size.social}px`, lineHeight: '20px', paddingTop: '4px', paddingBottom: '1px' }">
                          <a :href="phoneHref" style="text-decoration: none;">{{ data.phone }}</a>
                          <template v-if="emailAddress">
                            <span :style="{ color: COMPANY_NAME_COLOR, fontSize: '18px', lineHeight: '18px', verticalAlign: 'middle' }">&nbsp;&bull;&nbsp;</span>
                            <a :href="emailHref" style="text-decoration: none;">{{ emailAddress }}</a>
                          </template>
                          <template v-if="linkedinUrl">
                            <span :style="{ color: COMPANY_NAME_COLOR, fontSize: '18px', lineHeight: '18px', verticalAlign: 'middle' }">&nbsp;&bull;&nbsp;</span>
                            <a :href="linkedinUrl" target="_blank" rel="noopener noreferrer" style="text-decoration: underline;">LinkedIn</a>
                          </template>
                        </td>
                      </tr>

                      <template v-else>
                        <tr>
                          <td :style="{ fontSize: `${options.size.social}px`, lineHeight: '20px', paddingTop: '4px', paddingBottom: '1px' }">
                            <a :href="phoneHref" style="text-decoration: none;">{{ data.phone }}</a>
                          </td>
                        </tr>

                        <tr v-if="emailAddress || linkedinUrl">
                          <td :style="{ fontSize: `${options.size.social}px`, lineHeight: '20px' }">
                            <template v-if="emailAddress">
                              <a :href="emailHref" style="text-decoration: none;">{{ emailAddress }}</a>
                            </template>
                            <template v-if="linkedinUrl">
                              <span v-if="emailAddress" :style="{ color: COMPANY_NAME_COLOR, fontSize: '18px', lineHeight: '18px', verticalAlign: 'middle' }">&nbsp;&bull;&nbsp;</span>
                              <a :href="linkedinUrl" target="_blank" rel="noopener noreferrer" style="text-decoration: underline;">LinkedIn</a>
                            </template>
                          </td>
                        </tr>
                      </template>
                    </tbody>
                  </table>
                </td>
              </tr>
            </tbody>
          </table>

          <table style="margin-top: 12px; width: 100%; max-width: 540px; border-collapse: collapse;">
            <tbody>
              <tr>
                <td style="height: 1px; font-size: 0; line-height: 0;" :style="{ backgroundColor: DIVIDER_COLOR }">
                  &nbsp;
                </td>
              </tr>
              <tr>
                <td style="padding-top: 12px; padding-bottom: 4px;">
                  <a :href="LEGAL_500_BADGE_URL" target="_blank" rel="noopener noreferrer" style="text-decoration: none;">
                    <img
                      :src="LEGAL_500_BADGE_IMAGE_URL"
                      alt="The Legal 500"
                      height="72"
                      style="display: block; height: 72px; width: auto; max-width: 100%;"
                    >
                  </a>
                </td>
              </tr>
              <tr>
                <td :style="{ fontFamily: 'Arial, sans-serif', fontSize: '11px', lineHeight: '16px', paddingTop: '10px', color: DISCLAIMER_COLOR }">
                  {{ DISCLAIMER_TEXT }}
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <template #fallback>
          <div class="h-24 animate-pulse rounded-md bg-neutral-700" />
        </template>
      </ClientOnly>
    </div>

    <div class="mt-6 flex justify-center">
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
