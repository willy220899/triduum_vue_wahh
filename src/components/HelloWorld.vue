<template>
  <div class="hello" style="font-family: 'Star Wars', sans-serif;">
    <vs-row vs-justify="center">
        <vs-col  v-for="character in characters" :key="character.id" style="width: 33.33%; display: flex; margin-top: 4rem;" type="flex" vs-justify="center" vs-align="center" vs-w="6">
          <vs-card style="width: 25rem!important; height: 35rem!important; background-color: #3c3e44; color: #f5f5f5;" actionable class="cardx">
            <div @click="openPopupActivo(character)" slot="header">
              <h1>
                {{ character.name }}
              </h1>
            </div>
            <div @click="openPopupActivo(character)" slot="media">
              <img :src="character.imagen" height="300px">
            </div>
            <div @click="openPopupActivo(character)">
              <h2>Age: {{ character.age }}</h2>
              <h2>Gender: {{ character.gender }}</h2>
              <h2>Height: {{ character.height }} Cm</h2>
              <h2>Mass: {{ character.mass }} Kg</h2>
            </div>
            <div @click="openPopupActivo(character)" slot="footer">
              <vs-row vs-justify="flex-end">
                <vs-button color="#dba90f" type="filled" icon="theaters"></vs-button>
              </vs-row>
            </div>
          </vs-card>
        </vs-col>
    </vs-row>
    <vs-popup style="color:rgb(255,255,255)" background-color="rgba(0,0,0,.6)" :background-color-popup="colorx" :title="popupData.title" :active.sync="popupActivo">
      <vs-collapse type="margin">
        <vs-collapse-item v-for="item in popupData.films" :key="item.id">
          <div style="color:#dba90f;" slot="header">
            {{item.title}}
          </div>
          <h4 style="color:#dba90f;">Opening Crawl:</h4><h4>{{ item.opening_crawl }}</h4><br>
          <h4 style="color:#dba90f;">Planets:</h4><h4 v-for="planet in item.planetas" :key="planet.id">{{ planet.name }}</h4><br>
          <h4 style="color:#dba90f;">Directors:</h4><h4 v-for="director in item.directors" :key="director">{{ director }}</h4><br>
          <h4 style="color:#dba90f;">Producers:</h4><h4 v-for="producer in item.producers" :key="producer">{{ producer }}</h4><br>
        </vs-collapse-item>
      </vs-collapse>
    </vs-popup>
  </div>
</template>

<script>
import Vue from 'vue'
import Vuesax from 'vuesax'
import axios from 'axios'
import 'material-icons/iconfont/material-icons.css';

import 'vuesax/dist/vuesax.css' //Vuesax styles
Vue.use(Vuesax, {
  // options here
})
export default {
  name: 'HelloWorld',
  props: {
  },
  data(){
			return{
        characters:[],
        popupActivo:false,
        colorx:"#272b33",
        characterSelect:'',
        popupData:{
          title:'',
          films:[]
        },
      }
  },
		methods:{
      getCharacters(){
        this.$vs.loading({
          type:'point'
        })
        axios.get('http://localhost:8000/api/v1/characters/?page_size=30')
          .then(res=>{
            this.characters=res.data.results;
            this.$vs.loading.close()
            })
            .catch(err=>{
              console.log(err)
              this.$vs.loading.close()
            });
      },
      openPopupActivo(item){
        this.$vs.loading({
          type:'point'
        })
        this.popupActivo=true;
        this.popupData.title=item.name;
        axios.get('http://localhost:8000/api/v1/films/?character_id='+item.id)
          .then(res=>{
            this.popupData.films=res.data.results;
            this.$vs.loading.close()
            })
            .catch(err=>{
              console.log(err)
              this.$vs.loading.close()
            });
        
      }
    },
    mounted(){
      this.getCharacters();
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello{
  background-color: #272b33;
}
.cardx:hover{
  color: #dba90f!important;
  border: #dba90f solid 3px;
}
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
