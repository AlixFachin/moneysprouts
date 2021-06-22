<template>
  <div id="expense-input-container">
    <div id="expense-input-form">
      <div class="verticalContainer">
        <h1>Expenses</h1>
        <p>Let's input an expense!</p>
        <div class="inputDiv">
          <input
            class="expense-input-button"
            type="date"
            name="date-input"
            v-model="posts.date"
          />
        </div>
        <div class="inputDiv">
          <input
            class="expense-input-button"
            type="text"
            name="amount-input"
            placeholder="Amount"
            v-model="posts.amount"
          />
        </div>
        <div class="inputDiv">
          <input
            class="expense-input-button"
            type="text"
            name="description-input"
            placeholder="Description"
            v-model="posts.description"
          />
        </div>
        <div id="category-button-container">
          <input
            @change="getCategory('Groceries')"
            type="radio"
            id="groceries"
            name="category"
            value="groceries"
          />
          <label class="category" for="groceries">Groceries</label>

          <input
            @change="getCategory('Bills')"
            type="radio"
            id="bills"
            name="category"
            value="bills"
          />
          <label class="category" for="bills">Bills</label>

          <input
            @change="getCategory('Transport')"
            type="radio"
            id="transport"
            name="category"
            value="transport"
          />
          <label class="category" for="transport">Transport</label>

          <input
            @change="getCategory('Misc')"
            type="radio"
            id="misc"
            name="category"
            value="misc"
          />
          <label class="category" for="misc">Misc</label>
        </div>
      </div>
      <div id="eco-action-container">
        <div class="icon-box">
          <p class="icon-info">I used an eco bag/ I didn't use a bag:</p>
          <input
            id="eco-bag-no-bag"
            class="eco-action-checkbox toggle"
            type="checkbox"
            name="eco-bag-no-bag"
            value="eco-bag-no-bag"
            @change="ecoGoalValidatorSetter('ecoBag')"
            v-model="posts.ecoGoal.eco_bag.boolean"
          />
          <label for="eco-bag-no-bag">
            <span class="eco-watch">
              <i class="fas fa-shopping-bag" title="Eco bag/ No bag used"></i>
            </span>
          </label>
        </div>
        <div class="icon-box">
          <p class="icon-info">I didn't impulse purchase anything:</p>
          <input
            id="no-impulse-buy"
            class="eco-action-checkbox toggle"
            type="checkbox"
            name="no-impulse-buy"
            value="no-impulse-buy"
            v-model="posts.ecoGoal.no_impulse.boolean"
            @change="ecoGoalValidatorSetter('noImpulse')"
          />
          <label for="no-impulse-buy">
            <span class="eco-watch">
              <i class="fas fa-tags" title="No impulse purchase made"></i>
            </span>
          </label>
        </div>
        <div class="icon-box">
          <p class="icon-info">I took eco-conscious transport:</p>
          <input
            title="Eco conscious transport"
            id="eco-conscious-transport"
            class="eco-action-checkbox toggle"
            type="checkbox"
            name="eco-conscious-transport"
            value="eco-conscious-transport"
            v-model="posts.ecoGoal.eco_conscious_transport.boolean"
            @change="ecoGoalValidatorSetter('ecoConsciousTransport')"
          />
          <label for="eco-conscious-transport">
            <span class="eco-watch">
              <i class="fas fa-biking" title="Eco conscious transport"></i>
            </span>
          </label>
        </div>
        <div class="icon-box">
          <p class="icon-info">I didn't take an eco action...</p>
          <input
            id="failed-eco-warrior"
            class="eco-action-checkbox toggle"
            type="checkbox"
            name="failed-eco-warrior"
            value="failed-eco-warrior"
            v-model="posts.ecoGoal.no_eco_action.boolean"
            @change="ecoGoalValidatorSetter('noEcoAction')"
          />
          <label for="failed-eco-warrior">
            <span class="eco-watch">
              <i class="far fa-frown" title="No action taken"></i>
            </span>
          </label>
        </div>
      </div>
    </div>
    <div>
      <button form="expense-input-form" @click="postExpenseData">Submit</button>
    </div>
  </div>
