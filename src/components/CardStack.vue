<template>
  <section class="card_section">
    <section class="card">
      <card
        v-for="(card, index) in cards"
        :key="index"
        :cardNumber="card.cardNumber"
        :cardHolder="card.cardHolder"
        :validThru="card.validThru"
        :cvvCode="card.cvvCode"
        :imgPath="card.selected"
        @setActive="activeCard(card)"
      />
    </section>
    <section class="buttons">
      <button
        class="add_btn"
        type="button"
        @click.prevent="$router.push('/addcard')"
      >
        Add Card
      </button>
      <button @click="deleteCard" class="remove_btn" type="button">
        Remove Card
      </button>
    </section>
  </section>
</template>

<script>
import card from "../components/Card";
export default {
  name: "CardStack",
  props: {
    cards: Array,
    active: Object
  },
  components: {
    card
  },
  data: () => {
    return {};
  },
  methods: {
    activeCard(card) {
      let setActive = this.cards.filter(activeCard => {
        return activeCard.cardNumber == card.cardNumber;
      });
      this.$emit("card", setActive);
    },
    deleteCard() {
      let items = JSON.parse(localStorage.getItem("cards"));
      for (let i = 0; i < items.length; i++) {
        if (items[i].cardNumber == this.active.cardNumber) {
          let position = items.indexOf(items[i]);
          items.splice(position, 1);
          this.cards.splice(position, 1);
          localStorage.setItem("cards", JSON.stringify(items));
          this.$emit("resetActive");
        }
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.add_btn {
  margin-right: 10px;
  background-color: white;
  border-radius: 8px;
  outline: none;
  width: 181px;
  height: 50px;
  font-family: "PT Mono";
  color: black;
  font-weight: bold;
  font-size: 22px;
  margin-top: 20px;
  border: 1px solid #000000;
  box-sizing: border-box;
  border-radius: 8px;
}

.remove_btn {
  margin-left: 10px;
  background-color: white;
  border-radius: 8px;
  outline: none;
  width: 181px;
  height: 50px;
  font-family: "PT Mono";
  color: black;
  font-weight: bold;
  font-size: 22px;
  margin-top: 20px;
  border: 1px solid #000000;
  box-sizing: border-box;
  border-radius: 8px;
}

.buttons {
  margin-bottom: 10px;
}

.card {
  display: grid;
  grid-auto-rows: 40px;
  transform: translateY(-100px);
}
</style>
