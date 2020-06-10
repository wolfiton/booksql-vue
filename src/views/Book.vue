<template>
  <div class="book">
    <h1>This is a Book page {{ $route.params.id }}</h1>
    <ApolloQuery
      :query="require('@/graphql/queries/Book.gql')"
      :variables="{ id: 1 }"
    >
      <!-- The result will automatically updated -->
      <template slot-scope="{ result: { data, loading } }">
        <!-- Some content -->
        <div v-if="loading">Loading...</div>
        <div v-else>
          <div>
            {{ data.book.title }}
          </div>
          <div>
            {{ data.book.author }}
          </div>
          <img :src="`http://localhost:8000/img/${data.book.image}`" alt="" />
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>
