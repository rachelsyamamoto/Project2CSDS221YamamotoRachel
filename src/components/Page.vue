<template>
  <div class="container-fluid" id = "mainTable">
        <!--Start of card-->
        <div class="card">
          <div class="card-header bg-primary" id ="headerMainTable">
            <!--start of row in the header-->
            <div class="row">
                <div class="col-md-5"></div>
                <!--Frameworks + the three bar icon-->
                <div class="col-md-5" id="frameworkCentered">
                  <i class="fa fa-bars" id="makeWhite">&nbsp Frameworks&nbsp </i> 
                </div>
                <!--end of the framework div-->
                <!--Add button-->
                <div class="col-md-2" id="addButtonHugEnd">
                  <button type="button" class="btn btn-primary" v-on:click="addButtonClicked" id ="addButton"> 
                      <i class="fa fa-circle-plus"></i> Add
                  </button>
                </div>
                <!--End of Add Button-->
            </div>
            <!--End of Row-->
          </div>
          <!--End of card header-->
          <!--Add Task-->
          <Task v-show="taskShow" @close-modal="taskShow=false" :changeClicked="changeClicked" :tableRows="tableRows"
          :changeDescription="changeDescription"
          :changeDeadline="changeDeadline"
          :changePriority="changePriority"
          @infoSignal="addEntry" @updateSignal="updateEntry"/>
          <!--End Task-->
          <!--Start of table-->
          <table class="table">
            <thead class="thead"> 
            </thead>
            <!--start of table body-->
            <tbody>
              <!--table header information-->
              <tr>
                <td class="col-md-2">Title</td>
                <td class="col-md-2">Description</td>
                <td class="col-md-2 hello">Deadline</td>
                <td class="col-md-2">Priority</td>
                <td class="col-md-2">Is Complete</td>
                <td col="col-md-2">Action</td>
              </tr>
              <!--end of table header information-->
              <!--rest of table-->
              <tr v-for="(item, index) in tableRows">
                <td class="col-md-2">{{item.title}}</td>
                <td class="col-md-2">{{item.description}}</td>
                <td class="col-md-2">{{item.deadline}}</td>
                <td class="col-md-2">{{item.priority}}</td>
                <td class="col-md-2">
                  <span v-if="item.isNotComplete">
                       <input type="checkbox" id="checkboz" value="item.isNot" @change="toggle(index)">
                       </input>
                  </span>
                  <span v-else>
                       <input type="checkbox" id="checkboz" value="item.isNot" @change="toggle(index)" checked></input>
                  </span>     
                </td>
                <td class="col-md-2">
                <!--start of container-->
                  <div v-if="item.isNotComplete" >
                    <button type="button" class="btn btn-primary" @click="updateRow(index, item.description, item.deadline, item.priority)"><i class="fa fa-pen-to-square" id="makeWhite"> UPDATE</button>
                     <!--Task v-show="taskShow" @close-modal="taskShow=false"/-->
                  </div>
                  <button class="btn btn-danger" @click="deleteRow(index)"><i class="fa fa-circle-xmark" id="makeWhite">DELETE</button>  
                  <!--end of container-->
                </td>
              </tr>
            </tbody>
            <!--End of table body-->
          </table>
          <!--End of table-->
        </div>     
        <!--End of Card-->
  </div>
  <!--end container fluid-->
</template>

<script>
import Task from './Task.vue';
import toastr from 'toastr';
import fontawesome from 'font-awesome';
import 'toastr/build/toastr.css';
export default {
  name: 'Page',
  components: {
    Task
  },
  data(){
    return{
      taskShow:false,
      tableRows:[],
      changeClicked: '',
      updateIndex: 0,
      showUpdateButton: true,
      changeDescription: '',
      changeDeadline:'',
      changePriority:''
    }
  },
  methods:{
    //toggles the complete and not complete functionality of each row
    toggle(index){
      let toggleEntry = this.tableRows[index];
      if(toggleEntry.isNotComplete == true){
        toggleEntry.isNotComplete = false;
      }
      else{
        toggleEntry.isNotComplete = true;
      }
      this.tableRows.splice(index, 1, toggleEntry);
    },
    //triggered when update button is pressed
    updateEntry(desc, deadline, priority){
      let updateEntry = this.tableRows[this.updateIndex];
      updateEntry.description = desc;
      updateEntry.deadline = deadline;
      updateEntry.priority = priority;
      this.tableRows.splice(this.updateIndex, 1, updateEntry);
      toastr["success"]("Successfully Updated Task!!!", "", {
      "positionClass": "toast-bottom-right",
      "preventDuplicates": false,
      "showDuration": "300",
      "showEasing": "swing",
      "showMethod": "fadeIn"});
    },
    //add task functionality
    addEntry(title, desc, deadline, priority){
     var tableEntry = {
        title: title,
        description: desc,
        deadline: deadline,
        priority: priority,
        isNotComplete:true
     };
     this.tableRows.push(tableEntry);
      toastr["success"]("Successfully Added Task!!!", "", {
      "positionClass": "toast-bottom-right",
      "preventDuplicates": false,
      "showDuration": "300",
      "showEasing": "swing",
      "showMethod": "fadeIn"});
    },
    //triggered when the delete task button is pressed
    deleteRow(index){
      this.tableRows.splice(index, 1);
      toastr["success"]("Successfully Deleted Task!!!", "", {
      "positionClass": "toast-bottom-right",
      "preventDuplicates": false,
      "showDuration": "300",
      "showEasing": "swing",
      "showMethod": "fadeIn"});
    },
    //triggered when the add task button is pressed
    addButtonClicked(){
      this.taskShow = true;
      this.changeClicked = true;
    },
    //code used to update a row in the table
    updateRow(index, desc, deadline, priority){
      this.taskShow = true;
      this.changeClicked = false;
      this.updateIndex = index;
      this.changeDescription = desc;
      this.changeDeadline = moment(deadline).format("YYYY-MM-DD");
      this.changePriority = priority;
    },
  }
}
</script>

<style>
  /*formatting of the main table*/
  #mainTable{
    display:flex;
    flex-direction:column;
    width:100vw;
    height:100vh;
  }
  /*add button coloring and shadow*/
  #addButton{
    color:white;
    box-shadow: 1px 1px 2px 2px #000000;
  }
  /*used for making icons a certain color*/
  #makeWhite{
    color:white;
  }
  /*used for making the Framework word a certain color*/
  #changeColor{
    color:white;
    font-size:1.3em;
  }
  /*table entry formating*/
  td{
    text-align:center;
  }
  /*formats the add button*/
  #addButtonHugEnd{
    display:flex;
    justify-content:flex-end;
  }
  /*used to center items*/
  #frameworkCentered{
    display:flex;
    align-items:center;
  }

</style>