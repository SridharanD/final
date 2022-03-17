<template>
  <div>
      <div> <h1>Config File Creation Page</h1></div>
      <div>
          <label>
             Organization Name 
          </label>
          </div>
          <div>
          <input required type="text" id="text" v-model="client" placeholder="Organization Name" > <br/>
          <button id='button' v-on:click="getDetails()">Submit</button>
     </div>
     <div > 
         <label>LocationName</label>
         <br/>
         <select id='dropdown' v-model="OrgLocation" displayName='Select Location'>
             
             <option v-for="item in info"  :value='item.locationName' :key='item.deviceID' >{{item.locationName}} </option>
         </select>
     </div>

      <div>
         <label>DeviceName</label>
         <br/>
         <select id='dropdown' v-model="devName">
             <option  v-for="item in infodetails"  :value='item.deviceName' :key='item.deviceID'  >{{item.deviceName}} </option>
         </select>
     </div>

     <div>
          <button id='button' v-on:click="Download()">Download Config File</button>
     </div>
</div>



  
</template>

<script>
import axios from 'axios'
import convert from 'html-to-text'
export default {
    name:"ClientConfig",
data:function() {
    return {

      client:'',
      details:{},
      info:{},
      infodetails:{},
      OrgLocation:null,
      devName:null,
    
    }
},

    methods : {
    InvalidClient:function(){
        if(this.info.length==0){
            alert('Please enter the valid Organization Name');
        }
    },
    getDetails:function(){
        axios.get('http://localhost:6060/api/Home/getGlobeDetails',{
            params:{
                client: this.client,
            }
        }).then((res)=>{
           this.info=res.data;
           if(this.info.length==0){
               alert("Please Enter the Valid Organization name");
           }
        }
        )    
    },
    Download:function(){
      const basetemplate=
      '<p> module.exports = {globeIdentification: "",globeDeployment: "", name: "GlobeChek_DESKTOP",version: "V3B", //Acceptable environment values: </p>';
      const text = convert(basetemplate, {wordwrap: false}); 
      
       const url = window.URL.createObjectURL(new Blob([text]));
       const link = document.createElement('a');
       link.href = url;
       link.setAttribute('download', 'config.js'); //or any other extension
       document.body.appendChild(link);
       link.click();
    },
    
},

watch:{
    OrgLocation:function(){
        return this.infodetails=this.info.filter(x=> x.locationName== this.OrgLocation);
    },

    devName:function(){
        return this.details=this.infodetails.filter(x=> x.deviceName== this.devName);
    },
   
}



}
</script>

<style>
#button {
 background-color: #04AA6D;
  border: none;
  color: white;
  padding: 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  border-radius: 12px;
  
}
#text {
  width: 23%;
  padding: 12px 20px;
  margin: 10px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 5px;
}
</style>

