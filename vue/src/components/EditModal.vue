<template>
    <div class="editModal">     
        <div class="editModal__wrapper">
            <div class="editModal__content">
                <button type="button" class="editModal__closeButton" @click="$emit('close-modal', -1)"></button>
                <div class="editModal__inputs">                
                <div class="input">
                    <label for="photoURL">URL zdjęcia</label>
                    <input type="text" v-model="offerTemportary.photo" id="photoURL">
                </div>
                <div class="input">
                    <label for="make">Marka</label>
                    <input type="text" v-model="offerTemportary.make" id="make">
                </div>
                <div class="input">
                    <label for="model">Model</label>
                    <input type="text" v-model="offerTemportary.model" id="model"></div>                  
                <div class="input">
                    <label for="engine">Oznaczenie silnika</label>
                    <input type="text" v-model="offerTemportary.engine" id="engine">
                </div>                
                <div class="input input--withCheckbox">
                    <label for="availability">Dostępność</label>
                    <input type="checkbox" id="availability" v-model="offerTemportary.availability">
                </div>
            </div>
            <figure class="editModal__preview">
                <img src="asd" :src="offerTemportary.photo" @error="setAlternateImage">
            </figure>  
            </div>              
            <div class="editModal__options">
                <button class="catalogElement__actionButton catalogElement__availability" @click="deleteItem">
                    Usuń          
                </button>  
                <button class="catalogElement__actionButton catalogElement__edit" @click="applyChanges">
                    Zatwierdź          
                </button> 
            </div>            
        </div>     
    </div>
</template>
  
<script>
import axios from "axios"
export default {
  name: 'EditModal',
    data() {
        return {
            offerTemportary: {...this.offer},
        }
    },
    props: {
        offer: {}
    },
    methods: {
        async applyChanges(){
        //zatwierdzamy zmiany w bazie
            await axios.patch(`http://localhost:3000/offers/${this.offerTemportary.id}`, this.offerTemportary);
            this.$emit('close-modal',-1);           
        },
        async deleteItem(){
            //usuwamy element z bazy
            await axios.delete(`http://localhost:3000/offers/${this.offerTemportary.id}`);
            this.$emit('close-modal',-1);           
        },
        setAlternateImage(event) {
            // zapożyczenie metody z komponentu CatalogElement.vue
            this.$root.$refs.CatalogElement.setAlternateImage(event);
        }      
    }
}
</script>
  
<style lang="scss">
  $baseName: ".editModal";
    #{$baseName}{
      position: fixed;
      left: 0;
      right: 0;
      bottom: 0;
      top: 0;
      background: rgba(0,0,0,.7);
      &__wrapper{
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
        width: 800px;
        max-width: 100%;
        background: #fff;
        padding: 100px 30px 30px;
        border-radius: 4px;
        max-width: 100%;
        @media(max-width: 830px){
          top: 0;
          right: 0;
          bottom: 0;
          left: 0;
          transform: none;
        }
    }
    &__inputs{
      display: flex;
      flex-direction: column;
    }
    &__options{
      display: flex;
      justify-content: space-between;
    }
    &__content{
      display: flex;
      gap: 30px;
      @media(max-width: 830px){
        flex-direction: column;   
        margin-bottom: 30px;
      }
    }
    &__preview{
      width: 450px;
      img{
        max-width: 100%;
      }
      @media(max-width: 830px){
        width: 100%;
      }
    }
    &__closeButton{
      width: 30px;
      height: 30px;
      background: url('https://cdn-icons-png.flaticon.com/512/1828/1828778.png') center center;
      background-size: contain;
      position: absolute;
      right: 30px;
      top: 30px;
      border: 0;
      outline: 0;   
    }
  }
  </style>
  