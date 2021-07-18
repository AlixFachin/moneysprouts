<template>
  <div id="app" v-bind:class="{ jsLoading: isLoading }">
    <img
      src="@/assets/green-palmtree.jpg"
      v-if="this.isLoading"
      @load="allowAnimations"
      width="5px"
    />
    <div class="header">
      <h1 id="moneySproutsTitle">MoneySprouts</h1>
      <MainAnimation />
    </div>
    <div>
      <div v-bind:class="mainPanelClass">
        <!-- we display the LOGIN component if no user is currently active -->
        <user-message-display />
        <Login class="panel" v-if="this.$store.state.showLogin" />
        <BudgetVisualization
          class="panel"
          v-if="this.$store.state.showBudgetVisualization"
        />
        <BarChartScreen class="panel" v-if="this.$store.state.showBarChart" />
        <ExpenseInput class="panel" v-if="this.$store.state.showExpenseInput" />
        <SignUp class="panel" v-if="this.$store.state.showSignUp" />
        <BudgetInput class="panel" v-if="this.$store.state.showBudgetInput" />
      </div>
    </div>
    <div class="nav-bar">
      <div
        v-if="this.$store.state.userName !== ''"
        id="footer-button-container"
      >
        <button
          v-on:click="showBudgetVisualization"
          class="footer-button"
          name="profile"
          value="profile"
        >
          <i id="home-icon" class="fas fa-home"></i>
        </button>
        <button
          v-on:click="showExpenseInput"
          class="footer-button"
          name="expense-input"
          value="expense-input"
        >
          <i id="dollar-icon" class="fas fa-dollar-sign"></i>
        </button>
        <button
          v-on:click="showBudgetInput"
          class="footer-button"
          name="monthly-budget"
          value="monthly-budget"
        >
          <i id="calendar-icon" class="far fa-calendar-alt"></i>
        </button>
        <button
          v-on:click="showBarChart"
          class="footer-button"
          name="eco-action-chart"
          value="eco-action-chart"
        >
          <i id="chart-icon" class="fas fa-chart-bar"></i>
        </button>
        <button
          v-on:click="logout"
          class="footer-button"
          name="logout"
          value="logout"
        >
          <i id="signout-icon" class="fas fa-sign-out-alt"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Login from "./components/Login.vue";
import SignUp from "./components/SignUp.vue";
import MainAnimation from "./components/mainAnimation.vue";
import UserMessageDisplay from "./components/UserMessageDisplay";
/* DYNAMIC COMPONENT LOADING  */
/*
import ExpenseInput from "./components/ExpenseInput.vue";
import BudgetInput from "./components/BudgetInput.vue";
import BarChartScreen from "./components/BarChartScreen.vue";
*/
/* LAZY COMPONENT LOADING */
const BudgetVisualization = () => import(/* webpackChunkName: "BudgetVisualization" */ "@/components/BudgetVisualization.vue");
const ExpenseInput = () => import(/* webpackChunkName: "ExpenseInput" */ "@/components/ExpenseInput.vue");
const BudgetInput = () => import( /* webpackChunkName: "BudgetInput" */ "@/components/BudgetInput.vue");
const BarChartScreen = () => import(/* webpackChunkName: "BarChartScreen" */ "@/components/BarChartScreen.vue");

