<script setup lang="ts">
const pageTitle = 'Sama Al Naser Law Firm Signature Generator'
const pageDescription = 'Create and copy your official Sama Al Naser Law Firm email signature.'

useSeoMeta({
  title: pageTitle,
  description: pageDescription,
  author: 'Sama Al Naser Law Firm',
  twitterTitle: pageTitle,
  twitterDescription: pageDescription,
  twitterCard: 'summary_large_image',
  twitterImage: 'https://r2.alnaser-law.com/icon-global.png',
  ogUrl: 'https://mailsig.alnaser-law.com',
  ogImage: 'https://r2.alnaser-law.com/icon-global.png',
  ogSiteName: pageTitle,
  ogTitle: pageTitle,
  ogDescription: pageDescription,
})

useHead({
  link: [
    {
      rel: 'icon',
      type: 'image/x-icon',
      href: '/favicon.ico',
    },
    {
      rel: 'apple-touch-icon',
      sizes: '180x180',
      href: '/apple-touch-icon.png',
    },
    {
      rel: 'icon',
      type: 'image/png',
      sizes: '32x32',
      href: '/favicon-32x32.png',
    },
    {
      rel: 'icon',
      type: 'image/png',
      sizes: '16x16',
      href: '/favicon-16x16.png',
    },
    {
      rel: 'manifest',
      href: '/site.webmanifest',
    },
  ],
})
</script>

<template>
  <Html lang="en">
    <UApp>
      <div class="flex h-full flex-col">
        <LayoutNavbar />
        <div class="h-full">
          <NuxtPage />
        </div>
      </div>
    </UApp>
  </Html>
</template>

<style>
#__nuxt, #__layout, body, html {
  height: 100%;
}
</style>
