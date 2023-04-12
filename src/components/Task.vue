<template>
<div class="modal-overlay">
<div class="container" id="mainContainer">
  <form ref ="formRef" @submit.prevent="addTask" @reset.prevent="onCancel" >
   <div class="card" id = "makeCenter">
      <!--Card Header for ADD-->
      <div class="card-header bg-primary container-fluid" v-if="changeClicked">
          <i class="fa fa-circle-plus" id="colorWhite"></i>
          <span id="colorWhite">&nbsp Add Task</span> 
      </div>
      <!--Card Header for EDIT-->
      <div class="card-header bg-primary container-fluid" v-else>
          <i class="fa fa-pen-to-square" id="colorWhite"> 
          <span id="colorWhite">&nbsp Edit Task</span>
      </div>
      <!--End Of Card Header-->
      <!--Begin of row-->
      <div class="row">
        <!--Title input-->
        <div class="col-md-1" v-if="changeClicked"></div>
        <div class="form-group col-md-10" v-if="changeClicked">
          <label class="form-label" for="title">Title</label>
          <input type="text" 
          id ="title" 
          ref="titleRef"
          class="form-control form-outline"
          :class="validateTitle"
          placeholder="Title">
          <div v-if="notDuplicate" class="invalid-feedback">Title is required!</div>
          <div v-else class="invalid-feedback">Title is not unique! Choose another title!</div>
        </div>
        <div class="col-md-1" v-if="changeClicked"></div>
        <!--End of the title input-->
        <!--Description Input-->
        <div class="col-md-1"></div>
          <div class="form-group col-md-10">
            <label class="form-label" for="description">Description</label>
            <!--input for ADD-->
            <input v-if="changeClicked" type="text"
             id ="description"
             ref="descriptionRef" 
             class="form-control form-outline" 
             :class="validateDescription" 
             placeholder="Description">
             <!--input populate earlier stuff in EDIT-->
              <input v-else-if="!editDescription && isNotEmpty" type="text"
             id ="description"
             :value="changeDescription"
             ref="descriptionRef" 
             class="form-control form-outline" 
             :class="validateDescription" 
             placeholder="Description"
             @input="updateDescription">
            <input v-else type="text"
             id ="description"
            :value="editDescription"
             ref="descriptionRef" 
             class="form-control form-outline" 
             :class="validateDescription" 
             placeholder="Description"
             @input="updateDescription">
             <!--End of edit description functionality-->
          <div class="invalid-feedback">Description is required!</div>
        </div>
        <div class="col-md-1"></div>
        <!--End of Description Input-->
        <!--Deadline date picker-->
        <div class="col-md-1"></div>
          <div class="form-group col-md-10">
            <label class="form-label" for="date">Deadline</label>
            <!--Input date for ADD-->
            <input
            v-if="changeClicked" 
            type="date" 
            id ="date"
            ref="dateRef" 
            class="form-control form-outline" 
            :class="validateDate"  
            placeholder="Date">
            <!--Edit the deadline functionality-->
            <input type="date" 
            v-else-if="!editDeadline"
            :value="changeDeadline"
            id ="date"
            ref="dateRef" 
            class="form-control form-outline" 
            :class="validateDate"  
            placeholder="Date"
            @change="updateDate">
            <input type="date" 
            v-else
            :value="editDeadline"
            id ="date"
            ref="dateRef" 
            class="form-control form-outline" 
            :class="validateDate"  
            placeholder="Date"
            @change="updateDate">
            <!--End of edit functionality for deadline-->
            <div class="invalid-feedback">Deadline is required!</div>
        </div>
        <div class="col-md-1"></div>
        <!--end of date picker-->
        <!--Begin of priority-->
        <!--Label priority-->
        <div class="col-md-1"></div>
        <div class="col-md-10">
          <label>Priority:</label>
        </div>
        <div class="col-md-1"></div>
        <!--End Label Priority-->
        <!--Start priority buttons-->
        <div class="col-md-1"></div>
        <div class="col-md-10" v-if="changeClicked">
            <!--radios for ADD task-->
              <label class="radio-inline col-md-4 form-check-label" :class="validatePriority" for="lowButton">
                <input type="radio" class="form-check-input" :class="validatePriority" ref="lowRef" name="bottom" value="low" id="lowButton" @click="changeLowPriority" v-model="radioNumber" >&nbsp Low</input>
              </label>
              <label class="radio-inline col-md-4 form-check-label" :class="validatePriority" for="medButton">
                <input  type="radio" class="form-check-input" :class="validatePriority" ref="medRef" name="bottom" value="med" id="medButton" @click="changeMedPriority" v-model="radioNumber">&nbsp Med</input>
              </label>
              <label class="radio-inline col-md-4 form-check-label" :class="validatePriority" for="highButton">
                <input type="radio" class="form-check-input" :class="validatePriority" ref="highRef" name="bottom" value="high" id="highButton" @click="changeHighPriority" v-model="radioNumber">&nbsp High</input>
              </label>
              <div class="invalid-feedback">Choose a Priority!</div>
            <!--end of radios for ADD task-->
        </div>
        <div class="col-md-10" v-else>
            <!--start of radios for EDIT task-->
              <label class="radio-inline col-md-4 form-check-label" :class="validatePriority" for="lowButton">
                <span v-if="changePriority=='low' && !editPriority" v-html="lowCheck"></span>
                <input v-else type="radio" class="form-check-input" :class="validatePriority" ref="lowRef" name="bottom" id="lowButton" @click="changeLowPriority" >&nbsp Low</input>
              </label>
              <label class="radio-inline col-md-4 form-check-label" :class="validatePriority" for="medButton">
                <span v-if="changePriority=='med' && !editPriority" v-html="medCheck"></span>
                <input v-else type="radio" class="form-check-input" :class="validatePriority" ref="medRef" name="bottom" id="medButton" @click="changeMedPriority">&nbsp Med</input>
              </label>
              <label class="radio-inline col-md-4 form-check-label" :class="validatePriority" for="highButton">
                 <span v-if="changePriority=='high' && !editPriority" v-html="highCheck"></span>
                <input v-else type="radio" class="form-check-input" :class="validatePriority" ref="highRef" name="bottom" id="highButton" @click="changeHighPriority">&nbsp High</input>
              </label>
              <div class="invalid-feedback">Choose a Priority!</div>
            <!--end of radios for EDIT task-->
        </div>
        <div class="col-md-1"></div>
        <!--End priority buttons-->
        <!--start of the buttons at the end of the dialog-->
        <div class="col-md-3"></div>
        <div class="col-md-9">
           <button type="submit" class="btn btn-primary" id ="addRow" v-if="changeClicked"> 
              <i class="fa fa-circle-plus"></i> Add
            </button>
            <button type="submit" class="btn btn-primary" id ="addRow" v-else> 
               <i class="fa fa-pen-to-square" id="colorWhite"></i> Edit
            </button>
            <button type="reset" class="btn btn-danger close" id ="cancel"> 
              <i class="fa fa-ban"></i> Cancel
            </button>
        </div>
        <!--end of the buttons at the end of the dialog-->
      </div>
    <!--End of row-->
    </div>
    <!--end of card-->
  </form>
  <!--end of form-->
  </div>
  <!--end of container-->
  </div>
  <!--modal overlay-->
