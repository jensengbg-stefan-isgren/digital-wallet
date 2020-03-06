<template>
  <section class="top_section">
    <h1 class="page_title">{{pageTitle}}</h1>
    <p class="sub_title">{{subTitle}}</p>
  </section>
</template>

<script>
export default {
  name: "top",
  props: {
    pageTitle: String,
    subTitle: String
  }
};
</script>

<style lang="scss" scoped>
.page_title {
  margin: 0px;
  text-align: center;
  font-family: "Source Sans Pro";
  text-transform: uppercase;
  font-weight: bold;
}

.sub_title {
  text-align: center;
  font-family: "Source Sans Pro";
  font-size: 12px;
  line-height: 15px;
  font-weight: bold;
  color: rgba(34, 34, 34, 0.6);
}
</style>