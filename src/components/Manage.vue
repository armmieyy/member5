<template>
  <div class="c xs">
    <br /><br />
    <h1 class="ml-16">Manage User</h1>
    <br />
    <br /><br />
    <v-row no-gutters>
      <v-col cols="6" v-for="(item, index) in apidata" :key="index">
        <v-card elevation="8" class="pb-4 my-8 mx-16">
          <v-row class="mb-2 mr-10 ml-10 align-center">
            <v-card-title class="mx-20">
              Email : {{ item.email }}
            </v-card-title>

            <v-card-title> First Name : {{ item.fname }} </v-card-title>
            <v-card-title> Last Name : {{ item.lname }} </v-card-title>
          </v-row>
          <v-card-actions class="mr-14 ml-16">
            <!-- <v-btn color="info" @click="info(item._id)">Details</v-btn> -->
            <!-- <v-spacer></v-spacer> -->
            <v-btn color="warning" @click="editItemMode(item)">edit</v-btn>
            <v-spacer></v-spacer>
            <v-btn color="error" @click="deleteItemMode(item)">delete</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-dialog v-model="dialogedit" max-width="500px" class="">
      <v-card>
        <v-card-title>{{ savemode }}</v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="6">
              <v-text-field
                name="email"
                :rules="[rules.required, rules.email]"
                label="Email"
                id="email"
                type="email"
                v-model="postdata.email"
              ></v-text-field>
            </v-col>
            <!-- <v-col cols="6">
              <v-text-field
                name="password"
                :rules="rules.password"
                label="Password"
                id="password"
                type="password"
                v-model="postdata.password"
              ></v-text-field>
            </v-col> -->
            <v-col cols="6">
              <v-text-field
                name="fname"
                :rules="[rules.name, rules.required]"
                label="First Name"
                id="fname"
                v-model="postdata.fname"
              ></v-text-field>
            </v-col>
            <v-col cols="6">
              <v-text-field
                name="lname"
                :rules="[rules.name, rules.required]"
                label="Last Name"
                id="lname"
                v-model="postdata.lname"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions class="pb-8 justify-end pr-8">
          <v-btn color="error" text @click="closeData()">cancel</v-btn>
          <v-btn color="success" @click="selectSave()">save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogdelete" max-width="500px">
      <v-card>
        <v-card-title> Delete it </v-card-title>
        <v-card-text> are you sure ? </v-card-text>
        <v-card-actions>
          <v-btn color="info" text @click="closeData()"> cancel </v-btn>
          <v-btn color="error" @click="deleteData()"> delete </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: "",
      apidata: [],
      dialogedit: false,
      dialogdelete: false,
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
      if (this.savemode == "Edit User") {
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

<style scoped>
.c {
  margin-left: 250px;
  margin-right: 250px;
  /* border: 1px solid red; */
  /* text-align: center; */
}
</style>
