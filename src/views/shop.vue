<template>
    <button @click="clearcookie">清除cookie</button><br />
    <p>歡迎{{ this.$cookies.get("name") }}</p>
    <h2>食物總類({{ food.foods.length }}種)</h2>
    <button @click="add_new_food">新增食物</button>
  
    <p v-if="this.position === 'manager'">
    <table v-for="(food_, id) in food.foods" v-bind:key="id" align="center">
      <tr>
        食物名稱：<input type="text" v-model="food_.name" />
        食物種類：<input type="text" v-model="food_.foodtype" />
        單價：<input type="number" v-model="food_.price" />
        辣：<input type="checkbox" v-model="food_.spicy" />
        <button @click="deletefood(id)">刪除</button>
      </tr>
    </table>
    </p>
  
    <p v-if="this.position === 'manager' || 'student' || 'staff'">
    <table v-for="(food_, id) in food.foods" v-bind:key="id" align="center">
      <tr>
        {{
          food_.name
        }}
  
        <span v-if="food_.spicy == true">會辣</span>
  
        單價:{{
          food_.price
        }}
        數量:<input type="number" v-model="food_.quantity" />
        <button @click="addcart(id)">新增至購物車</button>
      </tr>
    </table>
    </p>
    總價:{{ this.price }}<br />
    <router-link to="/cart">購物車</router-link>
  </template>
  
  <script>
  import food from "../static/food.json";
  export default {
    data() {
      return {
        food: food,
        position: null,
        tempfn: [],
        tempfm: [],
        tempfq: [],
        temp: 0,
        price: 0,
      };
    },
    mounted() {
      this.position = this.$cookies.get("position");
      if (this.$cookies.get("cart") !== null) {
        this.tempfm = this.$cookies.get("money").split(",");
        this.tempfq = this.$cookies.get("quantity").split(",");
        for (let i = 0; i < this.tempfm.length; i++) {
          this.price += this.tempfm[i] * this.tempfq[i];
        }
      }
    },
    methods: {
      clearcookie() {
        this.$cookies.remove("cart");
        this.$cookies.remove("quantity");
        this.price = 0;
        console.log("-------------------------");
      },
      addcart(food) {
        this.temp = 0;
        if (this.$cookies.get("cart") === null) {
          this.price +=
            this.food.foods[food].price * this.food.foods[food].quantity;
          this.$cookies.set("cart", this.food.foods[food].name);
          this.$cookies.set("money", this.food.foods[food].price);
          this.$cookies.set("quantity", this.food.foods[food].quantity);
          this.tempfn = this.$cookies.get("cart").split(",");
          this.tempfq = this.$cookies.get("quantity").split(",");
          /*console.log("if");
          console.log(this.tempfn);
          console.log(this.$cookies.get("cart"));
          console.log(this.tempfq);
          console.log(this.$cookies.get("quantity"));*/
        } else {
          this.tempfn = this.$cookies.get("cart").split(",");
          this.tempfq = this.$cookies.get("quantity").split(",");
          for (let i = 0; i < this.tempfn.length; i++) {
            if (this.food.foods[food].name === this.tempfn[i]) {
              this.price +=
                this.food.foods[food].price * this.food.foods[food].quantity;
              this.tempfq[i] =
                Number(this.tempfq[i]) + this.food.foods[food].quantity;
              this.$cookies.set("quantity", this.tempfq);
              break;
            }
            if (this.food.foods[food].name !== this.tempfn[i]) {
              this.temp++;
              //console.log(this.temp);
            }
            if (this.temp === this.tempfn.length) {
              this.price +=
                this.food.foods[food].price * this.food.foods[food].quantity;
              this.$cookies.set(
                "cart",
                this.$cookies.get("cart") + "," + this.food.foods[food].name
              );
              this.$cookies.set(
                "money",
                this.$cookies.get("money") + "," + this.food.foods[food].price
              );
              this.$cookies.set(
                "quantity",
                this.$cookies.get("quantity") +
                  "," +
                  this.food.foods[food].quantity
              );
            }
          }
          /*console.log("else");
          console.log(this.tempfn);
          console.log(this.$cookies.get("cart"));
          console.log(this.tempfq);
          console.log(this.$cookies.get("quantity"));*/
        }
        /*console.log("ending");
        console.log(this.tempfn);
        console.log(this.$cookies.get("cart"));
        console.log(this.tempfq);
        console.log(this.$cookies.get("quantity"));*/
      },
      add_new_food() {
        this.food.foods.push({
          name: "食物",
          foodtype: "snack",
          price: 0,
          quantity: 1,
          spicy: false,
        });
      },
      deletefood(id) {
        this.food.foods.splice(id, 1);
      },
    },
  };
  </script>