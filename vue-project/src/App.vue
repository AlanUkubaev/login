<template>
<!-- Login -->
  <div>
    <div class="InputsOfUsers">
    <input type="text" placeholder="name" v-model="valName">
    <input type="text" placeholder="password" v-model="valPassword">
    <input class="valAgeInput" type="number" placeholder="age" v-model="valAge">
    <button class="SubmitToUsers" @click="addNameAndPassword">Submit</button>
    </div>
    <h1 v-if="userName" class="requires">Name is required</h1>
    <h1 v-if="userPassword" class="requires">Password is required</h1>
    <h1 v-if="userAge" class="requires">Age is required</h1>
    <div v-if="!isUsersCheckbox">
      <button>Activate</button>
      <button>Delete</button>
    </div>
    
  <div>

<!-- Table of Users (Name; Password; Age; Date; ID:) -->
<table class="tableUsers">
  <tr>
    <th></th>
    <th>Date</th>
    <th>Name</th>
    <th>Password</th>
    <th>Age</th>
    <!-- <th>ID</th> -->
    <th>Operations</th>
  </tr>
  <tr v-for="user of users">
    <td><input class="checkbox-tableUsers" type="checkbox" v-model="user.checkbox"></td>
    <td>{{ user.date }}</td>
    <td>{{ user.name }}</td>
    <td>{{ user.password }}</td>
    <td>{{ user.checkbox }}</td>
    <!-- <td>{{ user.id }}</td> -->
    <td>
      <div class="buttonsInTable">
        <button class="editUser" @click="editUser(user)">Edit</button>
        <button class="deleteFromUsers" @click="openModalDeleteUser(user)">Delete</button> 
      </div>
  </td>
  </tr>
  <h1 v-show="noDataDisappear">*NO DATA*</h1>
</table>
</div>
</div>



<!-- Modal Window with Inputs -->

<!-- Edit -->
<div v-if="appeared" >
  <div class="modal">
    <div class="modal-content">
      <h3 class="h3-modalEdit">EDIT</h3>
      <h3 v-if="userEditName" class="requires">Name is required</h3>
      <h3 v-if="userEditPassword" class="requires">Password is required</h3>
      <h3 v-if="userEditAge" class="requires">Age is required</h3>
    <button @click="closeEditUser" class="close">&times;</button>
    <input class="input-modalEdit" type="text" placeholder="name" v-model="valEdit.name">
    <input class="input-modalEdit" type="text" placeholder="password" v-model="valEdit.password">
    <input class="input-modalEdit" type="text" placeholder="age" v-model="valEdit.age">
    <button @click="submitEditUser()" class="SubmitToUsersThroughEdit">Submit</button>
  </div>
  </div> 
</div>

<!-- Delete -->
<div v-if="deleteAppeared" >
  <div class="modal">
    <div class="modal-content">
      <div class="closetext-modal-content">
        <h1>Are you sure you want to delete it?</h1>
        <button  @click="closeDeleteUser" class="deleteClose">&times;</button>
      </div>
    <button @click="deleteUser" class="SubmitToUsers">Yes</button>
  </div>
  </div> 
</div>
</template>

