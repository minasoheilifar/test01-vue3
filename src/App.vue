<script setup>
import { ref,computed,reactive } from "vue";
const header = ref("shopping list app");
const state=reactive({count:0})
//why computed? because of i want characterCount just update when newItem updated
const characterCount=computed(()=>{
  return newItem.value.length
})

const revercedItems=computed(()=>{
  return [...items.value].reverse()
})
const editing = ref(false);
const items = ref([
  { id: 1, label: "eggs", purchased: true , highPriority:false },
  { id: 2, label: "milk", purchased: true , highPriority:false },
  { id: 3, label: "bread", purchased: false , highPriority:true },
]);
const newItem = ref("");
const newItemHighPriority = ref(false);
const saveItem = () => {
  items.value.push({ id: items.value.length + 1, label: newItem.value,highPriority:newItemHighPriority.value });
  newItem.value = ""
  newItemHighPriority.value=""
};
const doEdit = (e) => {
  editing.value = e;
  newItem.value = "";
  newItemHighPriority.value=""

};
const togglePurchased=(item)=>{
  item.purchased=!item.purchased
}
//  <!-- product cart -->
const products=ref([
  {id:1 ,name:"iphone",price:10000},
  {id:2 ,name:"egg",price:100},
  {id:3 ,name:"bag",price:2000},
  {id:4 ,name:"car",price:20000},    
  ])

const total=ref(0)
const add=(product)=>{
  total.value+=product.price
  cartList.value.push(product)
}
const remove = (itemToRemove) => {
  total.value-=itemToRemove.price

  const index = cartList.value.findIndex(item => item.id === itemToRemove.id);
  if (index !== -1) {
    cartList.value.splice(index, 1);
  }
}
const cartList=ref([])


</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button class="btn" v-if="editing" @click="doEdit(false)">cancel</button>
    <button class="btn" v-else @click="doEdit(true)">add item</button>
    <a v-bind:href="newItem">link</a>
  </div>
  <form class="add-item-form" v-if="editing" @submit.prevent="saveItem">
    <input v-model.trim="newItem" type="text" placeholder="Add an item" />
    <br />

    <label>
      <input type="checkbox" v-model="newItemHighPriority" />
      High Priority
    </label>
    <br />
    <button class="btn" v-bind:disabled="newItem.length === 0">
      Save Item
    </button>
  </form>
  <p class="counter">{{ characterCount }}/200</p>
  <ul>
    <li
      v-for="(item, index) in revercedItems"
      :key="item.id"
      class="static-class"
      :class="{strikeout:item.purchased , priority:item.highPriority  }"
      @click="togglePurchased(item)"
    >
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">Nothing to see here...</p>
  <br/>
  <hr/>
  <br/>

  <!-- counter -->
  <div>
    <button class="btn" @click="state.count++">increment</button>
    <h3>{{ state.count }}</h3>
    <button class="btn" @click="state.count--">decrise</button>
  </div>
  <br/>
  <hr/>
  <br/>

  <!-- product cart -->
  <div class="productCart">
    <div>
      <h2>product list</h2>
      <ul>
        <li v-for="product in products" :key="product.id">{{ product.name }} - {{ product.price }} $
          <button class="btn" @click="add(product)">add to cart</button>
        </li>
      </ul>
    </div>
    <div>
      <h2>product cart</h2>
      <ul>
        <li v-for="cartList in cartList" :key="cartList.id" @click="remove(cartList)">{{ cartList.name }} : {{ cartList.price }} $ 
          <button v-if="cartList"  class="btn">remove</button> 
          
        </li>
        <hr/>
        <h3>Total price : {{total}}</h3>
  
      </ul>

    </div>
  </div>
  
</template>


<!-- <style>
  .productCart {
    display:flex;
    justify-content: space-evenly;
  }
</style> -->