export default {
  name: "App",
  metaInfo: {
    title: "MoneySprouts",
    htmlAttrs: {
      lang: "en-US",
    },
    meta: [
      { charset: "utf-8" },
      {
        name: "description",
        content:
          "Expense tracking software which enables you to track your ecological footprint as well",
      },
      { name: "viewport", content: "width=device-width, initial-scale=1" },
    ],
  },
  data() {
    return {
      isLoading: true,
    };
  },
  components: {
    Login,
    BudgetVisualization,
    ExpenseInput,
    BudgetInput,
    UserMessageDisplay,
    BarChartScreen,
    SignUp,
    MainAnimation,
  },
  methods: {
    showBudgetVisualization() {
      this.$store.commit("setShowsToFalse");
      this.$store.commit("showBudgetVisualization");
    },
    showExpenseInput() {
      this.$store.commit("setShowsToFalse");
      this.$store.commit("showExpenseInput");
    },
    showBudgetInput() {
      this.$store.commit("setShowsToFalse");
      this.$store.commit("showBudgetInput");
    },
    showBarChart() {
      this.$store.commit("setShowsToFalse");
      this.$store.commit("showBarChart");
    },
    showSignUp() {
      this.$store.commit("setShowsToFalse");
      this.$store.commit("showSignUp");
    },
    logout() {
      this.$store.commit("clearUserName");
      this.$store.commit("setShowsToFalse");
      this.$store.commit("showLogin");
    },
    allowAnimations() {
      // Once the App is mounted (=loaded???) then we can start the animations
      console.log("Document loaded!");
      setTimeout(() => {
        console.log("Baoum");
        this.isLoading = false;
      }, 500);
    },
  },
  mounted() {
    const externalScript = document.createElement("script");
    externalScript.setAttribute(
      "src",
      "https://kit.fontawesome.com/e3cbc00358.js"
    );
    externalScript.setAttribute("crossorigin", "anonymous");
    document.head.appendChild(externalScript);
  },
  computed: {
    mainPanelClass: function () {
      // Computed function to return the main Panel class dynamically
      // https://vuejs.org/v2/guide/class-and-style.html
      // Checking if the current component is included into the list of components
      // requiring the main Panel to be centered vertically
      const isCentered =
        this.$store.state.showLogin ||
        this.$store.state.showSignUp ||
        this.$store.state.showBarChart ||
        this.$store.state.showBudgetInput;
      return {
        "main-panel": true,
        contentCentered: isCentered,
      };
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Carme&family=Lato&display=swap");
body {
  margin: 0;
  padding: 0;
  /* Gradients fallback if the viewport is bigger than the ap (e.g. iPad)  */
  background: #11998e; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to bottom,
    #dff106,
    #63a50a
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to bottom,
    #dff106,
    #63a50a
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  display: flex;
  flex-direction: column;
  align-items: center;
}

/*  Pause animations while we load the pictures */
.jsLoading *,
.jsLoading *:before,
.jsLoading *:after {
  animation-play-state: paused !important;
}

h1 h2 {
  font-family: "Carme", sans-serif;
  color: #403d58;
}

button:active {
  background-color: rgb(111, 176, 42);
  transform: translateY(2px);
}

#app {
  --app-max-width: 500px;
  --header-color: #d7efbd;

  /* Variable calculation for positioning */
  --header-footer-height: max(10vh, 60px);

  max-width: var(--app-max-width);
  font-family: "Lato", sans-serif;

  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  min-height: 100vh;
  width: 100vw;

  background: url("assets/green-palmtree.jpg") top repeat-y;
  background-size: auto 100%;
}

#moneySproutsTitle {
  float: left !important;
  margin-left: 10px;
  font-size: 35px;
}

.main-panel {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  /* Height of the panel is the whole size - footer height */
  min-height: calc(100vh - 2 * var(--header-footer-height));
  /* The main panel has to be displayed from the bottom of the header, i.e
     header-content-height + 2* padding */
  margin-top: var(--header-footer-height);
  margin-bottom: var(--header-footer-height);
  text-align: center;
}

.contentCentered {
  justify-content: center;
}

.panel {
  padding: 5px;
  background-color: rgba(255, 255, 255, 0.6);
  backdrop-filter: blur(2px);
  /* min-width: 90%; */
  border-radius: 5px;
  margin-top: 10px;
  margin-bottom: 40px;
}

.header {
  /* Header position: ABSOLUTE (to always stay on top) */
  position: fixed;
  top: 0;
  width: 100vw;
  max-width: var(--app-max-width);
  box-sizing: border-box;
  height: var(--header-footer-height);
  background-color: var(--header-color);
  display: flex; /* header has a flex display in order to center the title vertically */
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  z-index: 5;
}

.header i {
  color: green;
}

.nav-bar {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 10px;
  box-sizing: border-box;
  position: fixed;
  bottom: 0;
  width: 100vw;
  max-width: var(--app-max-width);
  height: var(--header-footer-height);
  background: var(--header-color);
}

#footer-button-container {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  padding: 10px;
}

.footer-button {
  font-size: 2em;
  min-width: 60px;
  min-height: 60px;
  padding: 5px;
  text-align: center;
  border-radius: 12%;
  border: 3px solid #403d58;
  background-color: #403d58;
}

#home-icon,
#dollar-icon,
#calendar-icon,
#signout-icon,
#chart-icon {
  color: white;
}
</style>
