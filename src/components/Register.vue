<template>
      <v-layout>
        <v-flex xs12 sm6 offset-sm3 mt-3 >
          <v-card>
            <v-card-media src="/static/img/other/technology-3092486_640.jpg" height="200px">
            </v-card-media>
            <v-alert color="success" icon="new_releases" :value="true">
              This is a success alert with a custom icon.
            </v-alert>
            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">Registrasi</h3>
                <div>Dengan Rumah IoT , kamu tidak perlu memikirkan tempat untuk menyimpan data</div>
              </div>
            </v-card-title>
            <v-container>
              <v-form v-model="valid" ref="form" lazy-validation>
                <v-text-field
                  name="full_name"
                  label="Nama Lengkap"
                  v-model="fullName"
                  :rules ="[() => !!fullName || 'Harap masukkan nama lengkap anda',() => fullName.length <= 70 || 'Nama lengkap tidak dapat lebih 70 karakter']"
                  :counter="70"
                  required
                ></v-text-field>
                <v-text-field
                  name="email"
                  label="Alamat Email"
                  v-model="email"
                  :rules="[() => !!email || 'Harap masukkan e-mail anda',() => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(email) || 'Harap masukkan alamat email yang valid',() => email.length <= 254 || 'Alamat e-mail tidak dapat lebih dari 254 karakter']"
                  required
                ></v-text-field>
                <!--todo: add password length checker in this form-->
                <v-text-field
                  name="password"
                  label="Password"
                  v-model="password"
                  :append-icon="p1 ? 'visibility' : 'visibility_off'"
                  :append-icon-cb="() => (p1 = !p1)"
                  :type="p1 ? 'password' : 'text'"
                  :rules="[() => !!password || 'Harap masukkan password yang akan digunakan',() => password.length <= 50 || 'Password tidak dapat lebih 50 karakter']"
                  counter
                ></v-text-field>
                <v-text-field
                  name="retype_password"
                  label="Password (Ulangi)"
                  v-model="retypePassword"
                  :append-icon="p2 ? 'visibility' : 'visibility_off'"
                  :append-icon-cb="() => (p2 = !p2)"
                  :type="p2 ? 'password' : 'text'"
                  :rules="[() => !!retypePassword || 'Harap masukkan kembali password yang akan digunakan',() => password == retypePassword || 'Kedua password tidak sama',() => password.length <= 50 || 'Password tidak dapat lebih 50 karakter']"
                  counter
                ></v-text-field>
                <vue-recaptcha  ref="invisibleRecaptcha"
                                @verify="onVerify"
                                @expired="onExpired"
                                size="invisible"
                                :sitekey=sitekey>
                <v-btn block :disabled="!valid" large active-class color="primary">Daftar</v-btn>
                </vue-recaptcha>
              </v-form>
            </v-container>
          </v-card>
        </v-flex>
      </v-layout>
</template>

<script>
  import axios from 'axios'
  import VueRecaptcha from 'vue-recaptcha'
  export default {
    data: () => {
      return {
        fixed: false,
        title: 'Rumah IoT',
        dialog: false,
        // show/hide password icon
        p1: true,
        p2: true,
        valid: false,
        sitekey: '6Lf7OUIUAAAAAM56DMwL4YqZB77RRS9SYolHllM7'
      }
    },
    methods: {
      onSubmit (recaptchaResponse) {
        if (this.$refs.form.validate()) {
          const emailRegistrationEndpoint = 'http://localhost:8000/authenticate/email/register'
          const fd = new FormData()
          fd.append('full_name', this.fullName)
          fd.append('email', this.email)
          fd.append('password', this.password)
          fd.append('retype_password', this.retypePassword)
          fd.append('g_recaptcha_response', recaptchaResponse)
          axios.post(emailRegistrationEndpoint, fd)
            .then(function (result) {
              console.log(result)
            }, function (error) {
              console.log(error.response.data)
            })
        }
      },
      onVerify (response) {
        this.$refs.invisibleRecaptcha.execute()
        this.onSubmit(response)
      },
      onExpired () {
        console.log('Expired')
      },
      resetRecaptcha () {
        this.$refs.recaptcha.reset() // Direct call reset method
      }
    },
    components: { VueRecaptcha }
  }
</script>



