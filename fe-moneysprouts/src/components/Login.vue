<template>
  <div class="login-container">
    <h2>Let's go <span class="green-emphasis">green</span>!</h2>
    <input
      type="text"
      v-model="currentUser.userName"
      id="username-input"
      name="username-input"
      placeholder="username..."
    />
    <input
      v-on:keyup.enter="validateUserInput"
      type="password"
      v-model="currentUser.password"
      id="password-input"
      name="password-input"
      placeholder="password..."
    />
    <div class="button-row">
      <button @click="validateUserInput">Login</button>
      <button id="create-account" v-on:click="showSignUp">Sign Up</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  props: {
    msg: String,
  },
  data() {
    return {
      currentUser: {
        userName: "",
        password: "",
      },
    };
  },
  methods: {
    validateUserInput() {
      this.$store.dispatch("verifyLogin", {
        username: this.currentUser.userName,
        password: this.currentUser.password,
      });
    },
    showSignUp() {
      this.$store.commit("setShowsToFalse");
      this.$store.commit("showSignUp");
    },
  },
};
</script>

<style scoped>
.login-container {
  border-radius: 50%;
  border: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  width: 400px;
  height: 400px;
  box-sizing: border-box;
  
  animation: panel-appear 0.8s cubic-bezier(0.28, 0.19, 0.68, 2.19);
}

@keyframes panel-appear {
  from {
    transform: scale(0);
  }
  to {
    transform: scale(1);
  }
}

#login-background {
  width: 400px;
  height: 400px;
  position: fixed;
  top: 
}

@media (max-width: 450px) {
  .login-container {
    border-radius: 100px;
    width: 90%;
    height: auto;
    padding: 5px;
  }
}

input {
  margin-bottom: 1.5em;
  height: 2em;
  font-size: large;
  filter: none;
  width: 60%;
}

.green-emphasis {
  color: hsl(166, 37%, 10%);
  animation-name: color-flicker;
  animation-duration: 2s;
  animation-iteration-count: infinite;
  animation-direction: alternate-reverse;
  animation-timing-function: linear;
}

@keyframes color-flicker {
  0% {
    color: hsl(166, 37%, 10%);
  }
  100% {
    color: hsl(166, 37%, 50%);
  }
}

button {
  height: 2em;
  font-size: large;
  border-radius: 5px;
  background-color: #403d58;
  color: white;
  width: 70%;
  margin-left: 5px;
  margin-right: 5px;
  margin-bottom: 1em;
}

h2 {
  margin-top: 0;
}

#sign-up {
  font-size: smaller;
  background-color: lightcyan;
}

.button-row {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  width: 90%;
}
</style>