</template>

<script>
// helper function for date padding
function display2Digits(number) {
  if (number < 10) {
    return "0" + number;
  } else {
    return number;
  }
}

export default {
  name: "ExpenseInput",
  props: {},
  mounted() {
    // Setup the default date
    const currentDate = new Date();
    this.$data.posts.date = `${currentDate.getFullYear()}-${display2Digits(
      currentDate.getMonth() + 1
    )}-${display2Digits(currentDate.getDate())}`;
  },
  data() {
    return {
      warningNotice: null,
      posts: {
        date: null,
        amount: null,
        description: null,
        category: null,
        ecoGoal: {
          eco_bag: {
            name: "Eco bag/no bag",
            boolean: false,
          },
          no_impulse: {
            name: "No impulse purchase",
            boolean: false,
          },
          eco_conscious_transport: {
            name: "Eco conscious tranport",
            boolean: false,
          },
          no_eco_action: {
            name: "No action",
            boolean: true,
          },
        },
      },
    };
  },
  methods: {
    expenseValidator() {
      // Checking to make sure expense has a valid amount i.e is a number

      const isNumber = parseInt(this.posts.amount);

      if (isNaN(isNumber)) {
        this.warningNotice = "Not a valid amount";
        return false;
      }

      // Description is filled in

      if (!this.posts.description) {
        this.warningNotice = "Please add a description";
        return false;
      }

      // Category button is pressed

      if (!this.posts.category) {
        this.warningNotice = "Please click a button";
        return false;
      }

      return true;
    },
    postExpenseData(e) {
      if (this.expenseValidator()) {
        const categoryFilter = this.$store.state.categoriesList.filter(
          (category) => category.category_name === this.posts.category
        );

        const expenseData = {
          user_id: this.$store.state.user.user_id,
          category_id: categoryFilter[0].id,
          expense_description: this.posts.description,
          amount: parseInt(this.posts.amount),
          eco_goal: {},
        };

        for (let ecoGoal in this.posts.ecoGoal) {
          if (this.posts.ecoGoal[ecoGoal].boolean) {
            const ecoGoalObj = this.$store.state.ecoGoalsList.filter(
              (ecoGoalObj) =>
                ecoGoalObj.goal_name === this.posts.ecoGoal[ecoGoal].name
            );
            expenseData.eco_goal[this.posts.ecoGoal[ecoGoal].name] =
              ecoGoalObj[0].id;
          }
        }

        this.$store.dispatch("createExpense", expenseData);

        console.warn(this.posts);
        e.preventDefault();
      } else {
        alert(`${this.warningNotice}`);
      }
    },
    getCategory(category) {
      this.posts.category = category;
    },
    getEcoGoal(ecoGoal) {
      this.posts.ecoGoal = ecoGoal;
    },
    ecoGoalValidatorSetter(ecoGoalButtonValue) {
      if (ecoGoalButtonValue === "noEcoAction") {
        this.posts.ecoGoal.eco_bag.boolean = false;
        this.posts.ecoGoal.no_impulse.boolean = false;
        this.posts.ecoGoal.eco_conscious_transport.boolean = false;
      } else {
        this.posts.ecoGoal.no_eco_action.boolean = false;
      }

      if (ecoGoalButtonValue !== "noEcoAction") {
        if (
          this.posts.ecoGoal.eco_bag.boolean == false &&
          this.posts.ecoGoal.no_impulse.boolean == false &&
          this.posts.ecoGoal.eco_conscious_transport.boolean == false
        ) {
          this.posts.ecoGoal.no_eco_action.boolean = true;
        }
      }
    },
  },
};
</script>

