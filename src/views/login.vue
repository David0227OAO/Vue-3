<template>
  <form
    method="post"
    style="
      border: 1px solid green;
      max-width: 360px;
      height: 400px;
      margin: 150px auto 0;
    "
  >
    <div style="padding-top: 30px">
      <input
        type="text"
        placeholder="username"
        v-model="auser"
        style="height: 25px; padding: 5px"
      />
    </div>

    <div style="padding-top: 20px">
      <input
        type="password"
        placeholder="password"
        v-model="apassword"
        style="height: 25px; padding: 5px"
      />
      <p id="errorpwd"></p>
    </div>
    <input
      type="submit"
      value="Login"
      class="button"
      @click="validateForm()"
      style="
        height: 38.2px;
        width: 175.6px;
        margin-top: 50px;
        background-color: transparent;
        boder-style: none;
      "
    />
  </form>
</template>

<script>
  import account from "../static/account/login.json";
  export default {
    data() {
      return {
        account: account,
        auser: "111",
        apassword: "111",
        temp: 0,
      };
    },
    methods: {
      validateForm() {
        this.temp = 0;
        console.log(this.account.person.length);
        for (let i = 0; i < this.account.person.length; i++) {
          if (this.auser === this.account.person[i].account) {
            if (this.apassword === this.account.person[i].password) {
              this.$cookies.set("name", this.account.person[i].name);
              this.$cookies.set("position", this.account.person[i].position);
              this.$router.push("/shop");
            } else {
              this.temp++;
              console.log(this.temp);
            }
          } else {
            this.temp++;
            console.log(this.temp);
          }
          if (
            this.temp === this.account.person.length ||
            this.temp === this.account.person.length * 2 - 1
          ) {
            document.getElementById("errorpwd").innerHTML = "帳號或密碼錯誤";
          }
        }
      },
    },
  };
  </script>
