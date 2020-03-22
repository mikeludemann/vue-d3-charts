<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <barChart 
      id="bar" 
      widthSize="720" 
      heightSize="480" 
      marginLeft="60" 
      marginTop="30"
      marginRight="40" 
      marginBottom="50" 
      :data='[{"country":"Germany","resident":"82790000"},{"country":"Netherlands","resident":"17080000"},{"country":"Austria","resident":"8773000"},{"country":"Switzerland","resident":"8420000"}]'
     />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import barChart from '@/components/bar-chart.vue';

export default {
  name: 'Home',
  components: {
    HelloWorld,
    barChart
  }
}
</script>
