<template>
  <div id="app">
    <header>
      <h1>
        America
      </h1>
    </header>
    <div id="cart">
      <h1>Carrinho de compras</h1>
      <table>
        <thead>
          <tr>
            <th>Cod</th>
            <th>Produto</th>
            <th>Quantidade</th>
            <th>Valor</th>
            <th>Total</th>
            <th>Ação</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in items" :key="item.id">
            <td> {{item.id}} </td>
            <td> {{item.description}} </td>
            <td> {{item.quantity}} </td>
            <td> {{item.price}} </td>
            <td> {{item.price.split(' ')[1] * item.quantity}} </td>
            <td><a href="#">Excluir</a></td>
          </tr>
        </tbody>
      </table>
      <div>
        <button id="finishButton" @click="showModalVerification()">Finalizar compra</button>
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
  background: red;
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
  display: flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
}

table {
  border-spacing: 0;
}

#finishButton{
  cursor: pointer;
  margin: 30px;
  background: green;
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
