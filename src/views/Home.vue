<template>
  <div class="home">
    <h1 class="title is-2">
      Marduk Voting
    </h1>
    <h2 class="subtitle is-4">
      Proof of concept voting app
    </h2>
    <br />
    <p class="has-text-centered">
      <router-link
        class="button is-medium is-info is-outlined"
        :to="{ name: 'create' }"
      >
        Create Poll
      </router-link>
    </p>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",

  props: ["socket"],

  mounted() {
    console.log(this.socket);
  }
};
</script>
