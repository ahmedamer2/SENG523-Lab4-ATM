<template>
  <div id="ATM">
    <div id="screen">
      {{ buttonPressed }}
      <span v-if="buttonIsOne">Button is 1</span>
      <span v-else>Button is not 1</span>
    </div>
    <div id="bottom">
      <div id="keypad" class="grid">
        <div class="button" v-on:click="changeButton('1')">1</div>
        <div class="button" @click="changeButton('2')">2</div>
        <div class="button">3</div>
        <div class="button cancel">Cancel</div>
        <div class="button">4</div>
        <div class="button">5</div>
        <div class="button">6</div>
        <div class="button clear">Clear</div>
        <div class="button">7</div>
        <div class="button">8</div>
        <div class="button">9</div>
        <div class="button enter">Enter</div>
        <div class="button zero">0</div>
      </div>
      <div id="cardScanner">cardScanner</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
      default: null,
    },
  },
  data() {
    //independent vars
    return {
      buttonPressed: "",
      account: {
        name: "ahmed",
      },
    };
  },
  computed: {
    buttonIsOne() {
      return this.buttonPressed === "1";
    },
  }, // dependent vars
  mounted() {}, // lifecycle hooks
  methods: {
    changeButton(num) {
      this.buttonPressed = num;
      setTimeout(() => {
        console.log("hi");
      }, 1000);
    },
    withdraw() {
      this.acccount["name"] = "hefoiwejf";
    },
  }, // functions
};
</script>

<style lang="less" scoped>
#ATM {
  height: 85%;
  width: 65rem;
  margin: 0 auto;
  padding: 2rem;
  border: 2px solid black;
  font-family: "Barlow-Regular";

  #screen {
    height: 45%;
    border: 2px solid grey;
    width: 95%;
    margin: 0 auto 2rem;
    padding: 1em;
  }

  #bottom {
    display: flex;
    width: 70%;
    margin: 0 auto;
    gap: 1em;

    #keypad {
      font-family: "Barlow-SemiBold";
      font-size: 1.2rem;
      width: 65%;
      padding: 1rem;
      outline: 1px solid black;
      text-align: center;
      &.grid {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
        gap: 1.5rem;

        .button {
          outline: 1px solid #555;
          padding: 1rem;
          box-shadow: 2px 2px 3px 2px #bbb;
          border-radius: 5px;
          cursor: pointer;

          &:hover {
            background: #ccc;
          }

          &.zero {
            grid-column: 1/4;
          }

          &.enter {
            grid-row: 3/5;
            grid-column: 4;
            background: #2bb82b;

            &:hover {
              background: #44c244;
            }
          }

          &.clear {
            background: #ebeb38;
            &:hover {
              background: #e0e030;
            }
          }

          &.cancel {
            background: #ff2b2b;
            &:hover {
              background: #e22929;
            }
          }
        }
      }
    }

    #cardScanner {
      width: 30%;
      outline: 1px solid black;
      height: 2rem;
      margin: auto 0;
      padding: 1em;
    }
  }
}
</style>
