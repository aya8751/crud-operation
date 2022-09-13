<template>
  <div class="">
    <button
      type="button"
      class="btn btn-primary m-2 float-start"
      data-bs-toggle="modal" 
      data-bs-target="#exampleModal"
      @click="handleAdd()"
    >Add User</button>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Website</th>
          <th>Option</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.website }}</td>
          <td>
            <button
              type="button"
              class="btn btn-light me-2"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
              @click="handleEdit(user)"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-pencil-square"
                viewBox="0 0 16 16"
              >
                <path
                  d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"
                />
                <path
                  fill-rule="evenodd"
                  d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z"
                />
              </svg>
            </button>
            <button type="button" class="btn btn-light" @click="deleteClick(user.id)">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="16"
                height="16"
                fill="currentColor"
                class="bi bi-trash-fill"
                viewBox="0 0 16 16"
              >
                <path
                  d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z"
                />
              </svg>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- Modal -->
    <div
      id="exampleModal" 
      tabindex="-1" 
      aria-labelledby="exampleModalLabel" 
      aria-hidden="true"
      class="modal fade"
    >
      <div class="modal-dialog modal-lg modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">{{ modalTitle }}</h5>
            <button
              type="button"
              class="btn-close" 
              data-bs-dismiss="modal"
              aria-label="Close" 
            ></button>
          </div>
          <div class="modal-body">
            <div class="input-group mb-3">
              <span class="input-group-text">User Name</span>
              <input type="text" class="form-control" v-model="name" />
            </div>
            <div class="input-group mb-3">
              <span class="input-group-text">User Email</span>
              <input type="text" class="form-control" v-model="email" />
            </div>
            <div class="input-group mb-3">
              <span class="input-group-text">User Website</span>
              <input type="text" class="form-control" v-model="website" />
            </div>
            <button type="button" v-if="id == 0" class="btn btn-primary" @click="createClick()">
              Create
            </button>
            <button type="button" v-if="id != 0" class="btn btn-primary" @click="updateClick(id)">
              Update
            </button>
          </div> 
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import variables from '../variables';

export default {
  data() {
    return {
      users: [],
      modalTitle: "",
      name: "",
      email: "",
      website: "",
      id: 0
    };
  },
  mounted(){
    this.refersData();
  },
  methods: {
    refersData(){
        axios.get(variables.API_URL+"users").then((res)=>{
            this.users=res.data
            console.log(res.data)
        });
    },
    handleAdd() {
      this.modalTitle = "Add User";
      this.id = 0;
      this.name = "";
      this.email = "";
      this.website = "";
    },
    handleEdit(user) {
      this.modalTitle = "Edit User";
      this.id= user.id;
      this.name = user.name;
      this.email = user.email;
      this.website = user.website;
    },
    createClick(){
        axios.post(variables.API_URL+"users" ,{
            name:this.name,
            email:this.email,
            website:this.website
        }).then((res)=>{
            this.refersData();
            alert("Creating Done");
            // console.log(res)
        }).catch((error)=>{console.log(error)});
    },
    updateClick(id){
        axios.put(variables.API_URL+`users/${id}`, {
            name:this.name,
            email:this.email,
            website:this.website
        }).then((res)=>{
            this.refersData();
            alert("Updating Done");
            // console.log(res.data);
        }).catch((error)=>{console.log(error)})
    },
    deleteClick(id){
      if(!confirm("Are you sure?")){
        return;
      }
      axios.delete(variables.API_URL+'users/'+id)
      .then((res)=>{
        this.refersData();
        alert("Deleting Done");
      }).catch((error)=>{console.log(error)})
    }

  },
};
</script>

<style></style>
