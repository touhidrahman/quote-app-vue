<template>
    <div class="flex">
        <span
            class="inline-flex items-center rounded-l-md border border-r-0 border-gray-300 bg-gray-200 px-3 text-sm text-gray-900">
            <i class="bx bx-search text-xl"></i>
        </span>
        <input type="text" v-model="searchTerm" placeholder="Input and press enter to search" @change="searchTermChange"
            class="sm:text-md block w-full min-w-0 flex-1 rounded-none rounded-r-lg border border-gray-300 bg-gray-50 p-4 text-sm text-gray-900 focus:border-blue-500 focus:ring-blue-500" />
        <button @click="searchTermChange"
            class="ml-3 mr-2 rounded-lg border border-gray-200 bg-white px-5 text-sm font-medium text-gray-900 focus:z-10 focus:text-blue-700 focus:ring-2 focus:ring-blue-700 hover:bg-gray-100 hover:text-blue-700">
            Search
        </button>
        <button @click="reset">Reset</button>

    </div>
</template>

<script>
export default {
    props: [],

    data() {
        return {
            searchTerm: '',
        }
    },

    methods: {
        searchTermChange() {
            this.$emit('searchTermChange', this.searchTerm)
        },

        reset() {
            this.searchTerm = ''
            this.$emit('reset')
        },
    }

}
</script>