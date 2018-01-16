<template>
  <div id="app">
    <sidebar
      :categories="categories"
      v-on:category-selected="setSelectedCategory">
      <!-- bind 'selected-category event to the event handler setSelectedCategory' -->
    </sidebar>
    <div class="ui segments">
      <timer-area></timer-area>
      <bookmark-list
        :bookmarks="bookmarks | filterByCategory selectedCategory"
        :categories="categories">
      </bookmark-list>
    </div>
  </div>
</template>

<script>
  import store from './store'
  import Sidebar from './components/Sidebar.vue'
  import TimerArea from './components/TimerArea.vue'
  import BookmarkList from './components/BookmarkList.vue'
  import { filterByCategory } from './filters'

  export default {

    components: {
      Sidebar,
      BookmarkList,
      TimerArea
    },

    data () {
      return {
        categories: {},
        bookmarks: {},
        selectedCategory: ''
      }
    },

    filters: {
      filterByCategory
    },

    created () {
      store.on('data-updated', this.updateListings)
    },

    methods: {
      updateListings (categories, bookmarks) {
        this.categories = categories
        this.bookmarks = bookmarks
      },

      setSelectedCategory (category) {
        this.selectedCategory = category;
      }

    }

  }
</script>
