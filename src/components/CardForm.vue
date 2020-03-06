<template>
  <form class="card_form" autocomplete="off" id="add_card">
    <label for="card_number">
      Card number
      <span
        :class="validate.cardNumber ? 'valid': 'notValid'"
      >{{messages.cardNumberMessage}}</span>
    </label>
    <input
      @keyup="validateCardNumber"
      name="card_number"
      class="card_holder"
      :value="cardNumberValue"
      @input="changedCardNumber"
      id="card_number"
      maxlength="16"
      onfocus="this.value=''"
      type="text"
    />
    <label for="card_holder">
      Cardholder name
      <span
        :class="validate.cardHolder ? 'valid': 'notValid'"
      >{{messages.cardHolderMessage}}</span>
    </label>
    <input
      @keyup="validateCardHolder"
      name="card_holder"
      :value="cardHolderValue"
      type="text"
      id="card_holder"
      placeholder="FIRSTNAME LASTNAME"
      @input="changedCardHolder"
      onfocus="this.value=''"
    />

    <label for="valid_date">
      Valid thru
      <span
        :class="validate.validThru ? 'valid': 'notValid'"
      >{{messages.validThruMessage}}</span>
    </label>
    <input
      @keyup="validateValidThru"
      name="valid_thru"
      maxlength="5"
      :value="validThruValue"
      type="text"
      id="valid_date"
      @input="changedValidThru"
      onfocus="this.value=''"
    />
    <label for="cvv_code">
      Cvv
      <span :class="validate.cvvCode ? 'valid': 'notValid'">{{messages.cvvMessage}}</span>
    </label>
    <input
      @keyup="validateCvv"
      maxlength="3"
      :value="cvvValue"
      type="text"
      id="cvv_code"
      @input="changedCvvValue"
    />
    <select @change="validateVendor" v-model="card.selected" name="vendors" id="vendor_select">
      <option value>You must choose vendor</option>
      <option value="bitcoin">Bitcoin</option>
      <option value="ninja">Ninja</option>
      <option value="blockchain">Block Chain</option>
      <option value="evilcorp">Evil Corp</option>
    </select>
    <div class="buttons">
      <button class="add_btn" type="button" @click.prevent="updateCards">Add Card</button>
      <button class="wallet_btn" type="button" @click="$router.push('/')">Wallet</button>
    </div>
  </form>
</template>

