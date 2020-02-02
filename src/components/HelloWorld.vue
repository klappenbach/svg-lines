<template>
  <div id="myDiv" @click="svg()" ref="myDiv">
    <h1>{{ msg }}</h1>

    
  </div>

</template>


<script lang="ts">
    import { Component, Prop, Vue } from 'vue-property-decorator';
    // import { TweenMax, Power1 } from 'gsap';



    @Component
    export default class HelloWorld extends Vue {
        @Prop() private msg!: string;
        private svg() {
            console.log(this.$refs.myDiv);
            // TweenMax.bezierThrough([{x:0, y:0, scaleX:0.5}, {x:100, y:-200, scaleX:1.2}, {x:300, y:20, scaleX:0.8}]);
        }
    }
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;


}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
