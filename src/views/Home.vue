<template>
  <section class="show_wallet">
    <top :pageTitle="'E-WalLet'" :subTitle="'Active Card'" />
    <card
      class="active_card"
      :cardNumber="activeCard.cardNumber"
      :cardHolder="activeCard.cardHolder"
      :imgPath="activeCard.selected"
      :validThru="activeCard.validThru"
      :cvvCode="activeCard.cvvCode"
    />
    <CardStack
      :cards="cards"
      @card="setActive"
      :active="activeCard"
      @resetActive="resetActive"
    />
  </section>
</template>

<script>
import top from "../components/Top";
import card from "../components/Card";
import CardStack from "../components/CardStack";

export default {
  name: "home",
  components: {
    top,
    CardStack,
    card
  },
  data: () => {
    return {
      activeCard: {},
      cards: JSON.parse(localStorage.getItem("cards"))
    };
  },
  methods: {
    setActive(value) {
      this.activeCard = value[0];
      return this.activeCard;
    },
    resetActive() {
      this.activeCard = {};
    }
  }
};
</script>

<style lang="scss" scoped>
.active_card {
  margin-bottom: 120px;
}

.show_wallet {
  height: 100vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}

.active_card {
  transform: translateY(-50px);
}
</style>
