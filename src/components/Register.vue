<template>
  <v-container>
    <v-card class="mx-auto mt-16 pt-5 pb-5 ml-17 mr-17" max-width="700">
      <v-form ref="form">
        <h3 class="namepage">Sign Up</h3>
        <v-container fluid>
          <v-row>
            <v-col cols="12" sm="6">
              <v-text-field
                v-model="postdata.email"
                :rules="[rules.required, rules.email]"
                color="purple darken-2"
                label="Email"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field
                v-model="postdata.password"
                :rules="rules.password"
                color="purple darken-2"
                label="Password"
                type="password"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field
                v-model="postdata.fname"
                :rules="[rules.name, rules.required]"
                color="purple darken-2"
                label="First Name"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field
                v-model="postdata.lname"
                :rules="[rules.name, rules.required]"
                color="purple darken-2"
                label="Last Name"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-card-actions class="mt-5">
            <v-btn color="error" @click="reset"> Reset </v-btn>
            <v-btn depressed to="/">Back</v-btn>
            <v-spacer></v-spacer>
            <v-btn color="success" @click="selectSave()">Register</v-btn>
          </v-card-actions>
        </v-container>
      </v-form>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      id: "",
      apidata: [],
      dialogedit: false,
      dialogdelete: false,
      dialogdetail: false,
      itemdata: [],
      postdata: {
        // ตัวส่งข้อมูลหลัก
        email: "",
        password: "",
        fname: "",
        lname: "",
      },
      postdefault: {
        // ตัวเคลียค่าว่าง
        email: "",
        password: "",
        fname: "",
        lname: "",
        // username: '',
        // password: '',
        // name: '',
        // address: '',
        // moneypag: 1234
      },
      rules: {
        required: (value) => !!value || "Required.",
        name: [(val) => (val || "").length > 0 || "This field is required"],
        confirm_password: [
          (val) => (val || "").length > 0 || "This field is required",
        ],
        min: (v) => v.length >= 8 || "Min 8 characters",
        email: (value) => {
          if (value.length > 0) {
            const pattern =
              /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
            return pattern.test(value) || "Invalid e-mail.";
          }
        },
        password: [
          (value) => !!value || "Required password.",
          (value) => (value && value.length >= 8) || "minimum 8 characters",
        ],
        confirmPassword: [
          (value) => !!value || "Required confirm password",
          (value) =>
            value === this.postdata.password ||
            "The password confirmation does not match.",
        ],
      },
    };
  },
  computed: {
    savemode() {
      // เข็คโหมดการบันทึกเรียลไทม์
      // eslint-disable-next-line eqeqeq
      return this.id == "" ? "New User" : "Edit User";
    },
  },
  created() {
    this.getData();
  },
  methods: {
    reset() {
      this.$refs.form.reset();
    },
    submit() {
      this.resetForm();
    },
    getData() {
      this.axios.get("http://localhost:3000/user/").then((response) => {
        console.log(response.data);
        this.apidata = response.data.data;
      });
    },
    async postData() {
      // เพิ่มข้อมูล
      try {
        const { data } = await this.axios.post(
          "http://localhost:3000/register/",
          this.postdata
        );
        alert(data.message);
        this.getData();
        this.postdata = { ...this.postdefault };
        this.dialogedit = false;
      } catch (err) {
        // console.log(err);
        this.$router.push('/login');
      }
    },
    async deleteData() {
      // ลบข้อมูล
      try {
        const { data } = await this.axios.delete(
          "http://localhost:3000/user/" + this.id
        );
        alert(data.message);
        this.getData();
        this.id = "";
        this.postdata = { ...this.postdefault };
        this.dialogedit = false;
        this.dialogdelete = false;
      } catch (err) {
        console.log(err);
      }
    },
    async putData() {
      // แก้ไขข้อมูล
      try {
        const { data } = await this.axios.put(
          "http://localhost:3000/user/" + this.id,
          this.postdata
        );
        alert(data.message);
        this.getData();
        this.postdata = { ...this.postdefault };
        this.dialogedit = false;
      } catch (err) {
        console.log(err);
      }
    },
    selectSave() {
      // ตัดสินใจบันทึกโหมดไหน
      // eslint-disable-next-line eqeqeq
      if (this.savemode == "Edit Item") {
        this.putData();
      } else this.postData();
    },
    newItemMode() {
      // กดปุ่มสร้างใหม่
      this.id = "";
      this.dialogedit = true;
    },
    editItemMode(item) {
      // กดปุ่มแก้ไข
      this.id = item._id;
      this.postdata = { ...item };
      this.dialogedit = true;
    },
    closeData() {
      // กดปุ่มปิด
      this.id = "";
      this.postdata = { ...this.postdefault };
      this.dialogedit = false;
      this.dialogdelete = false;
      this.dialogdetail = false;
    },
    deleteItemMode(item) {
      // กดปุ่มแก้ไข
      this.id = item._id;
      this.dialogdelete = true;
    },
    info(itemid) {
      //
      console.log(itemid);
      this.axios
        .get("http://localhost:3000/product/" + itemid)
        .then((response) => {
          console.log(response.data);
          // this.length = response.data.data.length;
          this.itemdata = response.data.data;
          this.dialogdetail = true;
        });
    },
  },
};
</script>

<style>
.btn11 {
  margin-left: auto;
}
.namepage {
  text-align: center;
  font-size: 30px;
  margin-bottom: 15px;
}
</style>