<script>
export default {
  name: "cardform",
  components: {},
  props: {
    cardNumberValue: String,
    cardHolderValue: String,
    validThruValue: String,
    cvvValue: String
  },
  data: () => {
    return {
      card: {
        cardNumber: "",
        cardHolder: "",
        validThru: "",
        cvvCode: "",
        selected: ""
      },
      validate: {
        cardNumber: false,
        cardHolder: false,
        validThru: false,
        cvvCode: false,
        selected: false
      },
      messages: {
        cardNumberMessage: "",
        cardHolderMessage: "",
        validThruMessage: "",
        cvvMessage: ""
      }
    };
  },
  watch: {
    selected(value) {
      this.$emit("cardLogo", value);
    }
  },
  methods: {
    validateVendor() {
      if (this.selected == "") {
        this.validate.selected = false;
      } else {
        this.validate.selected = true;
      }
    },
    validateCvv() {
      let cvvValidate = /^[1-9]{3}$/;
      let cvvCode = document.getElementById("cvv_code").value;

      if (cvvValidate.test(cvvCode)) {
        this.validate.cvvCode = true;
        this.messages.cvvMessage = "";
      } else {
        this.validate.cvvCode = false;
        return (this.messages.cvvMessage = "cvv is 3 digits, keep going");
      }
    },
    validateValidThru() {
      let dateValidate = /^(0[1-9]|1[012])\/\d{2}$/;
      let validDate = document.getElementById("valid_date").value;

      if (dateValidate.test(validDate)) {
        this.validate.validThru = true;
        this.messages.validThruMessage = "";
      } else {
        this.validate.validThru = false;
        return (this.messages.validThruMessage = "Format needs to be MM/YY");
      }
    },
    validateCardNumber() {
      let cardNumberValidate = /\d{16}/;
      let cardNumber = document.getElementById("card_number").value;

      if (cardNumberValidate.test(cardNumber)) {
        this.validate.cardNumber = true;
        this.messages.cardNumberMessage = "";
      } else {
        this.validate.cardNumber = false;
        return (this.messages.cardNumberMessage =
          "kindly fill out 16 digits, thanks");
      }
    },
    validateCardHolder() {
      let cardHolderValidate = /^[A-Za-z ]+$/;
      let cardHolder = document.getElementById("card_holder").value;

      if (cardHolderValidate.test(cardHolder)) {
        this.validate.cardHolder = true;
        this.messages.cardHolderMessage = "";
      } else {
        this.validate.cardHolder = false;
        return (this.messages.cardHolderMessage = "Hey, only letters here");
      }
    },
    changedCardNumber() {
      this.card.cardNumber = event.target.value;
      this.$emit("changeCardNumber", this.card.cardNumber);
    },
    changedCardHolder() {
      this.card.cardHolder = event.target.value;
      this.$emit("changeCardHolder", this.card.cardHolder);
    },
    changedValidThru() {
      this.card.validThru = event.target.value;
      this.$emit("changeValidThru", this.card.validThru);
    },
    changedCvvValue() {
      this.card.cvvCode = event.target.value;
      this.$emit("changeCvvValue", this.card.cvvCode);
    },
    updateCards() {
      this.validateVendor();
      this.validateCardNumber();
      this.validateCardHolder();
      this.validateValidThru();
      this.validateCvv();
      this.validateVendor();

      if (
        this.validate.cardNumber == false ||
        this.validate.cardHolder == false ||
        this.validate.validThru == false ||
        this.validate.cvvCode == false ||
        this.validate.selected == false
      ) {
        return;
      } else {
        this.$emit("updateCard", this.card);
        let form = document.querySelector(".card_form");
        form.reset();
      }
    }
  },
  computed: {
    selected() {
      return this.card.selected;
    }
  }
};
</script>

<style lang="scss" scoped>
.add_btn {
  margin-right: 10px;
  background-color: #000000;
  border-radius: 8px;
  outline: none;
  width: 181px;
  height: 50px;
  border: none;
  font-family: "PT Mono";
  color: #ffffff;
  font-weight: bold;
  font-size: 22px;
  margin-top: 10px;
}

.wallet_btn {
  margin-left: 10px;
  background-color: #000000;
  border-radius: 8px;
  outline: none;
  width: 181px;
  height: 50px;
  border: none;
  font-family: "PT Mono";
  color: #ffffff;
  font-weight: bold;
  font-size: 22px;
  margin-top: 10px;
}

.buttons {
  margin-bottom: 10px;
  display: flex;
  justify-content: center;
}

label {
  margin-bottom: -14px;
  font-family: "PT Mono";
  font-size: 12px;
  color: rgba(0, 0, 0, 0.8);
  text-transform: uppercase;
}

input {
  font-family: "PT Mono";
  margin: 14px 0;
  text-align: center;
  width: 382px;
  font-size: 18px;
  height: 40px;
  border-radius: 8px;
  border-color: rgba(0, 0, 0, 0.8);
  border-width: 1px;
  outline: none;
}

select {
  width: 382px;
  height: 40px;
  border-radius: 8px;
  border-color: rgba(0, 0, 0, 0.8);
  border-width: 1px;
  outline: none;
}

input::placeholder {
  font-size: 18px;
  font-family: "PT Mono";
  text-align: center;
}

.card_form {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
}

input[name="card_number"] {
  color: white;
}

input[name="card_number"]:focus {
  color: black;
}

input[name="valid_thru"] {
  color: white;
}

input[name="valid_thru"]:focus {
  color: black;
}

.valid {
  color: green;
}

.notValid {
  color: red;
}
</style>
