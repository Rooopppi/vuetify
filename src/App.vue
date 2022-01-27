<template>
  <v-app>
    
    <v-navigation-drawer app width="20%" permanent>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6"> Menu </v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list-group prepend-icon="mdi-menu">
        <v-list-item v-for="item in items" :key="item.title" link>
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list-group>
    </v-navigation-drawer>

    <v-main>
      <v-form class="pa-10" ref="form" v-model="valid" lazy-validation>
        <v-text-field
          v-model="userName"
          :rules="userNameRules"
          label="userName"
          required
        ></v-text-field>

        <v-text-field
          v-model="age"
          :rules="ageRules"
          label="age"
          required
        ></v-text-field>

        <v-select
          v-model="gender"
          :items="genders"
          :rules="[(v) => !!v || 'Gender is required']"
          label="gender"
          required
        ></v-select>

        <v-btn :disabled="!valid" color="success" class="mr-4" @click="submit">
          Submit
        </v-btn>

        <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
      </v-form>

      <v-divider class="mb-10"></v-divider>

      <v-data-table
        :headers="headers"
        :items="users"
        :items-per-page="10"
        class="elevation-1"
      ></v-data-table>

      <v-snackbar
        color="green"
        right
        rounded="pill"
        v-model="snackbar"
      >
        {{ text }}

        <template v-slot:action="{ attrs }">
          <v-btn text v-bind="attrs" @click="snackbar = false"> Close </v-btn>
        </template>
      </v-snackbar>

    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  components: {},

  data: () => {
    return {
      users: [],
      headers: [
        {
          text: "Users",
          align: "start",
          sortable: false,
          value: "userName",
        },
        { text: "age", value: "age" },
        { text: "gender", value: "gender" },
      ],
      items: [
        { title: "Home", icon: "mdi-home" },
        { title: "Photos", icon: "mdi-image" },
        { title: "About", icon: "mdi-help-box" },
      ],
      right: null,

      valid: true,
      userName: "",
      userNameRules: [
        (v) => !!v || "UserName is required",
        (v) =>
          (v && v.length >= 3) || "UserName must be more than 3 characters",
        (v) => /[A-z]/.test(v) || "UserName must be A-z",
      ],
      age: "",
      ageRules: [
        (v) => !!v || "Age is required",
        (v) => +v >= 18 || "Age must be 18 and more",
      ],
      gender: "",
      genders: ["male", "female", "other"],
      snackbar: false,
      text: "Success",
    };
  },

  methods: {
    submit() {
      if (!this.$refs.form.validate()) return;

      const newUser = {
        userName: this.userName,
        age: this.age,
        gender: this.gender,
      };

      this.users = [...this.users, newUser];

      this.snackbar = true;
    },
    reset() {
      this.$refs.form.reset();
    },
  },

  created() {
    this.users = [
      {
        userName: "Alex",
        age: 23,
        gender: "male",
      },
      {
        userName: "Dima",
        age: 25,
        gender: "male",
      },
      {
        userName: "Den",
        age: 43,
        gender: "male",
      },
      {
        userName: "Ann",
        age: 18,
        gender: "female",
      },
    ];
  },
};
</script>
