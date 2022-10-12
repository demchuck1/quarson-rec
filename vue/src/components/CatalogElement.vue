<template>
    <li class="catalogElement" :class="{ 'catalogElement--disabled': !offer.availability }">        
        <picture class="catalogElement__picture">
          <img v-if="offer.photo" :src="offer.photo" @error="setAlternateImage">
          <img v-else src="@/assets/404.jpg">
        </picture>
        <div class="catalogElement__data">       
          <h3 class="catalogElement__name">{{ offer.make }} {{ offer.model }}</h3>
          <div>{{ offer.engine }}</div>
          <div class="catalogElement__options">            
            <button class="catalogElement__actionButton catalogElement__availability" @click="changeAvailability(offer.id)">
              <template v-if="offer.availability">Dezaktywuj</template>
              <template v-else>Aktywuj</template>            
            </button>  
            <button class="catalogElement__actionButton catalogElement__edit" @click="$emit('edit-element', offer.id)">
              Edytuj 
            </button>  
          </div>
        </div>             
      </li>
  </template>
  
<script>
import axios from "axios";
export default{
    name: 'CatalogElement',
    props:{
        offer:{},
    },
    created() {
        this.$root.$refs.CatalogElement = this;
    },  
    methods:{
        setAlternateImage(event){
            const src = require('@/assets/404.jpg');            
            event.target.src = src;
        },
        changeAvailability(id){
            let status = this.offer.availability = !this.offer.availability;
            try{
                axios.patch(`http://localhost:3000/offers/${this.offer.id}`,{ availability: status});
            } catch (error){
                console.log(error);
            }
        },
        editElement(){
            alert("asd");
        }
    }
}
</script>
  
<style lang="scss">
$baseName: ".catalog";
$green: #a5c93d;
#{$baseName}{
  display: flex;
  flex-wrap: wrap;
  align-items: stretch;
  margin-right: -30px;
  &Element{
    width: calc(25% - 30px);
    padding: 0 15px 15px;
    margin: 0 30px 30px 0;
    display: inline-flex;
    flex-direction: column;
    justify-content: flex-end;
    text-align: left;
    box-shadow: 0 0 20px rgba(0,0,0,.1);
    border-radius: 4px;
    position: relative;

    @media(max-width: 991px){
      width: calc(33% - 30px);
    }
    @media(max-width: 768px){
      width: calc(50% - 30px);
    }
    
    @media(max-width: 539px){
      width: 100%;
    }

    &--disabled{
      opacity: .45;
      transition: opacity .2s;
      &:hover{
        opacity: 1;
      }
    }
    &__picture{
      display: block;
      margin: 0 -15px;
      img{
        max-width: 100%;
      }
    }
    &__data{
      padding: 15px 0 0 0;
    }

    &__name{
        margin: 0 0 10px 0;
    }

    &__options{
      display: flex;
      align-items: baseline;
      gap: 10px;
      margin-top: 15px;
    }
    &__actionButton{
      font-family: inherit;
      font-weight: bold;
      width: 90px;
      height: 40px;
      border-radius: 4px;
      border: 0;
      background: $green;
      color: #fff;
    }
  }
}
</style>
  