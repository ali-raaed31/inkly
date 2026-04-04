<script setup lang="ts">
useSeoMeta({
  title: 'Al-Naser Law — Email Signature Generator',
  description: 'Generate your branded Al-Naser Law email signature in seconds.',
  author: 'Al-Naser Law',
  twitterTitle: 'Al-Naser Law — Email Signature Generator',
  twitterDescription: 'Generate your branded Al-Naser Law email signature in seconds.',
  twitterCard: 'summary',
  ogUrl: 'https://mailsig.alnaser-law.com',
  ogSiteName: 'Al-Naser Law — Email Signature Generator',
  ogTitle: 'Al-Naser Law — Email Signature Generator',
  ogDescription: 'Generate your branded Al-Naser Law email signature in seconds.',
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
