<template>
  <div id="ATM">
    <div id="screen">
      {{ displayMessage }}
    </div>
    <div id="bottom">
      <div id="keypad" class="grid">
        <div class="button" @click="numBtnHandler('1')">1</div>
        <div class="button" @click="numBtnHandler('2')">2</div>
        <div class="button" @click="numBtnHandler('3')">3</div>
        <div class="button cancel" @click="cancelBtnHandler()">Cancel</div>
        <div class="button" @click="numBtnHandler('4')">4</div>
        <div class="button" @click="numBtnHandler('5')">5</div>
        <div class="button" @click="numBtnHandler('6')">6</div>
        <div class="button clear" @click="clearBtnHandler()">Clear</div>
        <div class="button" @click="numBtnHandler('7')">7</div>
        <div class="button" @click="numBtnHandler('8')">8</div>
        <div class="button" @click="numBtnHandler('9')">9</div>
        <div class="button enter" @click="enterBtnHandler()">Enter</div>
        <div class="button zero" @click="numBtnHandler('0')">0</div>
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
        pin: "1234",
        maxAllowableWithdraw: 1234,
        currentAmountToWithdraw: 0
      },
      displayMessage: "",
      pmNum: 1,
      timeout: 0,
      statuses: {
        monitorStatus: true,
        keypadStatus: true,
        cardReaderStatus: true,
        pinTrialRemaining: 3,
        entering: false
      },
      data: {
        pin: ""
      },
      keypad: {
        input: ""
      }
    };
  },
  computed: {
    buttonIsOne() {
      return this.buttonPressed === "1";
    },
  }, // dependent vars
  mounted() {
    setInterval(() => {
      if (this.timeout > 0) {
        this.timeout -= 100;
        if (this.timeout === 0) {
          this.display("Input timed out. Please try again");
          setTimeout(() => this.pmNum = 1, 3000);
        }
      }
      else {
        switch(this.pmNum) {
          case 1: // welcome
            this.welcome();
            break;
          case 2:

        }
      }
    }, 100);
  }, // lifecycle hooks
  methods: {
    // --------------- PM 1: Welcome ---------------
    welcome() {
      this.display("Welcome");
    },
    // --------------- PM 2: Check PIN ---------------
    checkPIN() {
      if (this.pinTrialRemaining > 0) {
        if (this.pin !== this.account.pin) {
          this.display("Wrong PIN. Do you want to try again?");
          this.statuses.pinTrialRemaining--;
        }
        else {
          this.pmNum = 3;
        }
      }
      else {
        this.display("Invalid PIN");
        this.pmNum = 7;
      }
    },
    // --------------- PM 3: Withdraw Amount ---------------
    inputWithdrawAmount() {

    },
    // --------------- Helper Functions ---------------
    display(msg) {
      this.displayMessage = msg;
    },
    numBtnHandler(num) {
      this.pmNum = 0;
      this.keypad.input += num;
      this.display(this.keypad.input);
    },
    enterBtnHandler() {

    },
    cancelBtnHandler() {

    },
    clearBtnHandler() {
      this.clearKeypadInput();
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
