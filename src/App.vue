<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRegisterSW } from 'virtual:pwa-register/vue'

import ReloadPrompt from '@/components/ReloadPrompt.vue'

const serviceTitle = 'Service'
const serviceDescription = 'Description du service'
const logoText = ['Ministère', 'de l’intérieur']

const sourceLink = 'https://github.com/laruiss/dsfr-table-example'

const quickLinks = [
  {
    label: 'Home',
    path: '/',
    icon: 'ri-home-4-line',
    iconAttrs: { color: 'var(--red-marianne-425-625)' },
  },
  {
    label: 'À propos',
    path: '/a-propos',
    class: 'fr-fi-user-line',
  },
]
const searchQuery = ref('')

const {
  offlineReady,
  needRefresh,
  updateServiceWorker,
} = useRegisterSW()

const close = () => {
  offlineReady.value = false
  needRefresh.value = false
}

const pets = ref([])
onMounted(() => {
  setTimeout(() => {
    pets.value = [
      ['Snoopy', 'Dog'],
      ['Nemo', 'Fish'],
      ['Tigger', 'Cat'],
    ]
  }, 2000)
})
</script>

<template>
  <DsfrHeader
    v-model="searchQuery"
    :service-title="serviceTitle"
    :service-description="serviceDescription"
    :logo-text="logoText"
    :quick-links="quickLinks"
    show-search
  />
  <div class="fr-container">
    <DsfrTable
      title="Pets"
      :headers="['Name',
                 'Kind']"
      :rows="pets"
    />
    <p>
      <a
        class="fr-link"
        type="button"
        icon="ri-github-fill"
        :href="sourceLink"
      >
        Aller au code source
        <VIcon name="ri-github-fill" />
      </a>
    </p>
  </div>

  <ReloadPrompt
    :offline-ready="offlineReady"
    :need-refresh="needRefresh"
    @close="close()"
    @update-service-worker="updateServiceWorker()"
  />
</template>
