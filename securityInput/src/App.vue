<template>
  <div id="app">
    <security-input :length="6" @securityInput="securityInput"></security-input>
    <p id="currentInput">
      当前输入的是:{{ currentInputValue }}
    </p>
  </div>
</template>

<script>
import SecurityInput from '@/components/SecurityInput'

export default {
  name: 'App',
  data() {
    return {
      currentInputValue: ''
    }
  },
  components: {
    SecurityInput
  },
  methods: {
    securityInput(input) {
      this.currentInputValue = input.map(item => item.value).join('');
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#currentInput{
  float: left;
}
</style>
