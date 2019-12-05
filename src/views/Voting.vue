<template>
  <div class="home">
    <h1 class="title is-2">
      {{ canVote ? "Voting" : "Watching a poll" }}
    </h1>
    <h2 class="subtitle is-4">
      Live updates for votes
    </h2>

    <br />

    <div
      class="card"
      v-for="(option, i) in options"
      :key="i"
      style="margin-bottom: 10px"
    >
      <div class="card-content">
        <p class="title">"{{ option }}"</p>
      </div>
      <footer class="card-footer">
        <p class="card-footer-item">
          <span> {{ groupedVotes[i] }} Vote(s) </span>
        </p>

        <p v-if="canVote" class="card-footer-item">
          <b-button
            type="is-primary"
            @click="vote(i)"
            :disabled="currentVote && currentVote.vote === i"
          >
            Vote
          </b-button>
        </p>
      </footer>
    </div>
  </div>
</template>

<script>
import jwt from "jsonwebtoken";

export default {
  name: "Voting",

  props: ["socket"],

  data() {
    const { token } = this.$route.params;
    const { votingId, userId, options } = jwt.decode(token);
    return {
      id: votingId,
      token,
      userId,
      options,
      votes: {}
    };
  },

  computed: {
    canVote() {
      return this.userId !== 0;
    },

    groupedVotes() {
      const votesValues = Object.values(this.votes);
      return this.options.map(
        (_, i) => votesValues.filter(({ vote }) => vote === i).length
      );
    },

    currentVote() {
      return this.votes[this.userId];
    }
  },

  created() {
    this.socket.emit("listenVoting", { votingId: this.id });
    this.socket.on("updateVoting", this.onUpdate);
  },

  methods: {
    onUpdate({ votes }) {
      this.$set(this, "votes", votes);
    },

    vote(vote) {
      this.socket.emit("vote", {
        token: this.token,
        vote
      });
    }
    // create() {
    // const { voters } = this;
    // const options = this.optionsString.split("\n");
    // this.socket.emit("createVoting", {
    //   voters,
    //   options
    // });
    // this.socket.once("votingCreated", this.onVotingCreated);
    // },
    // onVotingCreated({ id, options, tokens }) {
    // console.log(id, options, tokens);
    // this.$router.push({
    //   name: "voting",
    //   params: {
    //     id
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
    // }
  }
};
</script>
