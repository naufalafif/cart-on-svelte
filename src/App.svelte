<script>

  const deepCopy = (object) => {
    let result = JSON.stringify(object)
    return JSON.parse(result)
  }

  const DUMMY_ITEM = [
    {
      id: 1,
      name: 'Sepatu',
      quantity: 1,
      price: 1000
    },
    {
      id: 2,
      name: 'Topi',
      quantity: 2,
      price: 1500
    },
    {
      id: 3,
      name: 'Dompet',
      quantity: 3,
      price: 2000
    },
    
  ]

  let cart = []
  function addCart() {
    const cartId = cart.map(item => item.id)
    const filterredItems = DUMMY_ITEM.filter(item => !cartId.includes(item.id))
    if(filterredItems.length)    
      cart = [...cart,filterredItems[0]] // Harus menggunakan Assingment, karena non assingment like push dll tidak reactive di svelte
    else
      alert('Barang Habis')
  } 

  function removeCartItem(id) {
    cart = cart.filter(item => item.id != id)
  }

  function increaseCartItemQTY(id){
    let currentCart = deepCopy(cart)
    currentCart = currentCart.map(item => {
      if(item.id === id)
        item.quantity++
      return item
    })
    cart = currentCart
  }

  function decreaseCartItemQTY(id){
    let currentCart = deepCopy(cart)
    currentCart = currentCart.map(item => {
      if(item.id === id && item.quantity>0)
        item.quantity--
      return item
    })
    cart = currentCart
  }  

  $: total = cart.reduce((sum, item) => sum + (item.quantity*item.price) ,0)
</script>

<style>
  .cart {
    width: 400px;
    margin: auto;
  }

  .cart-head {
    padding: 10px;
    display: flex;
  }

  .cart-head .info {
    flex:1;
  }

  .cart-head .add-cart {
    flex:1;
  }

  .cart-item {
    display: flex;
    padding: 10px;
  }

  .cart-item .title {
    flex:1;
    font-size: 26px;
    font-weight: bold;
  }
  .cart-item span {
    flex:1;
  }

  .cart-item .cart-item-remove {
    text-align: right;
    cursor: pointer;
    color: #c73f3f;
    padding: 5px;
  }

  .total {
    text-align: right;
  }

</style>

<div class="cart">
  <h1>Simple Cart on SvelteJS</h1>
  <div class="cart-head">
    <div class="info">
    Total Barang : {cart.reduce((sum, item) => sum + item.quantity ,0)}
    </div>
    <button class="add-cart" on:click="{addCart}">Tambah Barang</button>
  </div>
{#if cart.length > 0}  
  {#each cart as item}

     <div class="cart-item">
      <span class="title">{item.name}</span>
      <span >  
        <button on:click="{()=> decreaseCartItemQTY(item.id)}">-</button>
        {item.quantity}
        <button on:click="{()=> increaseCartItemQTY(item.id)}">+</button>  
      </span>
      <span class="cart-item-remove" on:click="{()=> removeCartItem(item.id)}">Hapus</span> 
     </div>
    -- Harga Satuan :  {item.price}
    <br/>
    -- Harga :  {item.price * item.quantity}
      <hr/>
  {/each}
{/if}
  <p class="total">Total : {total}</p>
</div>