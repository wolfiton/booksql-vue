<template>
  <div class="home">
    <router-link to="/books/add">Add Book</router-link>
    <ApolloQuery :query="categoriesQuery" class="move-down">
      <!-- The result will automatically updated -->

      <template slot-scope="{ result: { data, loading } }">
        <!-- Some content -->
        <div v-if="loading">Loading...</div>
        <div v-else>
          <a
            href="#"
            class="link-margin"
            @click.prevent="selectCategory('all')"
          >
            All
          </a>
          <a
            href="#"
            class="link-margin"
            @click.prevent="selectCategory('featured')"
          >
            Featured
          </a>
          <a
            href="#"
            v-for="category of data.categories"
            :key="category.id"
            @click.prevent="selectCategory(category.id)"
            class="link-margin"
          >
            {{ category.id }}.
            {{ category.name }}
          </a>
        </div>
      </template>
    </ApolloQuery>

    <div v-if="selectedCategory === 'all'">
      <ApolloQuery :query="query">
        <!-- The result will automatically updated -->
        <template slot-scope="{ result: { data, loading } }">
          <!-- Some content -->
          <div v-if="loading">Loading...</div>
          <div v-else>
            <div v-for="book of data.books" :key="book.id" class="link-margin">
              <router-link :to="`books/${book.id}`">
                .
                {{ book.title }}
              </router-link>
              <div>{{ book.author }}</div>
              <img
                :src="`http://localhost:8000/img/${book.image}`"
                alt="cover image"
              />
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>

    <div v-else-if="selectedCategory === 'featured'">
      <ApolloQuery :query="query" :variables="{ featured: true }">
        <!-- The result will automatically updated -->
        <template slot-scope="{ result: { data, loading } }">
          <!-- Some content -->
          <div v-if="loading">Loading...</div>
          <div v-else>
            <div
              v-for="book of data.booksByFeatured"
              :key="book.id"
              class="link-margin"
            >
              <router-link :to="`books/${book.id}`">
                {{ book.title }}
              </router-link>
              <div>{{ book.author }}</div>
              <img
                :src="`http://localhost:8000/img/${book.image}`"
                alt="cover image"
              />
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>

    <div v-else>
      <ApolloQuery :query="query" :variables="{ id: selectedCategory }">
        <!-- The result will automatically updated -->
        <template slot-scope="{ result: { data, loading } }">
          <!-- Some content -->
          <div v-if="loading">Loading...</div>
          <div v-else>
            <div
              v-for="book of data.category.books"
              :key="book.id"
              class="link-margin"
            >
              <router-link :to="`books/${book.id}`">
                {{ book.title }}
              </router-link>
              <div>{{ book.author }}</div>
              <img
                :src="`http://localhost:8000/img/${book.image}`"
                alt="cover image"
              />
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
  </div>
</template>

<script>
import categoriesQuery from "@/graphql/queries/Categories.gql";
import categoryQuery from "@/graphql/queries/Category.gql";
import booksQuery from "@/graphql/queries/Books.gql";
import booksFeaturedQuery from "@/graphql/queries/BooksFeatured.gql";
export default {
  name: "Home",
  data() {
    return {
      categoriesQuery,
      categoryQuery,
      selectedCategory: "all",
      categories: [],
      query: booksQuery,
    };
  },
  methods: {
    selectCategory(category) {
      if (category === "all") {
        this.query = booksQuery;
      } else if (category === "featured") {
        this.query = booksFeaturedQuery;
      } else {
        this.query = categoryQuery;
      }
      this.selectedCategory = category;
    },
  },
};
</script>

<style>
.link-margin {
  margin-right: 24px;
}
.move-down {
  margin-bottom: 3em;
}
</style>
