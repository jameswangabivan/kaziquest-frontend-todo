<template>
  <HomeComp @theme="getName"/>
</template>

<script>
import HomeComp from './components/HomeComp.vue'
import { watchEffect } from 'vue'


var currentTheme = "index";

export default {
  name: 'App',
  components: {
    HomeComp
  },
  methods: {
    getName(value){
      currentTheme = value;
    },
  },


}
watchEffect(() => {
  import(`@/style/${currentTheme}.scss`);
});
</script>

