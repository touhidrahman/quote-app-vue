<template>
    <div class="rounded bg-gray-50 p-4">
        <h2 class="text-lg font-bold">Authors</h2>

        <div class="flex items-baseline justify-between cursor-pointer" @click="selectAuthor(author._id)"
            v-for="author in authors" :key="author._id">
            <h2 class=" text-gray-500 hover:font-bold">
                {{ author.name }}
            </h2>
            <span class="rounded-full text-sm text-gray-500">{{ author.quoteCount }}</span>
        </div>

        <div class="flex items-baseline justify-between">
            <p class="text-sm">{{ totalCount }} authors | Page {{ currentPage }} / {{ totalPages }}</p>

            <div class="mt-6 flex items-center gap-4">
                <button class="rounded-full bg-green-200 p-2 text-sm text-green-500" @click="changeAuthorPage('prev')" :disabled="currentPage === 1">Prev</button>
                <button class="rounded-full bg-green-200 p-2 text-sm text-green-500" @click="changeAuthorPage('next')" :disabled="currentPage === totalPages">Next</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        authors: Array,
        totalCount: Number,
        currentPage: Number,
        totalPages: Number
    },

    methods: {
        selectAuthor(id) {
            this.$emit('selectAuthor', id)
        },

        changeAuthorPage(direction) {
            this.$emit('changePage', direction)
        }
    }
}
</script>
