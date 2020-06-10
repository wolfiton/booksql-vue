<template>
  <div class="create">
    <form action="#" method="POST" @submit.prevent="addBook">
      <div class="form-group">
        <label for="title">Title</label>
        <input type="text" name="title" id="title" v-model="title" />
      </div>
      <div class="form-group">
        <label for="author">Author</label>
        <input type="text" name="author" id="author" v-model="author" />
      </div>
      <div class="form-group">
        <label for="image">Image</label>
        <input type="text" name="image" id="image" v-model="image" />
      </div>

      <div class="form-group">
        <label for="description">Description</label>
        <textarea
          name="description"
          id="description"
          cols="30"
          rows="10"
          v-model="description"
        ></textarea>
      </div>

      <div class="form-group">
        <label for="link">Link</label>
        <input type="text" name="link" id="link" v-model="link" />
      </div>

      <div class="form-group">
        <label>
          <input
            type="checkbox"
            name="featured"
            id="featured"
            v-model="featured"
          />Featured</label
        >
      </div>

      <div class="form-group">
        <ApolloQuery
          :query="require('@/graphql/queries/Categories.gql')"
          class="move-down"
        >
          <template slot-scope="{ result: { data, loading } }">
            <!-- Some content -->
            <div v-if="loading">Loading...</div>
            <select v-else v-model="categoryId">
              <option
                v-for="category of data.categories"
                :key="category.id"
                :value="category.id"
              >
                {{ category.name }}
              </option>
            </select>
          </template>
        </ApolloQuery>
      </div>

      <div class="form-group">
        <button type="submit">Add Book</button>
      </div>
    </form>
  </div>
</template>

<script>
import addBook from "@/graphql/mutations/AddBook.gql";
export default {
  data() {
    return {
      title: "",
      author: "",
      image: "",
      description: "",
      link: "",
      featured: false,
      categoryId: 1,
    };
  },
  methods: {
    addBook() {
      this.$apollo
        .mutate({
          // Query
          mutation: addBook,
          // Parameters
          variables: {
            $title: this.title,
            $image: this.image,
            $author: this.author,
            $description: this.description,
            $link: this.link,
            $featured: this.featured,
            $categoryId: this.categoryId,
          },
        })
        .then((data) => {
          // Result
          console.log(data);
        })
        .catch((error) => {
          // Error
          console.error(error);
        });
    },
  },
};
</script>

<style>
.form-group {
  margin-bottom: 30px;
}

input[type="text"] {
  padding: 10px 14px;
}

button {
  padding: 16px;
  background: #027bff;
  color: white;
  border-radius: 5px;
  font-size: 16px;
}
</style>
