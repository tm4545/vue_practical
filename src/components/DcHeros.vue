<template>  
<div class="w-full flex">
    <div class="m-auto">
   <h1 class="text-3xl text-center mt-10">DC Heroes {{ herosCount }}</h1>
  <ul class="mt-4"> 
    <li class="flex justify-between " v-for="(hero,index) in dcHeros" :key="hero.name">
      <div>
        {{ hero.name }}
      </div>
      <button @:click="remove(index)">x</button>
    </li>
  </ul>

  <form class="mt-10" @:submit.prevent="addHero">

  <input class="border rounded" v-model="newHero" placeholder="Type Hero Name Here"/>
  <button class="border rounded bg-gradient-to-r from-red-700 to-pink-500 text-white" type="submit">Add Hero</button>
</form></div></div>

</template>

<script>
export default {
    methods: {
    addHero() {
      if(this.newHero !== ''){
      
      this.dcHeros.unshift({ name:this.newHero })
      this.newHero= '';
    }
  },
  remove(index){
    this.dcHeros = this.dcHeros.filter((hero,i) => i !== index)

  },

  setFullname(){
      this.fullname = 'Jack Morgan';
  },

},
    computed: {
    

    herosCount(){
      return this.dcHeros.length;
    },},
    data(){
        return{
            dcHeros : [
      {name:'Batman'},{name:'Superman'},{name:'Flash'},
      {name:'Arrow'},
    ],

        }
    }
}
</script>

<style>

</style>