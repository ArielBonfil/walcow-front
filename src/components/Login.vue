<template>
  <section class="src-components-login">
    <div class="container">
      <vue-form :state="formState" @submit.prevent="enviar()">
        <h1>Log in</h1>
        <!-- user -->

        <validate tag="div">
          <label for="user">Username</label>
          <input
            name="username"
            id="username"
            type="text"
            placeholder="Enter username"
            v-model.trim="formData.username"
            required
          />
          <field-messages name="username" show="$dirty">
            <div slot="required" class="alert alert-danger error">
              Enter your username
            </div>
          </field-messages>
        </validate>

        <!-- password -->
        <validate tag="div">
          <label for="password">Password</label>
          <input
            name="password"
            id="password"
            type="password"
            placeholder="Enter password"
            v-model.trim="formData.password"
            required
          />
          <field-messages name="password" show="$dirty">
            <div slot="required" class="alert alert-danger error">
              Enter your password
            </div>
          </field-messages>
        </validate>
        <!-- btn ingresar -->
        <button
          type="submit"
          :disabled="formState.$invalid"
          class="btn btn-light"
        >
          <b>log in</b>
        </button>

        <div class="alert alert-danger error mt-3" v-if="invalidCredentials">
          {{ this.errorMessage }}
        </div>
      </vue-form>
    </div>
  </section>
</template>

<script lang="js">
  import axios from "axios";
  export default  {
    name: 'src-components-login',
    props: [],
    mounted () {

    },
    data () {
      return {
        formData: this.getInicialData(),
        formState: {},
        invalidCredentials: false,
        errorMessage: ''
      }
    },
    methods: {
       getInicialData(){
        return{
          username: '',
          password: '',
        }
      },
       enviar(){
        //this.$store.state.success
        //console.log({...this.formData})
        let credentials = {
          username: this.formData.username,
          password: this.formData.password
        }
        try {
          axios.post("http://localhost:4000/api/users/login", credentials)
          .then(res =>{
            this.invalidCredentials = !res.data.canLogin;
            this.errorMessage = res.data.message
            if(res.data.canLogin){
             this.$store.dispatch('access', res.data.canLogin)
             this.$router.push('/home')
            }
          })
        } catch (error) {
          console.log(error)
        }

        this.formData = this.getInicialData()
        this.formState._reset()
      }
    },
    computed: {

    }
}
</script>

<style scoped lang="css">
.container label {
  padding: 0px;
  margin: 0px;
  display: block;
  margin-top: 20px;
  font-size: 17px;
}

.container h1 {
  padding: 0px;
  margin: 0px;
  display: block;
  text-align: center;
}

.container input {
  width: 100%;
  margin-bottom: 10px;
  margin-top: 10px;
  border: none;
  border-bottom: 1px solid #fff;
  background: transparent;
  outline: none;
  color: #fff;
}

.container button {
  display: block;
  width: 100%;
  margin-top: 15px;
  border-radius: 20px;
}

.container button:hover {
  background-color: rgb(189, 200, 200);
}

.error {
  text-align: center;
  color: red;
  background-color: transparent;
  border: none;
  padding: 0px;
}
</style>
