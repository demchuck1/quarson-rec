<template>
  <div id="app">
    <header>
      <div class="container">
        <div class="headerWrapper">
          <h1><a href="/" class="headerLogo"><img src="https://www.largus.fr/images/images/logo-qarson-rgb.png"></a></h1>
        </div>
      </div>
    </header>
    <div class="container">
    <ul class="catalog">
      <CatalogElement v-for="(offer,index) in offers" :offer="offer" @edit-element="editElement" :key="`key-${offer.id}`"/> 
    </ul>
    </div>
    <EditModal :offer="offers[currentEditId]" v-if="currentEditId > -1" @close-modal="closeModal" />
  </div>
</template>

<script>
import CatalogElement from './components/CatalogElement.vue'
import EditModal from './components/EditModal.vue'
import axios from "axios"

export default{
  name: 'App',
  data(){
    return{
      offers: [],
      currentEditId: Number,
    }
  },

  components:{
    CatalogElement,EditModal
  },

  async created(){    
    try{
      let results = await axios.get('http://localhost:3000/offers');
      results = await results.data;
      this.offers = results;
    } 
    catch (error){
      console.log(error);
    }
  },

  methods:{
    editElement(value){
      this.currentEditId = value;
    },
    closeModal(){
      this.rerenderComponent();
      this.currentEditId = -1;     
    },
    async rerenderComponent(){
      try{
        let result = await axios.get(`http://localhost:3000/offers/${this.currentEditId}`);
        result = await result.data;
        let index = this.offers.findIndex((offer) => offer.id == result.id);
        this.offers[index] = result;
        this.$forceUpdate();
      } 
      catch (error){
        console.log(error);
      }
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300;600&display=swap');
$baseName: ".catalog";
$green: #a5c93d;
body{
  font-family: 'Quicksand', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
  padding: 0;
}
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.container{
  max-width: 1200px;
  padding: 0 30px;
  margin: 0 auto;
}

button{
  &:hover{
    cursor: pointer;
  }
}

.headerLogo{
  max-width: 170px;
  display: block;
  margin: 45px 0;
  img{
    width: 100%;
  }
}

input{
  height: 40px;
  padding: 0 10px;
  line-height: 40px;
  border-radius: 4px;
  border: 1px solid rgba(0,0,0,.3);
}

.input{
  margin-bottom: 10px;
  display: flex;
  align-items: center;

  label{
    width: 150px;
  }

  &--withCheckbox{
    display: inline-flex;
    align-items: center;
    [type='checkbox']{
      margin-right: 10px;
      order: -1;
    }
  }

  @media(max-width: 600px){
    flex-direction: column;
    align-items: flex-start;
    label{
      margin-bottom: 5px;
    }
    input:not([type="checkbox"]){
      width: 100%;
    }
    &--withCheckbox{
      flex-direction: row;
      align-items: center;
      label{
        margin-bottom: 0;
      }
    }
  }
}

#{$baseName}{
  display: flex;
  flex-wrap: wrap;
  align-items: stretch;
  margin-right: -30px;
}

</style>
