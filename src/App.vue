<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" width="25%" />
    <HelloWorld msg="Hello Vue in CodeSandbox!" />
    <IPDCompChart :expandable="compExpanded" />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";
import IPDCompChart from "./components/ipd-comp-chart/ipd-comp-chart";

export default {
  name: "App",
  components: {
    HelloWorld,
    IPDCompChart,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