<style scoped>
#expense-input-container {
  --button-color: #403d58;
  --second-color: #368f8b;
}

h1 {
  padding-top: 20px;
  margin-bottom: 0px;
}

p {
  font-size: large;
  font-weight: bold;
  color: var(--button-color);
}

.expense-input-button {
  margin-bottom: 1px;
  height: 1.5em;
  font-size: large;
  filter: none;
}

.icon-info {
  font-size: smaller;
}

.verticalContainer {
  display: flex;
  flex-direction: column;
}

.inputDiv {
  margin-bottom: 10px;
}

.inputDiv input {
  font-size: large;
}

input[type="radio"] {
  display: none;
  margin-top: 10px;
}
input[type="radio"]:not(:disabled) ~ label {
  cursor: pointer;
  border-radius: 5px;
}
input[type="radio"]:disabled ~ label {
  color: #bcc2bf;
  border-color: #bcc2bf;
  box-shadow: none;
  cursor: not-allowed;
}

.category {
  height: 100%;
  display: inline-grid;
  width: 50px;
  background: white;
  border: 2px solid var(--second-color);
  border-radius: 20px;
  padding: 1rem;
  margin: 1px;
  text-align: center;
  justify-content: center;
  box-shadow: 0px 3px 10px -2px rgba(161, 170, 166, 0.5);
  position: relative;
  z-index: 0;
  font-weight: bold;
}

input[type="radio"]:checked + label {
  background: var(--button-color);
  border: 1px solid var(--second-color);
  color: white;
  border: 0px;
  border-radius: 5px;
}
input[type="radio"]:checked + label::after {
  color: #3d3f43;
  font-family: FontAwesome;
  border: 1px solid var(--second-color);
  font-size: 22px;
  position: relative;
  top: -25px;
  left: 50%;
  transform: translateX(-50%);
  height: 20px;
  width: 0px;
  line-height: 20px;
  text-align: center;
  border-radius: 20%;
  background: white;
  box-shadow: 0px 2px 5px -2px rgba(0, 0, 0, 0.25);
  border-radius: 5px;
}

input[type="checkbox"] + label {
  position: relative;
  padding-left: 24px;
}

input[type="checkbox"] + label .eco-watch {
  position: absolute;
  padding-left: 24px;
}

.fas,
.far {
  height: 20px;
  width: 20px;
  margin: 2px;
  margin-left: 20px;
  font-size: 20px;
  display: inline-flex;
}

button {
  margin-top: 20px;
  margin-bottom: 10px;
  height: 2em;
  font-size: large;
  border-radius: 5px;
  border: 0px;
  background-color: #403d58;
  color: white;
  width: 100px;
}

input.eco-action-checkbox {
  position: absolute;
  visibility: hidden;
}

input.eco-action-checkbox + label {
  display: block;
  position: relative;
  cursor: pointer;
  outline: none;
  margin-left: 120px;
  user-select: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

input.toggle + label:before,
input.toggle + label:after {
  display: block;
  position: absolute;
  content: "";
}

input.toggle + label {
  width: 50px;
  height: 20px;
  background-color: rgb(245, 133, 63);
  transition: background 0.1s;
}

input.toggle + label:before {
  top: 2px;
  left: 2px;
  bottom: 2px;
  right: 2px;
  transition: background 0.1s;
}

input.toggle + label:after {
  top: 4px;
  left: 4px;
  bottom: 4px;
  width: 25px;
  background-color: #fff;
  transition: margin 0.5s, background 0.1s;
}

input.toggle:checked + label {
  background-color: rgb(111, 176, 42);
}

input.toggle:checked + label:after {
  margin-left: 40px;
  background-color: #fff;
}

@media only screen and (max-width: 768px) {
  section {
    flex-direction: column;
  }
}

@media (min-width: 768px) and (max-width: 2000px) {
  input.eco-action-checkbox + label {
    margin-left: 300px;
  }
}
</style>
