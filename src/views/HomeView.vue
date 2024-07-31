<script setup>
import { reactive, computed } from "vue";

const data = reactive({
  state: true,
  inputName: "",
  names: [],
  error: "",
  showError: false,
  results: "",
});
const handleAdd = () => {
  const userName = data.inputName;
  if(validate(userName))
{
  data.names.push(userName);
  data.inputName = "";
  data.showError = false;
}
else{
  data.showError = true;
}
};

const validate = (value) => {
  data.error = "";
  if (value === "") {
    data.error = "Sorry, no emtpy name";
    return false;
  }
  if (data.names.includes(value)) {
    data.error = "Sorry, names must be unique";
    return false;
  }
  return true;
};
const handleDelete = (index) => {
  data.names.splice(index, 1);
};

const isReady = computed(() =>{
 return data.names.length > 1
}) 
const getRandomName = () => {
  return data.names[Math.floor(Math.random() * data.names.length)]
}

const generateResult = () =>
{
  let rand = getRandomName();

  if(data.results!=='')
{
  while(rand===data.results)
{
   rand = getRandomName ()
}
}

  data.results= rand
}
const showResults= () => {
  generateResult()
  data.state = false
}

const resetApp = () => {
  data.state= true;
  data.inputName= "";
  data.names= [];
  data.error= "";
  data.showError= false;
  data.results= "";
}

const getNewResult = () => {
  generateResult()
}


</script>

<template>
  <main>
    <div id="names" class="container" v-if="data.state">
      <div class="logo">
        <h1>Who pays the bill?</h1>
      </div>
      <div class="input-container">
        <input type="text" v-model.trim="data.inputName" />
        <button type="button" @click="handleAdd">Add</button>
         </div>
         <div class="error" v-if="data.showError">
          {{ data.error }}
         </div>
      <div class="list_of_names">
        <div class="names" v-for="(name, index) in data.names" :key="name">
          <div class="pills" @click="handleDelete(index)">{{ name }}</div>
        </div>
      </div>
      <div  v-if="isReady">
        <div class="btn" @click="showResults">Check the looser</div>
  
      </div>


    </div>
  
    <div id="results" class="results" v-if="!data.state">
      <div class="result-container">
        <h1>The looser is :</h1>
        <div class="results-value">{{data.results}}</div><br>
        <div>
          <div class="btn" @click="resetApp">Start Over</div><br/>
          <div class="btn-md" @click="getNewResult">Don't like it, get a new name</div>

        </div>
      </div>
    </div>
 
  </main>
</template>
<style lang="scss">
 h1 {
  color: #ea4c89;
  font-weight: bold;
  font-style: italic;
  text-decoration: underline;
  margin-bottom: 30px;
  text-decoration-color: #448ee4;
  font-size: 38px;
  padding: 0 50px;
}
.btn{
 border:1px solid #ea4c89;
 padding:8px 10px;
 border-radius:10px;
 text-align: center;
 width:220px;
 color:#ea4c89;
 cursor: pointer;
 margin: auto
}
.btn-md{
  border:1px solid #448ee4;
  padding:8px 10px;
  border-radius:10px;
  text-align: center;
  width:220px;
  color:#448ee4;
  cursor: pointer;
  margin: auto
 }
.btn:hover{
  background:#ea4c89;
  transition: all 1s;
  color:#fff
   } 
   .btn-md:hover{
    background:#448ee4;
    transition: all 1s;
    color:#fff
     }
.names {
  display: inline-flex;
  margin: 5px;
}
.input-container {
  display: flex;
}
input {
  font-size: 14px;
  border-radius: 6px;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
  line-height: 1.5;
  padding: 8px 10px;
  transition: box-shadow 100ms ease-in, border 100ms ease-in,
    background-color 100ms ease-in;
  border: 2px solid #dee1e2;
  color: rgb(14, 14, 16);
  background: #dee1e2;
  display: block;
  margin-bottom: 20px;
  width: 100%;
  // height: 36px;
  :hover {
    border-color: #ccc;
  }
  :focus {
    border-color: #9147ff;
    background: #fff;
  }
}
.pills {
  background-color: #448ee4;
  border-style: none;
  box-sizing: border-box;
  color: #ffffff;
  display: inline-block;
  font-size: 14px;
  font-weight: 500;
  border-radius: 10px;
  padding: 5px 10px;
  cursor: pointer;
  margin-bottom: 15px
}
button {
  background-color: #6dadf5;
  border-style: none;
  box-sizing: border-box;
  color: #ffffff;
  cursor: pointer;
  display: inline-block;
  font-size: 14px;
  font-weight: 500;
  height: 40px;
  line-height: 20px;
  list-style: none;
  margin: 0;
  outline: none;
  padding: 10px 16px;
  position: relative;
  text-align: center;
  text-decoration: none;
  transition: color 100ms;
  border-top-right-radius: 6px;
  border-bottom-right-radius: 6px;
}
.error{
  color:red;
  text-align: center;
  margin-bottom:10px
}
.results-value{
  text-align: center;
  color:#6dadf5;
  font-size:40px;
  font-weight:bold;
}
</style>
