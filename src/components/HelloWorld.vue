<template>
  <div class="hello">
    <!-- <div v-for="dog in dogs" :key="dog.id">
      {{ dog.name }}
    </div>

    <input v-model="dogName" type="text" />

    <button @click="onClicked">Press me</button> -->

    <ApolloQuery
      :query="
        (gql) => gql`
          query dogs {
            dogs {
              id
              name
            }
          }
        `
      "
    >
      <template v-slot="{ result: { loading, error, data } }">
        <div v-if="loading" class="loading apollo">Loading...</div>
        <div v-else-if="error" class="error apollo">An error occurred</div>
        <div v-else-if="data" class="result apollo">
          <div v-for="dog in data.dogs" :key="dog.id">
            {{ dog.name }}
          </div>
        </div>
        <div v-else class="no-result apollo">No result :(</div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "HelloWorld",
  methods: {
    async onClicked() {
      await this.$apollo.mutate({
        mutation: gql`
          mutation($label: String!) {
            createDog(data: { name: $label }) {
              name
            }
          }
        `,
        variables: {
          label: this.dogName,
        },
      });
    },
  },
  data() {
    return {
      dogName: "",
    };
  },
  apollo: {
    dogs: gql`
      query {
        dogs {
          id
          name
        }
      }
    `,
  },
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
