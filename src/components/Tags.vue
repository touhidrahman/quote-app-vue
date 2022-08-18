<template>
    <div class="rounded bg-gray-50 p-4">
        <h2 class="text-lg font-bold">Tags</h2>

        <div class="flex flex-wrap">
            <span @click="emitTagSelected(tag.name)" v-for="tag in tags" :key="tag.name"
                class="mr-3 cursor-pointer text-sm text-gray-500 hover:font-bold">
                {{ tag.name }}
            </span>
        </div>
    </div>
</template>

<script>
export default {
    props: { tags: Array },

    methods: {
        emitTagSelected(tagName) {
            this.$emit('selectTag', tagName)
        }
    }

}
</script>
