<template>
  <div class="hello">
    <img src="../assets/logo.png" />
    <h1>
      <p>{{ name }}'s Skills</p>
    </h1>

    <div class="holder">
      <!-- Input Skill Form -->
      <ValidationObserver ref="form">
        <form @submit.prevent="addSkill">
          <ValidationProvider rules="min:5" v-slot="{ errors }" name="skill">
            <input
              type="text"
              placeholder="Enter a new skill..."
              v-model="skill"
            />
            <transition
              name="pop-in"
              enter-active-class="animated flipInX"
              leave-active-class="animated flipOutX"
            >
              <p v-if="errors.length > 0" class="alert">{{ errors[0] }}</p>
              <span v-else>{{ skill }}</span>
            </transition>
          </ValidationProvider>
        </form>
      </ValidationObserver>

      <!--  Display Skill -->
      <ul>
        <transition-group
          name="list"
          enter-active-class="animated bounceInUp"
          leave-active-class="animated bounceOutDown"
        >
          <li v-for="(data, index) in skills" :key="index + 0">
            {{ data.skill }}
            <span style="cursor:pointer;">
              <i class="fa fa-minus-circle" v-on:click="remove(index)"></i
            ></span>
          </li>
        </transition-group>
      </ul>

      <P>These are the skills that you process.</P>
    </div>
  </div>
</template>

<script>
import { ValidationProvider, ValidationObserver } from "vee-validate";
import { extend } from "vee-validate";

extend("min", {
  validate(skill, args) {
    return skill.length >= args.length;
  },
  params: ["length"],
  message: "This {_field_} field must be atleast {length} charactors."
});

export default {
  name: "Skill",
  components: {
    ValidationProvider,
    ValidationObserver
  },
  data() {
    return {
      name: "3xbun",
      checked: false,
      skill: "",
      skills: [
        { skill: "Vue.js" },
        { skill: "html" },
        { skill: "css" },
        { skill: "Javascript" }
      ]
    };
  },
  methods: {
    addSkill() {
      this.$refs.form.validate().then(success => {
        if (!success) {
          return;
        } else {
          this.skills.push({ skill: this.skill });
          this.skill = "";
        }
      });
    },
    remove(id) {
      this.skills.splice(id, 1);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

img {
  height: 128px;
  display: block;
  margin: 2em auto 0;
}

.holder {
  background: #fff;
}

ul {
  margin: 0;
  padding: 0;
  list-style-type: none;
}

ul li {
  display: flex;
  justify-content: space-between;
  padding: 20px;
  font-size: 1.3em;
  background-color: #e0edf4;
  border-left: 5px solid #3eb3f6;
  margin-bottom: 2px;
  color: #3e5252;
}

p {
  text-align: center;
  padding: 30px 0;
  color: gray;
}

.container {
  box-shadow: 0px 0px 40px lightgray;
}

input {
  width: calc(100% - 40px);
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  background-color: #323333;
  color: #687f7f;
}

.alert {
  background: #fdf2ce;
  font-weight: bold;
  display: inline-block;
  padding: 5px;
  margin-top: -20px;
}

@import "https://cdn.jsdelivr.net/npm/animate.css@3.5.1";

.pop-in-enter-active {
  animation: bounce-in 0.5s;
}
.pop-in-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
</style>
