<template> 
  <v-container fluid fill-height>
    <v-layout align-center justify-center>
      <v-flex xs12 sm8 md4>
        <v-card class="elevation-12">

          <v-toolbar color="amber">
            <v-toolbar-title>Register</v-toolbar-title> 
          </v-toolbar>

          <form
            @submit.prevent="register"
          >

            <v-card-text>  
                <v-text-field prepend-icon="person" name="name" label="Name" type="name"
                  ref="name"
                  v-model="form.name"
                  :rules="[() => !form.errors.has('name') || form.errors.get('name') ]"
                ></v-text-field>
                
                <v-text-field prepend-icon="email" name="email" label="Email" type="email"
                  ref="email"
                  v-model="form.email"
                  :rules="[() => !form.errors.has('email') || form.errors.get('email') ]"
                ></v-text-field>
              
                <v-text-field prepend-icon="lock" name="password" label="Password" id="password" type="password"
                  ref="password"
                  v-model="form.password"
                  :rules="[() => !form.errors.has('password') || form.errors.get('password') ]"
                ></v-text-field>
              
                <v-text-field prepend-icon="lock" name="password_confirmation" label="Confirm pssword" id="password_confirmation" type="password"
                  ref="password_confirmation"
                  v-model="form.password_confirmation" 
                ></v-text-field> 
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" type="submit">Register</v-btn>
            </v-card-actions>
            
          </form>  

        </v-card>
      </v-flex>
    </v-layout>
  </v-container> 
</template>

<script>
  
  import { mapActions } from 'vuex'
  import Form from 'helpers/Form.js'

  export default {
    data: () => ({
      form: new Form({
        name: '',
        email: '',
        password: '',
        password_confirmation: ''
      })
    }),

    methods: {

      ...mapActions([
        'authSet', 'authInitialize', 'configInitialize', 'configRemove'
      ]),

      register() { 
        this.form.post('/api/register')
          .then(data => {
            this.authSet(data)
            this.authInitialize();

            this.configRemove();
            this.configInitialize();

            this.$router.push('/');
          })
          .catch(errors => {
            this.form.validate(this.$refs)
          })
      }

    }
  }
</script>