<template>
  <div id="app" style="display: flex;">
    <Input @update="podcast = $event"/>
    <Output :podcast="podcast"/>
  </div>
</template>

<script>
import Input from './components/Input.vue'
import Output from './components/Output.vue'

export default {
  components: {
    Input,
    Output
  },
  data() {
    return {
      podcast: {}
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

html,
body {
  margin: 0;
}

#app {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100%;
  line-height: 1.4;
  min-height: 100vh;
}

header {
  flex: 0;
  padding: 1rem 5%;
}

.container {
  display: flex;
  position: relative;
  flex-direction: column;
  flex-shrink: 0;
  width: 50%;
}

main,
form {
  flex: auto;
  padding: 1rem 5%;
}

@media screen and (min-height: 768px) {
  .container {
    height: 100vh;
  }

  main,
  form {
    overflow-y: scroll;
  }
}

textarea {
  font-family: Monaco, monospace;
}

input,
select,
textarea {
  display: block;
  margin: 0.25rem 0;
  padding: 0.5rem;
}

input,
textarea {
  width: 100%;
}

input,
select {
  font-size: 1rem;
}

label {
  display: block;
  margin-bottom: 1rem;
  font-size: 0.875rem;
  font-weight: bold;
}

label.error input,
label.error select,
label.error textarea {
  background-color: rgb(255, 240, 240);
}

label code {
  font: 0.875em Monaco, monospace;
  color: rgb(80, 60, 60);
  word-wrap: break-word;
}

h2 {
  font-size: 1.25rem;
  text-align: center;
  margin: 0;
}

.numeral {
  display: block;
  font-size: 1.5em;
}

h3,
h4 {
  text-align: center;
  margin: 0 0 0.25rem;
}

h3,
h4 {
  font-size: 1rem;
}

h4 {
  font-weight: normal;
  font-size: 1rem;
}

.cover-art {
  display: block;
  margin: 0 auto 1rem;
  width: 100%;
  max-width: 12rem;
  max-height: 12rem;
  object-fit: contain;
  filter: drop-shadow(0 0.75rem 0.75rem rgba(0, 0, 0, 0.35));
}

.error {
  color: rgb(200, 0, 0);
}

.note {
  margin: 0.5rem 0 0.75rem;
  padding: 0.5rem 0.875rem;
  border-radius: 0.25rem;
  background-color: rgba(0, 0, 0, 0.05);
  color: rgb(110, 100, 100);
  font-size: 0.875em;
  font-weight: normal;
}

.tc {
  text-align: center;
}

.wrap {
  white-space: wrap;
}
</style>