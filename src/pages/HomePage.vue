<template>

    <section class="mt-12 flex items-center justify-center">
        <div class="container mx-auto">
            <HeroQuote :author="randomQuote.content" :content="randomQuote.author" />
        </div>
    </section>

    <section class="mt-12 flex items-center justify-center">
        <div class="container mx-auto">
            <SearchBar @searchTermChange="loadSearchResults($event)" @reset="getAllQuotes" />
        </div>
    </section>

    <section class="container mx-auto mt-12 mb-16 grid grid-cols-4 gap-8">
        <div class="col-span-2 lg:col-span-3">
            <QuoteList :quotes="quotes.results" :totalCount="quotes.totalCount" :currentPage="quotes.currentPage"
                :totalPages="quotes.totalPages" @changePage="updateQoutePage($event)" />
        </div>

        <div class="col-span-2 lg:col-span-1">
            <Authors :authors="authors.allAuthors" :totalCount="authors.totalCount" :currentPage="authors.currentPage"
                :totalPages="authors.totalPages" @changePage="updateAuthorPage($event)"
                @selectAuthor="showAuthorQoutes($event)" />

            <Tags class="mt-8" :tags="tags" @selectTag="showTagQoutes($event)" />
        </div>
    </section>

</template>

<script>
import QuoteList from '../components/QuoteList.vue';
import Authors from '../components/Authors.vue';
import Tags from '../components/Tags.vue';
import SearchBar from '../components/SearchBar.vue';
import HeroQuote from '../components/HeroQuote.vue';

export default {
    data() {
        return {
            api: "https://api.quotable.io",
            randomQuote: {
                content: "",
                author: ""
            },
            viewMode: "",
            searchTerm: "",
            quotes: {
                totalCount: 0,
                currentPage: 1,
                totalPages: 0,
                results: []
            },
            authors: {
                totalCount: 0,
                currentPage: 1,
                totalPages: 0,
                allAuthors: []
            },
            selectedTag: "",
            tags: []
        };
    },

    methods: {
        loadSearchResults(searchTerm) {
            this.searchTerm = searchTerm
            const self = this;

            async function getSearchResults() {
                const response = await fetch(`${self.api}/search/quotes?query=${self.searchTerm}`);

                const data = await response.json();
                if (response.ok) {
                    self.quotes.currentPage = data.page;
                    self.quotes.totalPages = data.totalPages;
                    self.quotes.totalCount = data.totalCount;
                    self.quotes.results = data.results;
                }
            }

            if (this.searchTerm) {
                this.viewMode = "BY_SEARCH";
                getSearchResults();
            } else {
                this.getAllQuotes();
            }
        },

        async getAllQuotes() {
            this.viewMode = "";
            this.searchTerm = "";

            const response = await fetch(`${this.api}/quotes`);

            const data = await response.json();
            if (response.ok) {
                this.quotes.currentPage = data.page;
                this.quotes.totalPages = data.totalPages;
                this.quotes.totalCount = data.totalCount;
                this.quotes.results = data.results;
            }
        },

        async updateQoutePage(direction) {
            let page = this.quotes.currentPage;
            if (direction === "next" && this.quotes.currentPage < this.quotes.totalPages) {
                page = this.quotes.currentPage + 1;
            }
            else if (direction === "prev" && this.quotes.currentPage > 1) {
                page = this.quotes.currentPage - 1;
            } else {
                page = 1;
            }

            let response = "";
            if (this.viewMode === "BY_SEARCH") {
                response = await fetch(`${this.api}/search/quotes?query=${this.searchTerm}&page=${page}`);
            }
            else if (this.viewMode === "BY_TAG") {
                response = await fetch(`${this.api}/quotes?tags=${this.selectedTag}&page=${page}`);
            }
            else {
                response = await fetch(`${this.api}/quotes?page=${page}`);
            }

            const data = await response.json();
            if (response.ok) {
                this.quotes.currentPage = data.page;
                this.quotes.totalPages = data.totalPages;
                this.quotes.totalCount = data.totalCount;
                this.quotes.results = data.results;
            }
        },

        async updateAuthorPage(direction) {
            let page = this.authors.currentPage;
            if (direction === "next" && this.authors.currentPage < this.authors.totalPages) {
                page = this.authors.currentPage + 1;
            }
            else if (direction === "prev" && this.authors.currentPage > 1) {
                page = this.authors.currentPage - 1;
            }
            else {
                page = 1;
            }

            const response = await fetch(`${this.api}/authors?page=${page}`);
            const data = await response.json();
            if (response.ok) {
                this.authors.currentPage = data.page;
                this.authors.totalPages = data.totalPages;
                this.authors.totalCount = data.totalCount;
                this.authors.allAuthors = data.results;
            }
        },

        async showAuthorQoutes(authorId) {
            this.viewMode = "BY_AUTHOR";
            const response = await fetch(`${this.api}/authors/${authorId}`);

            const data = await response.json();
            if (response.ok) {
                this.quotes.currentPage = 1;
                this.quotes.totalPages = 1;
                this.quotes.totalCount = data.quoteCount;
                this.quotes.results = data.quotes;
            }
        },

        async showTagQoutes(tag) {
            const response = await fetch(`${this.api}/quotes?tags=${tag}`);

            const data = await response.json();
            if (response.ok) {
                if (!data) {
                    alert("No quotes!");
                }
                else {
                    this.quotes.currentPage = data.page;
                    this.quotes.totalPages = data.totalPages;
                    this.quotes.totalCount = data.totalCount;
                    this.quotes.results = data.results;
                    this.viewMode = "BY_TAG";
                    this.selectedTag = tag;
                }
            }
        }
    },

    mounted() {
        const self = this;
        async function updateRandomQuote() {
            const response = await fetch(`${self.api}/random`);

            const data = await response.json();
            if (response.ok) {
                self.randomQuote.content = data.content;
                self.randomQuote.author = data.author;
            }
        }

        // initially load a random quote
        updateRandomQuote();
        // load random quote every 10 seconds
        setInterval(() => { updateRandomQuote(); }, 10_000);

        async function getAllAuthors() {
            const response = await fetch(`${self.api}/authors`);

            const data = await response.json();
            if (response.ok) {
                self.authors.currentPage = data.page;
                self.authors.totalPages = data.totalPages;
                self.authors.totalCount = data.totalCount;
                self.authors.allAuthors = data.results;
            }
        }

        async function getAllTags() {
            const response = await fetch(`${self.api}/tags`);

            const data = await response.json();
            if (response.ok) {
                self.tags = data;
            }
        }

        this.getAllQuotes();
        getAllAuthors();
        getAllTags();
    },

    components: { HeroQuote, QuoteList, Authors, Tags, SearchBar, HeroQuote }
}
</script>
