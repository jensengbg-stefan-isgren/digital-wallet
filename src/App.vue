<template>
  <div id="app">
    <transition name="fade" mode="out-in">
      <router-view />
    </transition>
  </div>
</template>

<style lang="scss">
body {
  padding: 0;
  margin: 0;
}

#app {
  padding: 0;
  margin: 0;
  display: flex;
  height: 100vh;
  justify-content: center;
  align-items: center;
}

.fade-enter-active,
.fade-leave-active {
  transition-duration: 0.3s;
  transition-property: opacity;
  transition-timing-function: ease;
}

.fade-enter,
.fade-leave-active {
  opacity: 0;
}
</style>
