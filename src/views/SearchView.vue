<template>
    <div class="search">
        <h1>Search Books</h1>
        <div class="search-controls">
            <input v-model="searchQuery" placeholder="Enter title or author..." @input="performSearch">
            <select v-model="searchType">
                <option value="title">Title</option>
                <option value="author">Author</option>
            </select>
        </div>
        <div class="search-results">
            <div v-for="book in searchResults" :key="book.id" class="book-item">
                <BookView :book="book" />
                <button @click="() => { showModal = true; chooseBook = book }">Offer Trade</button>

            </div>
            <div v-if="showModal" class="modal">
                <div class="modal-content">
                    <span class="close" @click="closeModal">&times;</span>
                    <h2>Offer Trade</h2>
                    <input v-model="sendTo" placeholder="Enter username...">
                    <button @click="createTrade">Confirm</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import type { Book, OutgoingTrade } from '../../shared/book'
import BookView from '@/components/BookView.vue'

export default {
    data() {
        return {
            chooseBook: {} as Book,
            searchQuery: '',
            searchType: 'title' as 'title' | 'author',
            searchResults: [] as Book[],
            sendTo: '',
            showModal: false
        }
    },
    emits: ['createTrade'],
    methods: {
        closeModal(): void {
            this.showModal = false
        },
        performSearch(): void {
            try {
                // this is just to test that the search works
                const data: Book[] = [
                    {
                        id: 1,
                        title: 'The Great Gatsby',
                        author: 'F. Scott Fitzgerald',
                        cover: 'https://i0.wp.com/americanwritersmuseum.org/wp-content/uploads/2018/02/CK-3.jpg?resize=267%2C400&ssl=1'
                    },
                    {
                        id: 2,
                        title: '1984',
                        author: 'George Orwell',
                        cover: 'https://m.media-amazon.com/images/I/61NAx5pd6XL.jpg'
                    }
                ] as Book[]
                // TODO: make a request to the server to search for books
                // const response = await fetch(
                //     `/api/books/search?q=${this.searchQuery}&type=${this.searchType}`
                // )
                // const data = await response.json()
                this.searchResults = data as Book[]
            } catch (error) {
                console.error('Error performing search:', error)
                this.searchResults = []
            }
        },
        createTrade(): void {

            // emit trade to parent component 
            const trade: OutgoingTrade = {
                book: this.chooseBook,
                offeredTo: this.sendTo,
                status: 'pending',
                createdAt: new Date().toISOString(),
                id: 0,
                offeredBy: '' // TODO: get the current user
            }
            this.$emit('createTrade', trade);
        }
    },
    components: {
        BookView
    }
}
</script>

<style scoped>
.modal-content {
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
}
</style>