<template>
  <router-link to="/shop">商店</router-link><br />

  <table align="center" v-for="(food_, id) in tempfn1.length" v-bind:key="id">
    <tr>
      食物名稱：{{
        this.tempfn1[food_ - 1]
      }}
      數量：{{
        this.tempfq1[food_ - 1]
      }}
      數量:<input type="number" v-model="this.tempfq1[food_ - 1]" />
      <input type="button" @click="deletefood(id)" />
    </tr>
  </table>
  <input v-if="num == 1" type="submit" @click="ending()" />
</template>

<script>
export default {
  data() {
    return {
      cart: [],
      tempfn1: [],
      tempfq1: [],
      num: 0,
    };
  },
  mounted() {
    if (this.$cookies.get("cart") != null) {
      this.tempfn1 = this.$cookies.get("cart").split(",");
      this.tempfq1 = this.$cookies.get("quantity").split(",");
      this.num += 1;
    }
  },
  methods: {
    ending() {
      this.$router.push("/ending");
    },
    deletefood(id) {
      this.tempfn1.splice(id, 1);
      this.$cookies.set("cart", this.tempfn1);
      this.tempfq1.splice(id, 1);
      this.$cookies.set("quantity", this.tempfq1);
    },
    changecart(food) {
      for (let i = 0; i < this.tempfq1.length; i++) {
        if (this.tempfq1[food].name === this.tempfn1[i]) {
          this.price +=
            this.food.foods[food].price * this.food.foods[food].quantity;
          this.tempfq[i] =
            Number(this.tempfq[i]) + this.food.foods[food].quantity;
          this.$cookies.set("quantity", this.tempfq);
          break;
        }
      }
    },
  },
};
</script>