</template>

<script>
import Page from './Page.vue';
export default {
  name: 'Task',
  components:{
  },
  props: {
    changeClicked: String,
    tableRows: Array,
    changeDescription: String,
    changeDeadline: Date,
    changePriority: String,
  },
  data(){
    return{
      validateTitle:'',
      validateDescription:'',
      validateDate:'',
      validatePriority: '',
      editDescription: this.changeDescription,
      editPriority: '',
      editDeadline: '',
      variablePriority:'',
      radioNumber:'',
      showAdd: false,
      notDuplicate: true,
      lowCheck: ' <input type="radio" class="form-check-input" :class="validatePriority" ref="lowRef" name="bottom" id="lowButton" @click="changeLowPriority" checked></input>',
      medCheck: ' <input type="radio" class="form-check-input" :class="validatePriority" ref="medRef" name="bottom" id="medButton" @click="changeMedPriority" checked></input>',
      highCheck: ' <input type="radio" class="form-check-input" :class="validatePriority" ref="highRef" name="bottom" id="highButton" @click="changeHighPriority" checked></input>',
      vmDescription: '',
      isNotEmpty:true,
    };
  },
  
  methods:{
    //validates task to the frameworks table
    addTask(){
      let errorFound = false;
      //checking title
      if(this.changeClicked && !this.$refs.titleRef.value){
          this.notDuplicate = true;
          this.validateTitle = 'is-invalid';
          errorFound = true;
      }
      else if(this.changeClicked){
        let findDuplicateTitle = this.tableRows.find(row => row.title == this.$refs.titleRef.value);
         if(findDuplicateTitle){
            this.notDuplicate = false;
            this.validateTitle = 'is-invalid';
            errorFound = true;
         }
         else{
           this.validateTitle = '';
         }
        
      }
      // description validation
      if(!this.$refs.descriptionRef.value){
        this.validateDescription = 'is-invalid';
        errorFound = true;
      }
      else{
        this.validateDescription = '';
      }
      if(!this.$refs.dateRef.value){
        this.validateDate = 'is-invalid';
        errorFound = true;
      }
      else{
        this.validateDate = '';
      }
      //priority validation
      if(!this.variablePriority){
        
        if(!this.editPriority && !this.variablePriority && !this.changeClicked){
          this.variablePriority = this.changePriority;
        }
        else if(!this.editPriority && !this.changeClicked){
         
          this.editPriority = this.changePriority;
          this.variablePriority = this.changePriority;
        }
        else{
          this.validatePriority = 'is-invalid';
          errorFound = true;
        }
        
      }
      else{
      
        this.validatePriority = '';
      }
      //send data and reset form for ADD task
      if(this.changeClicked && !errorFound){
        let dateFormat = moment( this.$refs.dateRef.value).format('MM/DD/YY');
        this.$emit('infoSignal', this.$refs.titleRef.value, this.$refs.descriptionRef.value, dateFormat, this.variablePriority );
        this.onCancel();
      }
      //send data for EDIT task
      else if(!errorFound){
        let dateFormat2 = moment( this.$refs.dateRef.value).format('MM/DD/YY');
        this.$emit('updateSignal', this.$refs.descriptionRef.value, dateFormat2, this.variablePriority );
        this.onCancel();
      }
    
    },
    //cancelling task
    onCancel(){
      this.$emit('close-modal');
      if(this.changeClicked){
        this.validateTitle='';
        this.$refs.titleRef.value = '';
      }
      this.$refs.formRef.reset();
      this.$refs.descriptionRef.value = '';
      this.$refs.dateRef.value = '';
      this.validateDescription='';
      this.validateDate='';
      this.validatePriority='';
      this.variablePriority='';
      this.radioNumber="";
      this.editPriority="";
      this.editDescription ="";
      this.editDeadline = "";
      this.isNotEmpty = true;
    },
    //button value changes set to low
    changeLowPriority(){
      this.editPriority ='low';
      this.variablePriority = "low";
    },
    //change button value to med
    changeMedPriority(){
      this.editPriority = 'med';
      this.variablePriority = "med";
    },
    //change button value to high
    changeHighPriority(){
      this.editPriority = 'high';
      this.variablePriority = "high";
    },
    // update the description for EDIT task
    updateDescription(){
       this.editDescription = this.$refs.descriptionRef.value;
       if(!this.$refs.descriptionRef.value){
         this.isNotEmpty = false;
         this.editDescription = '';
       }
    },
    //update the date for the EDIT task
    updateDate(){
      this.editDeadline = this.$refs.dateRef.value;
    }
  

  },
}
</script>

<style scope>
/*background for the dialog*/
.modal-overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #000000da;
}
#mainContainer{
  width:29%;
  height:40%;
}
/*changing the size of the dialog*/
#dialogSize{
  width:29%;
  height:40%;
}
/*for any text/icon that needs to be the color white*/
#colorWhite{
  color:white;
}
/*changing size of button. cancel button*/
#cancel{
  width:45%;
}
/*changing the size of the add button*/
#addRow{
  width:42%;
}
</style>