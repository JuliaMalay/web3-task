<template>
  <div>
    <Header :options="options" :selected="selected" @select="optionSelect" />
    <Test />
  </div>
</template>

//
<script>
import Header from '@/components/Header.vue'
import Test from '@/components/Test.vue'
import 'assets/styles/variables.css'

export default {
  components: {
    Header,
    Test,
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
