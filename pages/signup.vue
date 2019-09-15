<template>
  <v-form v-model="valid">
    <v-container cols="2">
      <v-row>
        <v-col cols="10" md="6">
          <v-text-field
            v-model="firstname"
            :rules="nameRules"
            :counter="10"
            label="Họ"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="10" md="6">
          <v-text-field
            v-model="lastname"
            :rules="nameRules"
            :counter="10"
            label="Tên"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="10" md="6">
          <v-menu
            ref="menu"
            v-model="menu"
            :close-on-content-click="false"
            :return-value.sync="birth"
            transition="scale-transition"
            offset-y
            full-width
            min-width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                v-model="birth"
                label="Ngày sinh"
                readonly
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="birth" no-title scrollable>
              <div class="flex-grow-1"></div>
              <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.menu.save(birth)">OK</v-btn>
            </v-date-picker>
          </v-menu>
        </v-col>

        <v-col cols="10" md="6">
          <v-menu
            ref="menu"
            v-model="menu1"
            :close-on-content-click="false"
            :return-value.sync="date"
            transition="scale-transition"
            offset-y
            full-width
            min-width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                v-model="date"
                label="Ngày đăng ký"
                readonly
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="date" no-title scrollable>
              <div class="flex-grow-1"></div>
              <v-btn text color="primary" @click="menu1 = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.menu1.save(date)">OK</v-btn>
            </v-date-picker>
          </v-menu>
        </v-col>

        <v-col cols="10" md="10">
          <v-text-field
            v-model="email"
            :rules="emailRules"
            label="E-mail"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="10" md="10">
          <v-text-field
            v-model="password"
            :append-icon="show1 ? 'ẩn' : 'hiện'"
            :rules="[rules.required, rules.min]"
            :type="show1 ? 'text' : 'password'"
            name="input-10-1"
            label="Normal with hint text"
            hint="At least 8 characters"
            counter
            @click:append="show1 = !show1"
          ></v-text-field>
        </v-col>

        <v-col cols="10" md="3">
          <v-switch
            v-model="state"
            :counter="10"
            label="Trạng thái"
            required
          ></v-switch>
        </v-col>
      </v-row>
      <v-btn color="success" class="mr-4" @click="validate">Sign Up</v-btn>
    </v-container>
  </v-form>
</template>

<script>
export default {
  data: () => ({
    date: new Date().toISOString().substr(0, 10),
    birth: new Date().toISOString().substr(0, 10),
    menu: false,
    menu1: false,
    valid: false,
    firstname: '',
    lastname: '',
    nameRules: [
      v => !!v || 'Name is required',
      v => v.length <= 10 || 'Name must be less than 10 characters'
    ],
    email: '',
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /.+@.+/.test(v) || 'E-mail must be valid'
    ],
    show1: false,
    password: '',
    rules: {
      required: value => !!value || 'Required.',
      min: v => v.length >= 8 || 'Min 8 characters',
      emailMatch: () => ('The email and password you entered don\'t match')
    },
    state: false
  })
}
</script>
