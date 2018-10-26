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