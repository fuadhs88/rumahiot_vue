<template>
      <v-layout>
        <v-flex xs12 sm6 offset-sm3 mt-3 >
          <v-card>
            <v-card-media src="/static/img/other/technology-3092486_640.jpg" height="200px">
            </v-card-media>
            <v-card-title primary-title>
              <div>
                <h3 class="headline mb-0">Registrasi</h3>
                <div>Dengan Rumah IoT , kamu tidak perlu memikirkan tempat untuk menyimpan data</div>
              </div>
            </v-card-title>
            <v-container>
              <v-form v-model="valid">
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
                <div class="g-recaptcha" data-sitekey="6Leimz8UAAAAAOROGyNnlZKjfJW_bFzqXUXodgez"></div>
                <v-btn block @click="submit" :disabled="!valid" large active-class color="primary">Daftar</v-btn>
              </v-form>
            </v-container>
          </v-card>
        </v-flex>
      </v-layout>
</template>

<script>
  import axios from 'axios'

  export default {
    data: () => {
      return {
        fixed: false,
        title: 'Rumah IoT',
        dialog: false,
        // show/hide password icon
        p1: true,
        p2: true,
        valid: false
      }
    },
    // recaptcha loader
    created: () => {
      let recaptchaScript = document.createElement('script')
      recaptchaScript.setAttribute('src', 'https://www.google.com/recaptcha/api.js')
      document.head.appendChild(recaptchaScript)
    },
    submit: () => {
      if (this.$refs.form.validate()) {
        axios.post('/api/submit', {
          name: this.name,
          email: this.email,
          select: this.select,
          checkbox: this.checkbox
        })
      }
    }
  }
</script>



