<template>
  <ProjectFront msg="Employee Database"/>
</template>

<script>
import ProjectFront from './components/ProjectFront.vue'

export default {
  name: 'App',
  components: {
    ProjectFront
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 40px;
}
h1 {
  font-size: 4rem;
  color: white;
}
html{
  background: url('./assets/aaron-burson-wood-background.jpg') no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  color: white;
  text-shadow: -.5px .5px 0 black,
        .5px .5px 0 black,
        .5px -.5px 0 black,
        -.5px -.5px 0 black;
}
</style>
