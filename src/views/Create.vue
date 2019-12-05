<template>
  <div>
    <template v-if="!tokens.length">
      <h1 class="title is-2">
        Create a poll
      </h1>
      <h2 class="subtitle is-4">
        Setup options and how many voters
      </h2>

      <br />

      <p class="has-text-centered">
        <b-field>
          <b-numberinput v-model="voters" :min="2"></b-numberinput>
        </b-field>

        <b-input
          type="textarea"
          placeholder="Separate options by line"
          v-model="optionsString"
        ></b-input>

        <a @click="create" class="button is-medium is-info is-outlined">
          Create
        </a>
      </p>
    </template>

    <template v-else>
      <h1 class="title is-2">
        Poll created
      </h1>
      <h2 class="subtitle is-4">
        Send tokens to users
      </h2>

      <br />

      <p class="has-text-centered">
        <router-link :to="{ name: 'voting', params: { token: tokens[0] } }">
          <span class="tag is-success is-large">Read-only access</span>
        </router-link>
      </p>

      <p class="has-text-centered">
        <router-link
          v-for="(token, i) in tokens.slice(1)"
          :key="i"
          :to="{ name: 'voting', params: { token: token } }"
        >
          <span
            class="tag is-info is-large"
            style="display: block; width: 100%; margin: 10px 5px"
            >Voter #{{ i + 1 }}</span
          >
        </router-link>
      </p>
    </template>
  </div>
</template>

<script>
export default {
  name: "Create",

  props: ["socket"],

  data() {
    return {
      voters: 10,
      optionsString: "",
      tokens: []
    };
  },

  methods: {
    create() {
      const { voters } = this;
      const options = this.optionsString.split("\n");

      this.socket.emit("createVoting", {
        voters,
        options
      });

      this.socket.once("votingCreated", this.onVotingCreated);
    },

    onVotingCreated({ id, options, tokens }) {
      console.log(id, options, tokens);
      // const readOnlyToken = tokens[0];

      this.tokens = tokens;

      // this.$router.push({
      //   name: "voting",
      //   params: {
      //     token: readOnlyToken
      //   }
      // });
      // const [token] = tokens;
      // const data = jwt.decode(token);
      // console.log(data);
      // this.socket.emit("listenVoting", { votingId: data.votingId });
      // this.socket.emit("vote", {
      //   token,
      //   vote: 1
      // });
    }
  }
};
</script>
