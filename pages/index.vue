<template>
  <div>
    <Header :options="options" :selected="selected" @select="optionSelect" />
    <TestCopy />
  </div>
</template>

//
<script>
import Header from '@/components/Header.vue'
import TestCopy from '@/components/TestCopy.vue'
import 'assets/styles/variables.css'

export default {
  components: {
    Header,
    TestCopy,
  },
  data() {
    return {
      options: [
        { name: 'Ru', value: 'ru' },
        { name: 'Eng', value: 'eng' },
      ],
      selected: 'Eng',
    }
  },
  methods: {
    optionSelect(option) {
      this.selected = option.name
    },
  },
}
</script>
