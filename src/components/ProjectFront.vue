<template>
  <div class="directory">
    <h1>{{ msg }}</h1>
    <button v-on:click="putRequest">Load Data to DB</button>
  </div>
  <Suspense>
    <body>
      <div id="entries">
        <template v-for="employee in getData" :key="employee.employeeId">
          <img :src="employee.employeeImage"/>
          <template v-for="el in Object.entries(employee)" :key="el">
            <li v-if="!['employeeImage','managerFlag','personalInformation'].includes(el[0])">
              {{ startCase(el[0]) }}: {{ el[1] }}
            </li>
            <li v-if="el[0]=='personalInformation'&&el[1]!=null">
              Personal Information:
              <ul v-for="subEl in Object.entries(el[1])" :key="subEl">
                {{ startCase(subEl[0]) }}: {{ subEl[1] }}
              </ul>
            </li>
          </template>
        </template>
      </div>
    </body>
  </Suspense>
</template>

<script>
import axios from 'axios'
//import VueAxios from 'vue-axios'
import _ from "lodash"
export default {
  name: 'ProjectFront',


  data: function () {
    return {
      extensionID: "",
      getAgain: false,
      putData: [],
      getData: [],
      displayData: [],
      employeeList: [],
      managerFlag: "false"
    }
  },
  props: {
    msg: String
  },
  computed:{
    startCase(){
      return _.startCase
    }
  },
  methods:{
    async getRequest(){
      console.log('deleting')
      this.getData.splice(0)
      console.log(this.getData)
      await this.employeeList.forEach(async (employeeID, i) => {
        console.log('Sending Get Request. Manager Flag = '+this.managerFlag)
        console.log(employeeID)
        let result = await axios.get("http://localhost:8080/employees/"+employeeID, {
          params: {
            flag: this.managerFlag
          }
        })
        result.data.employeeImage = result.data.employeeImage.replace("C:\\Users\\Stego\\","http://localhost:8002/")
        console.log(result)
        
        this.getData[i] = result.data
      })
      console.log('check'+this.getAgain)
      if (this.getAgain == true){
        this.getAgain = false
        this.getRequest()
      } else {
        this.getAgain = true
      }
    },
    async putRequest(){
      console.log('put')
    //Code to add employees to DB
      const inputData = require('../assets/inputdata.json')
      Object.values(inputData).forEach(element => this.putData.push(element))
      for (let i = 0; i < this.putData.length; i++){
        let employeePut = this.putData[i]
        const result = await axios.put("http://localhost:8080/employees", employeePut)
        console.log(result)   
      }
    }
  },

  async beforeMount() {
    this.employeeList = require('../assets/employeelist.json')
    this.extensionID = require('../assets/extensionID.json')
    
    var vm = this

    console.log('before'+ vm.getAgain)
    window.addEventListener("message", function(event) {
      if (event.source == window &&
          event.data &&
          event.data.direction == "from-page-script") {
        console.log('got event')
        vm.managerFlag = "true"
        if (vm.getAgain == true){
          vm.getAgain = false
          vm.getRequest()
        } else {
          vm.getAgain = true
        }
      }
    })
    console.log('after')

    this.getRequest()
    
    //Run this to add employees to DB
    //this.putRequest()
  },
}

</script>


<style scoped>
#entries {
  padding: 40px;
  text-align: left;
  color:white;
}
img {
  width: 200px;
}
html {
  background: url('../assets/aaron-burson-wood-background.jpg');
}
</style>
