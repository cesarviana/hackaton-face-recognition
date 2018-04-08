<template>
  <div id="app">
    <header>
      <h1>
        América Compras
      </h1>
    </header>
    <div id="cart">
      <h1>Carrinho de compras</h1>
      <table>
        <thead>
          <tr>
            <th>Cod</th>
            <th colspan="2">Produto</th>
            <th>Quantidade</th>
            <th>Valor</th>
            <th>Total</th>
            <th>Ação</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in items" :key="item.id">
            <td> {{item.id}} </td>
            <td>
              <img :src="item.img" :alt="item.description">
            </td>
            <td> 
              {{item.description}} 
            </td>
            <td> {{item.quantity}} </td>
            <td> {{item.price}} </td>
            <td> {{item.price.split(' ')[1] * item.quantity}} </td>
            <td><a href="#">Excluir</a></td>
          </tr>
        </tbody>
      </table>
      <div class="cart-bottom">
        <span>
          <input type="radio" name="method"> Boleto <br>
          <input type="radio" name="method" checked> <b>Cartão de crédito</b> <br>
          <input type="radio" name="method"> PayPal <br>
        </span>
        <span>
          <button id="finishButton" @click="showModalVerification()">Finalizar compra</button>
        </span>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'app',
  data () {
    return {
      items: [
        {
          id: '1',
          img: 'https://encrypted-tbn0.gstatic.com/shopping?q=tbn:ANd9GcQC3Ds8WpC5cowHUtUvNX4noqtykIYdtbZYs-GdIX9kX832tcc&usqp=CAc',
          description: 'Celular Meizu 510',
          quantity: 2,
          price: 'R$ 800'
        }
      ],
    }
  },
  methods: {
    showModalVerification(){
      alert('show modal');
    }
  }
}
</script>
<style lang="scss">
header{
  background: #e60014;
  color: white;
  padding: 2px 10px;
  width: 100%;
}
body {
  padding: 0 !important;
  margin: 0 !important;
  font-family: Arial;
}
h1{
  padding: 0;
}
td, th {
  padding: 10px;
  border: 1px solid lightgray;
  cellspacing: 0;
}
#cart {
  width: 50%;
  margin: 0 auto;
  display: flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
}

#cart .cart-bottom{
  width: 100%;
  display: flex;
  flex-flow: row no-wrap;
  justify-content: space-between;
  padding: 10px;
}

table {
  border-spacing: 0;
}

#finishButton{
  cursor: pointer;
  background: red;
  color: white;
  padding: 10px;
  border: none;
  font-size: 20px;
  border-radius: 2px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
