<template>
  <v-app class="background" >
    <v-card v-if="!isRegister" class="pa-5 mx-auto my-auto" width="400" color="#B9E0FF">
      <v-card v-if="messages != ''" color="success">{{messages}}</v-card>
      <v-card-title>
        <div class="title-1 mx-auto">
          Salt-pepper Login
        </div>
      </v-card-title>

      <v-card-text  class="pb-0">
        <v-text-field
          prepend-inner-icon="mdi-account"
          ref="email"
          v-model="form.email"
          :rules="emailRules"
          :error-messages="errorMessages"
          label="Email">
        </v-text-field>
      </v-card-text>

      <v-card-text>
        <v-text-field
        prepend-inner-icon="mdi-key"
          :type="passwordShow? 'text' : 'password'"
          ref="password"
          v-model="form.password"
          :rules="[() => !!form.password || 'This field is required']"
          :error-messages="errorMessages"
          :append-icon="passwordShow ? 'mdi-eye' : 'mdi-eye-off'"
          @click:append="passwordShow = !passwordShow"
          label="Password">
        </v-text-field>
      </v-card-text>

      <v-card-actions class="d-flex flex-column">
        <v-btn class="primary mx-auto mb-2" block @click="login">
          Login
        </v-btn>
        <v-btn class="mx-auto" text color="primary" @click="isRegister = true">
          Register
        </v-btn>
      </v-card-actions>

    </v-card>

    <v-card v-else class="pa-5 mx-auto my-auto" width="400">
      <v-card v-if="messages != ''" color="success">{{messages}}</v-card>
      <v-card-title>
        <div class="title-1 mx-auto">
          Pepper Salt Register
        </div>
      </v-card-title>

      <v-card-text  class="pb-0">
        <v-text-field
        prepend-inner-icon="mdi-account"
          ref="name"
          v-model="form_register.name"
          :rules="[() => !!form_register.name || 'This field is required']"
          :error-messages="errorMessages"
          label="Name">
        </v-text-field>
      </v-card-text>

      <v-card-text  class="pb-0">
        <v-text-field
        prepend-inner-icon="mdi-mail"
          ref="email2"
          v-model="form_register.email2"
          :rules="emailRules2"
          :error-messages="errorMessages"
          label="Email">
        </v-text-field>
      </v-card-text>

      <v-card-text>
        <v-text-field
        prepend-inner-icon="mdi-key"
          ref="password2"
          v-model="form_register.password2"
          :rules="[() => !!form_register.password2 || 'This field is required']"
          :error-messages="errorMessages"
          label="Password">
         
        </v-text-field>
       
      </v-card-text>

      <v-card-actions class="d-flex flex-column">
        <v-btn class="primary mx-auto mb-2" block @click="register">
          Register
        </v-btn>
        <v-btn class="mx-auto" text color="primary" @click="isRegister = false">
          Login
        </v-btn>
      </v-card-actions>

    </v-card>
  </v-app>    

</template>

<script>

export default {
  data() {
    return {
      
      passwordShow:false,
      isRegister: false,
      errorMessages: '',
      messages: '',
      form: {
        email: '',
        password: '',
        
      },
      form_register: {

        name: '',
        email2: '',
        password2: '',
      },
      emailRules: [() => !!this.form.email || 'This field is required', v => !v || /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'],
      emailRules2: [() => !!this.form_register.email2 || 'This field is required', v => !v || /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid']
    }
  },
  methods: {
    async login() {
      let validate = true
      Object.keys(this.form).forEach(f => {

        if(!this.$refs[f].validate()) {
          this.$refs[f].validate(true)
          validate = false
        } 
       
      })

      if(validate) {
        let obj = {
          email: this.form.email,
          password: this.form.password,
        }
        let res = await this.$axios.post('/login', obj)
        if(res.status == 200) {
          this.messages = "Succesfully Login!!!"

          setTimeout(() => {
            this.$router.push('/home')
          }, 2000)

        }
        
      }

    },
    async register() {
      let validate = true
      Object.keys(this.form_register).forEach(f => {

        if(!this.$refs[f].validate()) {
          this.$refs[f].validate(true)
          validate = false
        } 
       
      })

      if(validate) {
        let obj = {
          name: this.form_register.name,
          email: this.form_register.email2,
          password: this.form_register.password2,
        }
        let res = await this.$axios.post('/register', obj)
        if(res.status == 200) {
          this.messages = "Succesfully registered!!!"

          setTimeout(() => {
            this.messages = ''
          }, 2000)
        }
      }

    }
  }
}
</script>


<style>
.background{
  background-image: url(./static/image/doms.jpg) !important;
  height: 400px;
  width: 100%;
  display: block;
  position: absolute;
  top: 0;
  background-size: cover;

}
</style>

