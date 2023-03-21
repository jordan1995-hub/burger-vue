<template>
  <q-layout view="lHh Lpr lFf">
    <q-header class="header text-h3 row q-pa-md  " >
      <a href="#" target="_blank" @click.prevent="isShowingCart = false"><Strong>BURGER STATION</Strong></a>
      <div class="text-h6 q-mr-sm col-6" align="right"></div>
      <div class="text-h6 q-mr-xs q-pt-md  ">{{cart.items.length}}<template v-if="cart.items.length == 1">item</template>
        <template  v-else>items</template> </div>
        <div class="text-h6 q-mr-sm q-pt-md "> in cart, </div>
      <div class="text-h6 q-pt-md  q-mr-sm">totalling</div>
      <div class="text-h6 q-pt-md ">{{ cartTotal | currency}}
        <q-btn   class=" " color="#53c5db" glossy label="view cart" @click="isShowingCart = true" />
      </div>

    </q-header>

  <div v-if="!isShowingCart"  class="q-pa-xl  row q-ml-xl q-mt-xl q-gutter-md" >
    <!-- card 1 -->
  <q-card v-for="product in products" :key="product.id" class="my-card " align="center" >
    <q-img  :src="product.image"  style="height: 280px; max-width: 300px"></q-img>

    <q-card-section class="  q-mr-md">
      <div class="text-h6">{{ product.name }}</div>
      <div class="text-subtitle2">{{ product.description}}</div>
        <div class="row q-mt-md ">
        <div class="text-h5 col">{{ product.price | currency }}</div>
        <div class="text-h6  q-mr-sm" :class="{few: product.inStock < 10, none: product.inStock == 0}" >{{ product.inStock}}</div>
        <div class="text-h6" >In Stock</div>
        <q-btn class="btn1 q-ml-md " flat style="background: #53c5db" color="white"  label="Add to cart" @click="addProductToCart(product)" :disable="product.inStock == 0"   />
      </div>
    </q-card-section>
  </q-card>


<!-- card 2 -->

  <q-card v-for="drink in drinks" :key="drink.id" class="my-card q-mt-xl" align="center" >
    <q-img  :src="drink.image"  style="height: 405px; max-width: 320px"></q-img>

    <q-card-section class=" q-mr-md">
      <div class="text-h6">{{ drink.name }}</div>
      <div class="text-subtitle2">{{ drink.description}}</div>
        <div class="row q-mt-md ">
        <div class="text-h5 col">{{ drink.price | currency }}</div>
        <div class="text-h6  q-mr-sm" >{{ drink.inStock}}</div>
        <div class="text-h6" :class="{few: drink.inStock < 10, none: drink.inStock == 0}" >In Stock</div>"
        <q-btn class="btn1 q-ml-md " flat style="background: #53c5db" color="white"  label="Add to cart"  @click="addProductToCart(drink)" :disable="drink.inStock == 0"   />
      </div>
    </q-card-section>
  </q-card>
  </div>
  <div  class="q-pa-xl  q-ml-xl q-mt-xl"   v-else>
    <h2 class="q-mt-xl">CART</h2>
      <table v-if="cart.items.length > 0" class="table-striped">
      <thead>
        <tr>
          <th class="q-ml-xl text-h3">Product</th>
          <th class="q-ml-xl text-h3">Quantity</th>
          <th class="q-ml-xl  text-h3">Price</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="item in cart.items" :key="item.id"  >
          <td class="td1 " >{{ item.product.name  }}</td>
          <td class="td1">
            {{ item.quantity }} &nbsp;
            <q-btn   flat style="background: #53c5db" label="+" @click="increaceQuantity(item)" :disable="item.product.inStock == 0"></q-btn>
            <q-btn class="q-ml-sm" flat style="background: red" label="-" @click="decreaceQuantity(item)" ></q-btn>
          </td>
          <td class="td1">{{ item.quantity * item.product.price | currency }}</td>
        </tr>

        <tr>
          <td class="td2 text-right" colspan="2">
            <strong >Subtotal:</strong>
          </td>

          <td class="td2">{{  cartTotal | currency }}</td>
        </tr>

        <tr>
          <td class="td2 text-right" colspan="2">
            <strong >Taxes:</strong>
          </td>

          <td class="td2">{{ taxAmountTotal | currency }}</td>
        </tr>
        <tr>
          <td class="td2 text-right" colspan="2">
            <strong >Grand Total:</strong>
          </td>

          <td class="td2">{{  cartTotal + taxAmountTotal | currency }}</td>
        </tr>
        <tr class="float-right" colspan="2">
           <q-btn class="q-mt-md float-right"  flat style="background: #53c5db" label="Check out" @click="checkout"></q-btn>

        </tr>


      </tbody>
    </table>

    <p v-else>Your cart is currently empty.</p>
  </div>

    </q-layout>

