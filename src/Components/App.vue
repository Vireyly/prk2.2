<template>
   <div>
      <table>
                <tr>
                    <th>Photo</th>
                    <th>Name</th>
                    <th>Group</th>
                    <th>Mark</th>
                    <th>PR is Done</th>
                </tr>

                 <tr>
                <tr v-for="stud in students" v-bind:key="stud._id" >
                    <td><img v-bind:src="stud.photo"  width="50px"></td>
                    <td >
                        <div v-bind:style="idTest == stud._id ? 'display:none' : 'display:inline'"><router-link v-bind:to="'/student-info/'+stud._id">{{stud.name}}</router-link></div>
                        <input v-bind:style="idTest == stud._id ? 'display:inline' : 'display:none'"  v-model="newStudent.name">
                    </td>
                    <td>
                        <div v-bind:style="idTest == stud._id ? 'display:none' : 'display:inline'">{{stud.group}}</div>
                        <select input v-bind:style="idTest == stud._id ? 'display:inline' : 'display:none'" name="group" v-model="newStudent.group" >
                        <option value="RPZ 18 1/9">RPZ 18 1/9</option>
                        <option value="RPZ 18 2/9">RPZ 18 2/9</option>
                        </select>
                    </td>
                    <td>
                        <div v-bind:style="idTest == stud._id ? 'display:none' : 'display:inline'">{{stud.mark}}</div>
                        <input v-bind:style="idTest == stud._id ? 'display:inline' : 'display:none'" v-model="newStudent.mark">
                    </td>
                    <td>
                        
                        <div   type="checkbox" v-bind:style="idTest == stud._id ? 'display:none' : 'display:inline'" > <input type="checkbox" v-bind:checked="stud.isDonePr"></div>
                        <input type="checkbox" v-bind:style="idTest == stud._id ? 'display:inline' : 'display:none'" v-model="newStudent.isDonePr">
                    </td>
                    <td>
                        <a href="#" v-on:click="deletes(stud._id)">Видалити</a>
                    </td>
                    <td>
                        <button v-on:click="get(stud._id,stud.name,stud.group,stud.isDonePr,stud.mark)">
                            <img src="/1.png" width="25px">
                        </button>
                    </td>
                    <button v-on:click="update()" v-bind:style="showInput ? 'display:inline' : 'display:none'">edit</button>
                </tr>
            </table>



            <br> <h2>Add Student : </h2>
            <br> Фото  <select v-model="photo">
                 <option  value="https://robohash.org/rerecbrb" width="50px">1 </option>
              <option value="https://robohash.org/recm0cfljvfpf"  width="50px">2</option>
                 <option value="https://robohash.org/yzvfgr"  width="50px">3</option>
                    <option value="https://robohash.org/ojhgfrt2578jg"  width="50px">4</option>
                       <option value="https://robohash.org/cat"  width="50px">5</option>
              </select>
            <br> Name  <input v-model="name">
            <br> Group <select v-model="group">
                <option value="RPZ 18 1/9">RPZ 18 1/9</option>
              <option value="RPZ 18 2/9">RPZ 18 2/9</option>
            </select>
            <br> Mark <input type="number"  v-model="mark">
            <br> PR <input type="checkbox" v-model="isDonePr">
            <br> <button v-on:click="add">Add</button>
   </div>
</template>

<script>
    import VueRouter from 'vue-router'
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'


    export default {
        props: {
           id: '',
       },

    data: function() {
        return {
        students: [],
        currency:[],
        newStudent: {},
        mark1: "",
        name1:"",
        group1:"",
        isDonePr1:false,
        showInput:false,
        photo:"",
        name:"",
        group:"",
        studId:"",
        isDonePr:false,
        search:"",
        mark: 0,
        start_ccy:"",
        end_ccy:"",
        sell:0,
        buy:0,
        start_value:0,
        end_value:0,
        result:"",
        idTest:"",
        studId:"",
    }},
    mounted: function(){
        axios.get("http://46.101.212.195:3000/students").then((response)=>{
            console.log(response.data);
            this.students = response.data;
        })
        axios.get("https://api.privatbank.ua/p24api/pubinfo?exchange&json&coursid=4").then((response)=>{
            console.log(response.data);
            this.currency = response.data;
        })
        
    
    },
    methods:{
       deletes:function(id){
            Vue.axios.delete("http://46.101.212.195:3000/students/"+id, {
            }).then((response)=>{
                axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
            })

            })
            
           
        },
         get: function(id,name,group,isDone,mark){
             this.newStudent.id = id;
            this.newStudent.name = name;
            this.newStudent.group = group;
            this.newStudent.isDonePr = isDone;
            this.newStudent.mark = mark;
            this.studId = id;
            this.idTest = id;
            this.showInput = true;
        },
        update:function(){
            Vue.axios.put("http://46.101.212.195:3000/students/"+this.studId, {
                photo:this.newStudent.photo,
                name: this.newStudent.name,
                group: this.newStudent.group,
                isDonePr: this.newStudent.isDonePr,
                mark:this.newStudent.mark,
               
            })
            axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
            })
            this.idTest = "";
        },
        
    add:function(){ 
            Vue.axios.post("http://46.101.212.195:3000/students", { 
                photo: this.photo,
                name: this.name, 
                group: this.group, 
                isDonePr: this.isDonePr, 
                mark:this.mark,
            }) 
            .then((response) => { 
                axios.get("http://46.101.212.195:3000/students").then((response)=>{ 
                this.students = response.data; 
            })
                console.log(response.data) 
            }) 
            
             
        },

        

       deleteRow:function(id){
            this.students =  this.students.filter(stud => stud.id!=id)
       },
       convert:function(){
           for(let i=0; i<this.currency.length; i++){
               if (this.currency[i].ccy==this.start_ccy)
                     this.sell=this.currency[i].sale;
               if (this.currency[i].ccy==this.end_ccy)
                     this.buy=this.currency[i].buy;
           }
           this.end_value=(this.start_value*this.sell)/this.buy;
           this.result = this.start_value + " " + this.start_ccy + "  ===  " ;
           
       }
    },
    filters:{
            roundValue: function(value){
                return parseFloat(value.toFixed(2));
            },
        }

}


</script>