<template>
  <div class=" text-center">
    <v-dialog width="200" v-model="loading" persistent>
      <v-card height="40" class="pt-5 px-5" color="primary">
        <v-progress-linear indeterminate color="white"></v-progress-linear>
      </v-card>
    </v-dialog>


    <v-form ref="form" v-model="valid" lazy-validation>
      <div class="bgs">
        <div class="control-row">
          <div style="width:100%;display:flex;flex-direction:row;justify-content:center;height:100vh;align-items:center">
            <img src="../assets/images/bgs.jpg" width="100%" />
          </div>
          <div style="width:100%;height:100vh;display:flex;flex-direction:row;justify-content:center;align-items:center">
            <v-card width="350" class="mx-auto pb-10 card-shadow" rounded
              style="background-color: white; margin-left: 200px">
              <div class="text-center pt-5">
                <img class="mx-auto" src="../assets/images/logo.jpeg" width="50" style="border-radius: 50%" />

              </div>
              <div class="text-center font-weight-bold pt-1 pb-5" style="font-size: 14pt">
                Login
              </div>
              <v-card-text class="px-10">
                <v-text-field label="ຊື່ຜູ້ໃຊ້" dense placeholder="ປ້ອນຊື່ຜູ້ໃຊ້" prepend-inner-icon="mdi-account-outline"
                  outlined v-model="username" :rules="nameRules"></v-text-field>
                <v-text-field label="ລະຫັດຜ່ານ" dense placeholder="*********"
                  :append-icon="showPassword ? 'mdi-eye-off' : 'mdi-eye'" prepend-inner-icon="mdi-lock" outlined
                  v-model="password" :rules="nameRules" :type="showPassword ? 'password' : 'text'"
                  @click:append="onShowPass" @keyup.enter="myFunction"></v-text-field>
                <v-btn color="primary px-5" small class="mt-1" height="40" rounded elevation="0" block
                  @click="myFunction">
                  ເຂົ້າສູ່ລະບົບ
                  <v-spacer></v-spacer>
                  <v-icon>mdi-arrow-right</v-icon>
                </v-btn>
              </v-card-text>
            </v-card>
            <br />
            <span class="mt-8 white--text">ພັດທະນາໂດຍຂະແໜງໂມບາຍແບັງຄີງ</span>
          </div>
        </div>
      </div>

    </v-form>
    <!-- mobile -->

  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: '_blank',
  data() {
    return {
      loading: false,
      showPassword: true,
      connection: true,
      username: '',
      password: '',
      valid: true,
      nameRules: [(v) => !!v || 'ຕ້ອງປ້ອນຂໍ້ມູນ'],
    }
  },
  methods: {

    onLogin() {
      this.loading = true
      this.$router.push('/securities_location/nationwide')
      this.loading = true
    },
    onShowPass() {
      this.showPassword = !this.showPassword
    },
    myFunction: function () {
      if (!this.$refs.form.validate()) return null
      if (navigator.onLine) {
        try {
          this.loading = true;
          let data = {
            user: this.username,
            password: this.password
          }
          this.$axios.$post('/Login', data).then((data) => {
            if (data?.code === '00') {
              console.log("login:", data)
              localStorage.setItem('branchCode', data?.data?.branchCode)
              localStorage.setItem('branchName', data?.data?.branchName)
              localStorage.setItem('userLogin', data?.data?.userLogin)
              localStorage.setItem('userType', data?.data?.userType)
              localStorage.setItem('ee4a2604-bd95-4590-ab83-c4c37cf385f8', 'i am not hacker')
              this.loading = false;
              this.$router.push('/report-income-expand')
              this.connection = true
            } else {
              this.loading = false;

            }
          })
        } catch (error) {
          console.log(error)
          this.loading = false
        }

      }
    },
  },
  mounted() {
    localStorage.clear();
  }
}
</script>
<style scoped>
.bgs {
  /* background-image: url('../assets/images/bgs.jpg');
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover; */
  height: 100vh;
  background-color: #fff;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.control-row {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
}
</style>
