<template>
  <div class="hello">
    <div v-if="isLoading" id='Loader'>
      <img src="https://media4.giphy.com/media/3oEjI6SIIHBdRxXI40/200.gif"/>
    </div>
   <div class="row">
     <div class="col-2">
        <p class="fonthead">#</p >
     </div>
      <div class="col-3">
        <p class="fonthead">PlaceName</p>
     </div>
      <div class="col-3">
        <p class="fonthead">CityId</p>
     </div>
      <div class="col-3">
        <p class="fonthead">Country</p>
     </div>
     <hr/>
     <div class="row" v-for="item,index in places" :key="item.PlaceId">
       <div class="col-2">
        <p class="fonthead">{{index+1}}</p>
     </div>
      <div class="col-3">
        <p>{{item.PlaceName}}</p>
     </div>
      <div class="col-3">
        <p>{{item.CityId}}</p>
     </div>
      <div class="col-3">
        <p>{{item.CountryName}}</p>
     </div>
          <hr/>
     </div>
     <div>
     <h4 v-if='isNoResult' class="text-danger">No Result found</h4>
     <input v-model="searchvalue" :disabled="disabled" @keyup.enter="searchValue"  placeholder="search PlaceName" class="search"/>
     <button class="btn-color ml" v-if="this.searchvalue==''" @click="disabled=!disabled">Start Searching</button>   
     </div>
   </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      places:[],
      searchvalue:"",
      isNoResult:false,
      isLoading:false,
      disabled: true
    }
  },
  methods:{
    searchValue(){
      let result = this.places.filter((item)=>{
        return item.PlaceName.includes(this.searchvalue)
      })
      this.places = result
    },
    callApi(){
        const self = this;
        self.isLoading =true;
          var options = {
          method: 'GET',
          url: 'https://skyscanner-skyscanner-flight-search-v1.p.rapidapi.com/apiservices/autosuggest/v1.0/UK/GBP/en-GB/',
          params: {query: 'Delhi'},
          headers: {
            'x-rapidapi-key': '4ac5e3352fmshe6ac515ca3b8ccap1f0045jsnf0a504a87bbe',
            'x-rapidapi-host': 'skyscanner-skyscanner-flight-search-v1.p.rapidapi.com'
          }
        };
        axios.request(options).then(function (response) {
          var data = response.data.Places;
         for(let items of data){
           self.places.push(items)
         }
           self.isLoading =false;
        //  console.log(self.places);
        }).catch(function (error) {
          console.error(error);
        });   
    }
  },
   created() {
      this.callApi();
    },
    watch:{
      searchvalue(val){
        console.log(val)
        if(val==''){
          this.disabled=true;
          this.places=[]
          this.callApi()
        }  
      },
       places(val){
          val.length?this.isNoResult=false:this.isNoResult=true
        }
    }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.ml{
  margin-left: 2%;
}
.btn-color{
   width: 241px;
   height: 38px; 
   font-size: 16px; 
   display: inline-block;
    font-weight: 400;
    line-height: 1.5;
    text-align: center;
    text-decoration: none;
    user-select: none;
    vertical-align: middle;
    border: 1px solid transparent;
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    border-radius: 0.25rem;
    transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out, border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
    color: #fff;
    background-color: rgb(33, 37, 41);
    border-color: #0d6efd;
}
.search{
   width: 241px;
   height: 38px; 
   font-size: 16px; 
   background-color: rgb(255, 255, 255);
   border-color: rgb(206, 212, 218);
   padding: 6px 12px 6px 12px;
   border-radius: 4px
}
.search:active:focus{
  border-color: #7952b3;
  box-shadow: 0 0 0 3px rgb(121 82 179 / 25%);
}
.search:disabled{
  Background-color: rgb(233, 236, 239)
}
.fonthead{
  font-weight: 700
}
</style>
