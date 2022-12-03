<template>
  <div id="ATM">
    <div id="screen">
      <div v-if="processNumber == 1" class="state welcome">
        Welcome Please Insert Your Card
      </div>
      <div v-if="processNumber == 2" class="state checkPin">
        <div class="prompt">Please enter your 4 digit Pin Code</div>
        <div class="pinInputs">
          <div class="dash">
            <span v-if="this.buttonPressed.length >= 1" class="asterisk">
              *
            </span>
          </div>
          <div class="dash">
            <span v-if="this.buttonPressed.length >= 2" class="asterisk">
              *
            </span>
          </div>
          <div class="dash">
            <span v-if="this.buttonPressed.length >= 3" class="asterisk">
              *
            </span>
          </div>
          <div class="dash">
            <span v-if="this.buttonPressed.length >= 4" class="asterisk">
              *
            </span>
          </div>
        </div>
      </div>
    </div>
    <div id="bottom">
      <div id="keypad" class="grid">
        <div class="button" @click="keypadHandler('1')">1</div>
        <div class="button" @click="keypadHandler('2')">2</div>
        <div class="button" @click="keypadHandler('3')">3</div>
        <div class="button cancel" @click="keypadHandler('cancel')">Cancel</div>
        <div class="button" @click="keypadHandler('4')">4</div>
        <div class="button" @click="keypadHandler('5')">5</div>
        <div class="button" @click="keypadHandler('6')">6</div>
        <div class="button clear" @click="keypadHandler('clear')">Clear</div>
        <div class="button" @click="keypadHandler('7')">7</div>
        <div class="button" @click="keypadHandler('8')">8</div>
        <div class="button" @click="keypadHandler('9')">9</div>
        <div class="button enter" @click="keypadHandler('enter')">Enter</div>
        <div class="button zero" @click="keypadHandler('0')">0</div>
      </div>
      <div id="cardScanner" @click="welcome(true)">Card Scanner</div>
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
      enterPressed: false,
      account: {
        name: "Jhon Doe",
        pin: "1234",
        balance: 2500,
      },
      atm: {
        balance: 2000,
        bills: {
          5: 0,
          10: 0,
          20: 0,
          50: 0,
          100: 0,
        },
      },
      processNumber: 1,
      cardInserted: false,
      mainLoop: null,
      count: 0,
      seconds: 0,
      buttonTimer: null,
    };
  },
  computed: {
    buttonIsOne() {
      return this.buttonPressed === "1";
    },
  }, // dependent vars
  mounted() {
    // this will start up the process when the page loads
    this.mainLoop = setInterval(() => this.dispatcher(), 0); // runs the dispatcher every 10ms
  },
  unmounted() {
    clearInterval(this.mainLoop);
  },
  methods: {
    dispatcher() {
      switch (this.processNumber) {
        case 1:
          this.welcome();
          break;
        case 2:
          this.checkPin();
          break;
      }

      this.count++;
      if (this.count === 250) {
        // this val can be changed to make the clock faster
        // 300 counts seems to be approximately 1 sec
        this.seconds++;
        this.count = 0;
      }
    },
    welcome(cardInserted) {
      this.seconds = 0;
      this.count = 0;
      if (cardInserted) {
        this.processNumber = 2;
        this.cardInserted = false;
      }
    },
    checkPin() {
      if (this.seconds > 3) {
        this.processNumber = 1;
        this.buttonPressed = "";
        this.seconds = 0;
      }
    },
    keypadHandler(val) {
      if (val === "cancel") {
        this.buttonPressed = "";
      } else if (val === "clear") {
        this.buttonPressed = this.buttonPressed.slice(
          0,
          this.buttonPressed.length - 1
        );
        return;
      } else if (val === "enter") {
        this.enterPressed = true;
        return;
      } else {
        this.buttonPressed += val;
      }
      this.seconds = 0;
    },
    clearKeypadInput() {
      this.keypad.input = "";
    }
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
    background-color: #47a6ff;

    .state {
      display: flex;
      width: 100%;
      height: 100%;
      justify-content: center;
      align-items: center;
    }

    .welcome {
      font-family: "Barlow-Bold";
      font-size: 3rem;
    }

    .checkPin {
      font-family: "Barlow-Normal";
      font-size: 2rem;
      display: flex;
      flex-direction: column;
      justify-content: center;
      gap: 2em;

      .prompt {
        font-family: "Barlow-Bold";
      }
      .pinInputs {
        display: flex;
        gap: 1em;
        .dash {
          border-bottom: 1px solid black;
          width: 1.5em;
          text-align: center;

          .asterisk {
            font-family: "Barlow-Bold";
            font-size: 3rem;
          }
        }
      }
    }
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
      padding: 1rem;
      text-align: center;
      font-size: 1.2rem;
      font-family: "Barlow-Bold";
      cursor: pointer;
    }
  }
}
</style>
