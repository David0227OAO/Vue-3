<template>
    <h1>感謝購買</h1>
    購買序號：{{ serial_number }}
    <table
      align="center"
      v-for="(food_, id) in this.tempfn.length"
      v-bind:key="id"
    >
      <tr>
        食物名稱：{{
          this.tempfn[food_ - 1]
        }}
        數量：{{
          this.tempfq[food_ - 1]
        }}
      </tr>
    </table>
    總價:{{ this.totle }}
    <router-link to="/shop">返回主頁面</router-link>
  </template>
  <script>
  export default {
    data() {
      return {
        serial_number: "111",
        totle: 0,
        tempfn: [],
        tmepfm: [],
        tempfq: [],
      };
    },
    mounted() {
      this.tempfn = this.$cookies.get("cart").split(",");
      this.tempfm = this.$cookies.get("money").split(",");
      this.tempfq = this.$cookies.get("quantity").split(",");
      for (let i = 0; i < this.tempfm.length; i++) {
        this.totle += this.tempfm[i] * this.tempfq[i];
      }
      this.$cookies.remove("cart");
      this.$cookies.remove("money");
      this.$cookies.remove("quantity");
    },
  };
  </script>