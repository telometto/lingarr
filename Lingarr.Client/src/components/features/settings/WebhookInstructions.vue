<template>
    <CardComponent title="Webhook">
        <template #description>
            Configure webhooks in Radarr and Sonarr to automatically trigger translations when new
            media is added.
        </template>
        <template #content>
            <div class="flex flex-col space-y-2">
                <span>
                    Go to
                    <b>Settings → Connect → Connections → +</b>
                    and use this URL:
                </span>
                <span class="font-semibold">Radarr</span>
                <div class="bg-accent/20 mt-1 flex items-center justify-between rounded p-2">
                    <code class="text-sm">
                        {{ webhookUrl }}/api/webhook/radarr
                    </code>
                    <button
                        class="hover-row cursor-pointer p-1"
                        title="Copy to clipboard"
                        @click="copy(`${webhookUrl}/api/webhook/radarr`)">
                        <CheckMarkIcon v-if="copied === 'radarr'" class="h-4 w-4 text-green-400" />
                        <CopyIcon v-else class="h-4 w-4" />
                    </button>
                </div>
                <span class="font-semibold">Sonarr</span>
                <div class="bg-accent/20 mt-1 flex items-center justify-between rounded p-2">
                    <code class="text-sm">
                        {{ webhookUrl }}/api/webhook/sonarr
                    </code>
                    <button
                        class="hover-row cursor-pointer p-1"
                        title="Copy to clipboard"
                        @click="copy(`${webhookUrl}/api/webhook/sonarr`)">
                        <CheckMarkIcon v-if="copied === 'sonarr'" class="h-4 w-4 text-green-400" />
                        <CopyIcon v-else class="h-4 w-4" />
                    </button>
                </div>
            </div>
        </template>
    </CardComponent>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import CardComponent from '@/components/common/CardComponent.vue'
import CopyIcon from '@/components/icons/CopyIcon.vue'
import CheckMarkIcon from '@/components/icons/CheckMarkIcon.vue'

const webhookUrl = window.location.origin
const copied = ref<string | null>(null)

const copy = async (url: string) => {
    const key = url.endsWith('radarr') ? 'radarr' : 'sonarr'
    await navigator.clipboard.writeText(url)
    copied.value = key
    setTimeout(() => {
        copied.value = null
    }, 2000)
}
</script>