</template>

<script>
export default {
name: 'PageIndex',
data: function () {
  return {
    isShowingCart: false,
    cart:{
      items:[]
    },
    products: [
      {
        id: 1,
        image: require("../assets/image/burger2.png"),
        name: 'Fluff Screamer Burger',
        description: ' Fluff screamer is a unique American burger \'originating  .',
        price: 195,
        inStock: 30
      },
      {
        id: 2,
        image: require("../assets/image/burger9.png"),
        name: 'Poached Burger',
        description: 'Poached burger is an American burger\' variety originating ',
        price: 99,
        inStock: 33
      },
      {
        id: 3,
        image: require("../assets/image/burger6.png"),
        name: 'Steam Cheese Burger',
        description: 'Steamed cheeseburger or cheeseburg is a   originating .',
        price: 135,
        inStock: 5
      },
      {
        id: 4,
        image: require("../assets/image/burger4.png"),
        name: 'Theata Burger',
        description: ' Theta burger is the name of an  bread bun and a meat patty.',
        price: 195,
        inStock: 0
      }
    ],
    drinks: [
      {
        id: 1,
        name: 'Lemonaide',
        image: require("../assets/image/drinks1.png"),
        description: ' Fluff screamer is a unique American burger \'originating  .',
        price: 95,
        inStock: 23
      },
      {
        id: 2,
        name: 'Ice Tea',
        image: require("../assets/image/drinks7.png"),
        description: 'Poached burger is an American burger\' variety originating ',
        price: 45,
        inStock: 26
      },
      {
        id: 3,
        name: 'Coke',
        image: require("../assets/image/drinks5.png"),
        description: 'Steamed cheeseburger or cheeseburg is a   originating .',
        price: 145,
        inStock: 55
      },
      {
        id: 4,
        name: 'Mango soda',
        image: require("../assets/image/drinks6.png"),
        description: ' Theta burger is the name of an  bread bun and a meat patty.',
        price: 25,
        inStock:0
      }

    ]
  }
},

methods: {
  addProductToCart: function( product,drink){

    var cartItem = this.getCartItem(product);

    if (cartItem != null){
        cartItem.quantity++;
    } else{

     this.cart.items.push({
      product:product,
      drink:drink,
     quantity: 1
  });

    }

  product.inStock--;
  },
  getCartItem: function(product) {
   for (var  i = 0; i < this.cart.items.length; i++){
    if (this.cart.items[i].product.id === product.id){
      return this.cart.items[1];
    }
   }
   return  null;
  },
  increaceQuantity: function(cartItem){
    cartItem.product.inStock--;
    cartItem.quantity++;
  },
  decreaceQuantity: function(cartItem){
    cartItem.quantity--;
    cartItem.product.inStock++;
    if( cartItem.quantity == 0){
      this.removeItemFromCart(cartItem);
    }
  },
  removeItemFromCart: function(cartItem){
    var index = this.cart.items.indexOf(cartItem);
    if (index !== -1){
      this.cart.items.splice(index,1);
    }
  },
  checkout: function(){
    if(confirm('Are you sure that you want to pruchase these products?')){
      this.cart.items.forEach(function(item){
        item.product.inStock += item.quantity;
      });
      this.cart.items = [];
    }
  }

},

computed:{
  cartTotal: function(){
    var total = 0;
   this.cart.items.forEach(function(item){
      total += item.quantity * item.product.price;
    });
    return total;
  },
  taxAmountTotal: function(){
    return ((this.cartTotal * 10) /100)
  },
},
filters:{
    currency: function(value){
      var formatter = Intl.NumberFormat('en-US',{
        style: 'currency',
        currency: 'USD',
        minumumFractionDigits: 0
      });
     return formatter.format(value);
    }
  }
}
</script>

