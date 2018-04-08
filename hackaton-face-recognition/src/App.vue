<template>
  <div id="app">
    <header style="display: flex; justify-content: space-around; align-items: center;">
      <h1>
        América Compras
      </h1>
      <span>
        Bem-vindo,
        <b id="login" @click="isRussian = !isRussian">
          {{!isRussian ? 'João Honesto': 'Hacker Russo'}}
        </b>
      </span>
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
          <button id="finishButton" @click="finish()">Finalizar compra</button>
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
      isRussian: false,
      mouseMovementsX: [],
      mouseMovementsY: [],
      avgPositionHonestUser: {x: 0, y: 0},
      avgPositionCurrentUser: {x: 0, y: 0},
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
    trackUser(){
      document.addEventListener('mousemove', this.trackMouseMove);
    },
    trackMouseMove(event){
      this.mouseMovementsX.push(event.clientX);
      this.mouseMovementsY.push(event.clientY);
    },
    avgMovement(movements) {
      return movements.reduce((sum, movement) =>  { return sum + movement}, 0) / movements.length
    },
    detectCracker(){
      this.computeAverageCurrentUsersMovements();
      let errorMarginX = 500;
      let errorMarginY = 300;
      let isXCrackerMovements = ((this.avgPositionCurrentUser.x + (errorMarginX/2)) 
        - this.avgPositionHonestUser.x) > errorMarginX;
      let isYCrackerMovements = ((this.avgPositionCurrentUser.y + (errorMarginY/2)) 
        - this.avgPositionHonestUser.y) > errorMarginY;
      this.resetMovements();
      return isXCrackerMovements || isYCrackerMovements;
    },
    finish(){
      
        let number = prompt("INFORME O NÚMERO DO CARTÃO");
        if(number != 123){
          alert('CARTÃO INCORRETO')
        } else if(this.isRussian){
            if(this.detectCracker()){
              setTimeout(()=>{
                alert('TRANSAÇÃO RECUSADA, TENTE NOVAMENTE UTILIZANDO A AUTENTICAÇÃO FACIAL');
              },1000);
            }
        }else{
          setTimeout(()=>{
            alert('TRANSAÇÃO APROVADA')
          },1000);
        }
        this.resetMovements();
    },
    resetMovements(){
      this.mouseMovementsX = [];
      this.mouseMovementsY = [];
    },
    computeAverageHonestUsersMovements(){
      this.avgPositionHonestUser.x = this.avgMovement(this.mouseMovementsX);
      this.avgPositionHonestUser.y = this.avgMovement(this.mouseMovementsY);
      this.resetMovements();
    },
    computeAverageCurrentUsersMovements(){
      this.avgPositionCurrentUser.x = this.avgMovement(this.mouseMovementsX);
      this.avgPositionCurrentUser.y = this.avgMovement(this.mouseMovementsY);
      this.resetMovements();
    }
  },
  watch:{
    'isRussian'(value, valueBefore){
      if(valueBefore === false && value === true){
        this.computeAverageHonestUsersMovements();
      }else{
        this.resetMovements();
      }
    }
  },
  created(){
    this.trackUser();
  }
}
</script>
<style lang="scss">
header{
  h1 {
    border-top: 3px solid white;
    border-bottom: 3px solid white;
  }
  #login{
    text-transform: uppercase;
    font-size: 20px;
  }
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