<!-- SCRIPT -->
<script>
export default {
  data(){
    return{
      users: [],
      valName: '',
      valPassword: '',
      valAge: '',
      appeared: false,
      deleteAppeared: false,
      valEdit: [{name: '', password: '', age: '', id: ''}],
      deleteUserName: '',
      userName: false,
      userPassword: false,
      userAge: false,
      nothing: '',
      userEditName: false,
      userEditPassword: false,
      userEditAge: false,
      counterpartOfusers: [],
      checkboxTableUsersValue: false,
      
    }
    },

methods: {
// Submit => Array => Table
    addNameAndPassword(){
    this.valAge == Number(this.valAge)
    if(this.valAge >= 18){
      if(this.valName == this.nothing || this.valPassword == this.nothing){
      if(this.valName == this.nothing){
        this.userName = true
      }else{
        this.userName = false
      }
      if(this.valPassword == this.nothing){
        this.userPassword = true
      }else{
        this.userPassword = false
      }
    }else{
      this.users.push({name:this.valName, password:this.valPassword, age:this.valAge, date: new Date,id:this.users.length + 1, checkbox:false})
      this.counterpartOfusers.push({id:this.users.length + 1})
      this.valName = ''
      this.valPassword = ''
      this.valAge = ''
      this.userName = false
      this.userPassword = false
    }
    
   }else{
    alert('You have not enough age')
   }
    },

    //  Delete
    openModalDeleteUser(user){
    this.deleteAppeared = true
    this.delteUserName = user.name
   
    
    },
    deleteUser(){
      let deleted = this.users.findIndex((ell) => ell.name === this.deleteUserName)
      this.users.splice(deleted, 1)
      this.deleteAppeared = false
    },

// click EDIT => Inputs in Modal Window Edit
    editUser(user){
      this.appeared = true
      this.valEdit.name = user.name
      this.valEdit.password = user.password
      this.valEdit.age = user.age
      this.valEdit.id = user.id
    },

// FIND ARRAY => BACK with CLOSE
    submitEditUser(){
      let finding = this.users.find((el) => el.id == this.valEdit.id)
      if(this.valEdit.age > 17){
        if(this.valEdit.name == this.nothing || this.valEdit.password == this.nothing){
          if(this.valEdit.name == this.nothing){
            this.userEditName = true
          }else{
            this.userEditName = false
          }
          if(this.valEdit.password == this.nothing){
            this.userEditPassword = true
          }else{
            this.userEditPassword = false
          }
        }else{
          finding.name = this.valEdit.name
          finding.password = this.valEdit.password
          finding.age = this.valEdit.age
          this.appeared = false
        }
      }else{
        alert('You have not enough age')        
      }
    },
      closeEditUser(){
      this.appeared = false
    },
    closeDeleteUser(){
      this.deleteAppeared = false
    }
    
   },

  computed:{
    disableBtn(){
      return this.valmillioner
    },
    noDataDisappear(){
      if(this.users.length > 0){
        return false
      }else{
        return true
      }
    },
    isUsersCheckbox(){
    return !this.users.find((ell)=>ell.checkbox === true)
    }
  }
  }
</script>


<style>
input {
  padding-left: 6px;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
  margin-top: 40px;
}

td, th {
  border-bottom: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}
tr:hover {
  background-color: #f5f7fa;
}

.checkbox-tableUsers{
  width: 18px;
  height: 18px;
}
.checkbox-tableUsers:hover{
  cursor: pointer;
}

.requires{
  color: rgb(255, 48, 48);
}
.input-modalEdit{
height: 40px;
width: 400px;
margin-bottom: 20px;
}

.InputsOfUsers{
  display: flex;
  gap: 10px;
}
.h3-modalEdit{
  text-align: center;
}
.valAgeInput{
  width: 50px;
}
.SubmitToUsersThroughEdit{
  border: 0px;
border-radius: 5px;
width: 400px;
height: 40px;
background: rgb(82, 158, 82);
color: white;
cursor: pointer;
}
.SubmitToUsers{
border: 0px;
border-radius: 5px;
width: 100px;
height: 30px;
background: rgb(82, 158, 82);
color: white;
cursor: pointer;
}
.SubmitToUsers:hover{
  background: rgb(89, 173, 89);
}
.deleteFromUsers{
border: 0px;
border-radius: 5px;
width: 60px;
height: 30px;
background: #f56c6c;
color: white;
cursor: pointer;
}
.deleteFromUsers:hover{
border: 0px;
border-radius: 5px;
width: 60px;
height: 30px;
background: #f89898;
color: white;
transition: .1s;
}
.editUser{
border: 1px solid #dcdfe6;
border-radius: 5px;
width: 60px;
height: 30px;
background: white;
color: #606266;
cursor: pointer;
}
.editUser:hover{
border: 1px solid #409eff;
background: #f5f7fa;
color: #409eff;
transition: .1s;
}
.buttonsInTable{
  display: flex;
  gap: 5px;
}

.modal {
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content */
.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  height: 550px;
  width: 450px;
  border-radius: 10px;
}

/* The Close Button */
.close {
  color: #aaaaaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  border: 0px;
  background: transparent;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}

.closetext-modal-content{
  display: flex;
  gap: 100px;
}
.deleteClose{
  font-size: 28px;
  font-weight: bold;
  background: transparent;
  border: 0px;
}
.deleteClose:hover{
  color: #464646;
  cursor: pointer;
}
</style>

