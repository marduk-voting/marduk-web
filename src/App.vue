<template>
  <section class="hero is-fullheight is-default is-bold">
    <div class="hero-head">
      <nav class="navbar">
        <div class="container">
          <div class="navbar-brand">
            <a class="navbar-item" href="../">
              Marduk
            </a>
            <span class="navbar-burger burger" data-target="navbarMenu">
              <span></span>
              <span></span>
              <span></span>
            </span>
          </div>
          <div id="navbarMenu" class="navbar-menu">
            <div class="navbar-end">
              <div class="tabs is-right">
                <ul>
                  <router-link
                    :to="{ name: 'home' }"
                    tag="li"
                    active-class="is-active"
                    axact
                  >
                    <a href="#">Home</a>
                  </router-link>

                  <router-link
                    :to="{ name: 'create' }"
                    tag="li"
                    active-class="is-active"
                  >
                    <a href="#">Create</a>
                  </router-link>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </nav>
    </div>
    <div class="hero-body">
      <div class="container has-text-centered">
        <div class="columns is-vcentered">
          <div class="column is-5">
            <figure class="image">
              <img src="./assets/marduk-and-pet.jpg" alt="Description" />
            </figure>
          </div>
          <div class="column is-6 is-offset-1">
            <router-view :socket="socket"></router-view>
          </div>
        </div>
      </div>
    </div>

    <div class="hero-foot">
      <div class="container">
        <div class="tabs is-centered">
          <ul>
            <li><a>Marduk App</a></li>
          </ul>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import io from "socket.io-client";
// import jwt from "jsonwebtoken";

export default {
  name: "App",

  data() {
    return {
      socket: null
    };
  },

  provide() {
    return {
      socket: this.socket
    };
  },

  created() {
    this.socket = io("localhost:3000");
    this.socket.on("connect", this.onConnect);
    this.socket.on("invalidToken", this.onInvalidToken);
    // this.socket.on("votingCreated", this.onVotingCreated);
    // this.socket.on("updateVoting", console.log.bind(undefined, "update"));
  },

  methods: {
    onConnect() {
      // TODO
    },

    onVotingCreated({ id, options, tokens }) {
      console.log(id, options, tokens);
      // const [token] = tokens;
      // const data = jwt.decode(token);
      // console.log(data);
      // this.socket.emit("listenVoting", { votingId: data.votingId });
      // this.socket.emit("vote", {
      //   token,
      //   vote: 1
      // });
    },

    onInvalidToken() {
      this.$buefy.toast.open({
        message: "Invalid token!",
        position: "is-bottom",
        type: "is-danger"
      });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
