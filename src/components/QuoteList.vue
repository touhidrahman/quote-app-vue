<template>
    <QuoteItem class="mb-6" v-for="quote in quotes" :key="quote._id" :content="quote.content" :author="quote.author" :tags="quote.tags" />

    <div class="flex items-baseline justify-between">
        <p class="text-sm">
            {{ totalCount }} quotes found | Page {{ currentPage }} of {{ totalPages }}
        </p>

        <div class="mt-6 flex items-center gap-4">
            <button class="rounded-full bg-green-200 p-2 text-sm text-green-500" @click="changePage('prev')" :disabled="currentPage === 1">Prev</button>
            <button class="rounded-full bg-green-200 p-2 text-sm text-green-500" @click="changePage('next')" :disabled="currentPage === totalPages">Next</button>
        </div>
    </div>
</template>

<script>
import QuoteItem from './QuoteItem.vue';

export default {
    props: {
        quotes: Array,
        totalCount: Number,
        currentPage: Number,
        totalPages: Number
    },

    methods: {
        changePage(direction) {
            this.$emit("changePage", direction);
        }
    },

    components: { QuoteItem }
}
</script>

