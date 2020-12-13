<template>
  <div>
    <!-- <h3 class="text-gray-700 text-3xl font-semibold">Posts</h3> -->
    <button
      @click="open = true,modal_title = 'Add New Post'"
      class="mt-3 px-6 py-3 bg-indigo-600 rounded-md text-white font-medium tracking-wide hover:bg-indigo-500"
    >
      Add Posts
    </button>

    <hr />
    <div class="mt-5">
      <!-- <h4 class="text-gray-700">Horizontal</h4> -->
      <div :key="index" v-for="(post,index) in posts" class="max-w-sm w-full lg:max-w-full lg:flex mt-6">
        <div
          class="h-48 lg:h-auto lg:w-48 flex-none bg-cover rounded-t lg:rounded-t-none lg:rounded-l text-center overflow-hidden"
          title="Woman holding a mug">
        <img style="object-fit: cover;min-height: 150px;min-width: 150px" :src="'https://blog-node-server-1.herokuapp.com/uploads/'+post.featured_image" alt="">
        </div>
        <div
          class="border-r border-b border-l border-gray-400 lg:border-l-0 lg:border-t lg:border-gray-400 bg-white rounded-b lg:rounded-b-none lg:rounded-r p-4 flex flex-col justify-between leading-normal"
        >
          <div style="margin-bottom:0" class="mb-8">
            <p  class="text-sm text-gray-600 flex items-center">
              <svg
                class="fill-current text-gray-500 w-3 h-3 mr-2"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
              >
                <path
                  d="M4 8V6a6 6 0 1 1 12 0v2h1a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2v-8c0-1.1.9-2 2-2h1zm5 6.73V17h2v-2.27a2 2 0 1 0-2 0zM7 6v2h6V6a3 3 0 0 0-6 0z"
                />
              </svg>
              Admin only
            </p>
            <div class="flex" style="float:right">
              <a @click="editPost(post._id),modal_title = 'Edit Post'" href="javascript:">
                <svg  style="width:21px;color:brown" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
                </svg>
              </a>&nbsp;&nbsp;
              <a @click="deletePost(post._id)" href="javascript:">
                <svg style="width:21px;color:red" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
              </svg>
              </a>
            </div>
            <div v-text="post.title" class="text-gray-900 font-bold text-xl mb-2">
              
            </div>
            <p v-text="post.description" class="text-gray-700 text-base"></p>
          
          </div>
          <div class="flex items-center">
            
          </div>
        </div>
      </div>
    </div>
    <div
      :class="`modal ${
        !open && 'opacity-0 pointer-events-none'
      } z-50 fixed w-full h-full top-0 left-0 flex items-center justify-center`"
    >
      <div
        class="modal-overlay absolute w-full h-full bg-gray-900 opacity-50"
      ></div>

      <div
        class="modal-container bg-white w-11/12 md:max-w-md mx-auto rounded shadow-lg z-50 overflow-y-auto"
      >
        <div
          class="modal-close absolute top-0 right-0 cursor-pointer flex flex-col items-center mt-4 mr-4 text-white text-sm z-50"
        >
          <svg
            class="fill-current text-white"
            xmlns="http://www.w3.org/2000/svg"
            width="18"
            height="18"
            viewBox="0 0 18 18"
          >
            <path
              d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
            />
          </svg>
          <span class="text-sm">(Esc)</span>
        </div>

        <!-- Add margin if you want to see some of the overlay behind the modal-->
        <div class="modal-content py-4 text-left px-6">
          <!--Title-->
          <div class="flex justify-between items-center pb-3">
            <p v-text="modal_title" class="text-2xl font-bold"></p>
            <div class="modal-close cursor-pointer z-50" @click="closeReset">
              <svg
                class="fill-current text-black"
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 18 18"
              >
                <path
                  d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
                />
              </svg>
            </div>
          </div>

          <!--Body-->
          <form @submit.prevent="submitForm">
            <div class="grid grid-cols-1 sm:grid-cols-1 gap-6 mt-4">
              <div>
                <label class="text-gray-700" for="username">Title</label>
                <input v-model="title"
                  class="form-input w-full mt-2 rounded-md focus:border-indigo-600"
                  type="text"
                />
              </div>

              <div>
                <label class="text-gray-700" for="emailAddress"
                  >Description</label
                >
                <textarea v-model="description"
                  class="form-input w-full mt-2 rounded-md focus:border-indigo-600"
                ></textarea>
              </div>

              <div>
                <label class="text-gray-700">Feature Image</label>
                <label for="file-upload" class="custom-file-upload">
                  <i class="fa fa-cloud-upload"></i> <span style="font-size:13px">{{this.img_text}}</span>
                </label>
                <input style="display:none" id="file-upload" class="form-input w-full mt-2 rounded-md focus:border-indigo-600"
                  type="file" v-on:change="handleFileUpload"
                />
                <input type="hidden" v-model="id"/>
              </div>
            </div><br>
            <button type="submit" class="px-6 py-3 bg-indigo-600 rounded-md text-white font-medium tracking-wide hover:bg-indigo-500" style="float:right">
              Submit
            </button><br><br>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<style lang="css">
.custom-file-upload {
  border: 1px solid #ccc;
  display: block;
  padding: 6px 12px;
  margin-top: 7px;
  cursor: pointer; 
}
</style>
<script lang="ts">
import { defineComponent, ref } from 'vue';
import axios from 'axios';
import config from '../config';

interface User {
  username: string;
  email: string;
  password: string;
  confirm: string;
}

export default defineComponent({
  data(){
    return {
      posts: null,
      id:"",
      title:"",
      description:"",
      img_text: "Upload",
      featured_image: "",
      token: localStorage.getItem('token')
    }
  },
  methods:{
      submitForm(){
        let formData = new FormData();
        formData.append('title', this.title);
        formData.append('description', this.description);
        let apiPath;
        let msg;
        if(this.featured_image)
          formData.append('featured_image', this.featured_image);
        if(this.id !== ''){
          formData.append('id', this.id);
          apiPath = '/posts/update';
          msg = 'Post updated successfully!!';
        }else{
          apiPath = '/posts/create';
          msg = 'Post created successfully!!';
        }
        axios.post( config.apiUrl + apiPath, formData, {
        headers: {"auth-token": this.token},
        }).then((res) => {
          if(res.data.status == true){
            this.$swal.fire({
              position: 'top-end',
              icon: 'success',
              title: msg,
              showConfirmButton: false,
              timer: 1500
            })
            this.fetchPosts();
          }else{
            this.$swal.fire({
              position: 'top-end',
              icon: 'error',
              title: 'Opps!! Something went wrong',
              showConfirmButton: false,
              timer: 1500
            })
          }
        })
        .catch(function(err){
          console.log(err);
      });
    },
    handleFileUpload(event){
      this.img_text = event.target.files[0].name;
      this.featured_image = event.target.files[0];
    },
    deletePost(id){
      this.$swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!'
      }).then((result) => {
        if (result.isConfirmed) {
          axios.delete( `${config.apiUrl}/posts/delete`,{
                headers: {"auth-token": this.token},
                data:{id}
          }).then((res) => {
              if(res.data.status == true){
                this.$swal.fire({
                  position: 'top-end',
                  icon: 'success',
                  title: 'Post has been deleted',
                  showConfirmButton: false,
                  timer: 1500
                })
                this.fetchPosts();
              }
            })
            .catch(function(err){
              this.$swal.fire({
                  position: 'top-end',
                  icon: 'error',
                  title: 'Opps!! Something went wrong',
                  showConfirmButton: false,
                  timer: 1500
                })
          });
        }
      })
    },
    closeReset(){
        this.title = '';
        this.description = '';
        this.open = false;
        this.img_text = 'Upload';
        this.id = '';
    },
    editPost(id){
      axios.get(`${config.apiUrl}/posts/get/`+id,{
          headers: {"auth-token": this.token}
      }).then(res => {
        if(res.data){
          this.title = res.data.title;
          this.description = res.data.description;
          this.open = true;
          this.id = id;
          if(res.data.featured_image !== '')
            this.img_text = res.data.featured_image;
        }
      });
    },
    async fetchPosts(){
       const posts = await axios.get(`${config.apiUrl}/posts/get`,{
            headers: {"auth-token": this.token}
        })
        this.posts = posts.data;
        this.closeReset()
    }
  },
  async created(){
    if(this.token){
      try{
        const user = await axios.get(`${config.apiUrl}/get/user`,{
          headers: {"auth-token": this.token}
        });
        this.fetchPosts();
      }catch(err){
        localStorage.removeItem('token');
        this.$router.push('/');
      };
    }else this.$router.push('/');
  },
  setup() {
    const open = ref(false);
    return {
      open
    };
  },
});
</script>
