<template>
    <div class="rounded bg-gray-50 p-4">
        <h2 class="mb-4 text-lg font-semibold text-green-500">{{ author }}</h2>
        <p>{{ content }}</p>
        <p class="mt-4 text-xs text-gray-400">
            tags:
            <span class="mr-2" v-for="tag in tags">{{ tag }}</span>
        </p>
    </div>
</template>

<script>
export default {
    props: {
        content: String,
        author: String,
        tags: Array
    },
}
</script>