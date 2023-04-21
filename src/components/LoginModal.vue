<template>
  <div class="container">
    
    <div class="modal" v-if="modal">  
      <div class="input-btn-con">
        <form @submit.prevent="submitForm">
          <div class="e-p-btn">
            <input type="email" v-model="email" placeholder="Email"/>
            <input type="password" v-model="password" placeholder="Password"/>   
          </div>
          <div class="d-log-btn">
            <button type="submit" class="log-btn">Login</button>
          </div>
        </form>
      </div>
      <div class="btn-x">
        <button @click="closeModal">X</button>
      </div>
    </div>

    <div class="d-btn">
      <button @click="openModal">Open</button>
      <button @click="logOut">Log Out</button>
    </div>
    
  </div>
</template>

<script>
import {reactive, ref, toRefs} from "vue"
import app from '../firebase'
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

export default {
  name: 'LoginModal',
  setup(){
    const form = reactive({
      email: "",
      password: ''
    })
    const modal = ref(false);
    const isloggedIn = ref(false);

    function openModal(){
      modal.value = true
    }

    console.log(app)

    function closeModal(){
      modal.value = false
    }

    function submitForm(){
      const auth = getAuth();
      signInWithEmailAndPassword(auth, form.email, form.password)
        .then((userCredential) => {
          // Signed in 
          
          const user = userCredential.user;
          isloggedIn.value = true
          closeModal()
          // ...
          console.log(user)
          alert("Successfully logged in")
        })
        .catch((error) => {
          console.log(error)
          // const errorCode = error.code;
          // const errorMessage = error.message;
        });
    }


    return{
      modal,
      openModal,
      closeModal,
      ...toRefs(form),
      submitForm,
      // logOut
    }
  }
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.modal{
  position: fixed;
  background-color: rgba(36, 89, 114, 0.4);
  width: 100%;
  height: 100%;
}

.input-btn-con{
 display: flex;
 justify-content: center;
 flex-direction: column;
 margin-top: 40vh;
 align-items: center;
}




input{
  display: block;
  margin: 0 .0.5vh;
  padding: 1vh 2vw 1vh 0.5vw;
}

.d-log-btn{
  display: flex;
 justify-content: center;
 align-items: center;
}

.log-btn{
  padding: 1vh 1.5vw;
  background-color: rgb(195, 243, 195);
  border: none;
}

.btn-x{
  position: absolute;
  top: 32vh;
  right: 40vw;
}

.btn-x button{
  padding: 1vh 1.5vw;
  background-color: red;
  border: none;
}

</style>
