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
                    <code class="min-w-0 overflow-x-auto text-sm">
                        {{ webhookUrl }}/api/webhook/radarr
                    </code>
                    <button
                        type="button"
                        class="hover-row cursor-pointer p-1"
                        title="Copy to clipboard"
                        aria-label="Copy Radarr webhook URL"
                        @click="copy(`${webhookUrl}/api/webhook/radarr`, 'radarr')">
                        <CheckMarkIcon v-if="copied === 'radarr'" class="h-4 w-4 text-green-400" />
                        <CopyIcon v-else class="h-4 w-4" />
                    </button>
                </div>
                <span class="font-semibold">Sonarr</span>
                <div class="bg-accent/20 mt-1 flex items-center justify-between rounded p-2">
                    <code class="min-w-0 overflow-x-auto text-sm">
                        {{ webhookUrl }}/api/webhook/sonarr
                    </code>
                    <button
                        type="button"
                        class="hover-row cursor-pointer p-1"
                        title="Copy to clipboard"
                        aria-label="Copy Sonarr webhook URL"
                        @click="copy(`${webhookUrl}/api/webhook/sonarr`, 'sonarr')">
                        <CheckMarkIcon v-if="copied === 'sonarr'" class="h-4 w-4 text-green-400" />
                        <CopyIcon v-else class="h-4 w-4" />
                    </button>
                </div>
            </div>
        </template>
    </CardComponent>
</template>

<script setup lang="ts">
import { ref, onBeforeUnmount } from 'vue'
import CardComponent from '@/components/common/CardComponent.vue'
import CopyIcon from '@/components/icons/CopyIcon.vue'
import CheckMarkIcon from '@/components/icons/CheckMarkIcon.vue'

const webhookUrl = window.location.origin
const copied = ref<string | null>(null)
let copyTimer: ReturnType<typeof setTimeout> | null = null

onBeforeUnmount(() => {
    if (copyTimer) clearTimeout(copyTimer)
})

const copy = async (url: string, key: 'radarr' | 'sonarr') => {
    if (!navigator.clipboard?.writeText) return

    try {
        await navigator.clipboard.writeText(url)
        if (copyTimer) clearTimeout(copyTimer)
        copied.value = key
        copyTimer = setTimeout(() => {
            copied.value = null
            copyTimer = null
        }, 2000)
    } catch {
        return
    }
}
</script>
