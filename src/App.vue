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
      <div v-if="!cameraOpened">
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
      <br>
    </div>
    <div v-show="cameraOpened" id="camera">
      <video id="video" width="400" height="300" autoplay></video>
      <button id="snap" style="width: 100%" @click="verifyIdentityByCamera">Verificar identidade</button>
      <canvas v-show="false" id="canvas" width="400" height="300"></canvas>
    </div>
  </div>
</template>
<script>

export default {
  name: 'app',
  data () {
    return {
      successAudio: new Audio("https://www.soundjay.com/button/sounds/beep-01a.mp3"),
      errorAudio: new Audio("http://download1642.mediafire.com/l42bn486xqhg/2161oud1412l6m6/Windows+XP+Error+Sound+Effect.mp3"),
      isRussian: false,
      cameraOpened: false,
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
      return movements.reduce((sum, movement) => { return sum + movement}, 0) / movements.length
    },
    detectCracker(){
      this.computeAverageCurrentUsersMovements();
      let errorMarginX = 90;
      let errorMarginY = 90;
      let a = this.avgPositionCurrentUser.x < this.avgPositionHonestUser.x + errorMarginX;
      let b = this.avgPositionCurrentUser.x > this.avgPositionHonestUser.x - errorMarginX;
      let c = this.avgPositionCurrentUser.y < this.avgPositionHonestUser.y + errorMarginY;
      let d = this.avgPositionCurrentUser.y > this.avgPositionHonestUser.y - errorMarginY;
      return !(a && b && c && d);
    },
    finish(){
        if(!this.isRussian){
          this.computeAverageHonestUsersMovements();
        }
        let number = prompt("INFORME O NÚMERO DO CARTÃO");
        if(number != 123){
          this.warningIncorrectCart();
        } else if(this.isRussian){
            if(this.detectCracker()){
                this.recuseTransactionAndAskCamera();
                this.openCamera();
            }else{
                this.approveTransaction();
            }
        }else{
            this.approveTransaction();
        }
        this.resetMovements();
    },
    resetMovements(){
      this.mouseMovementsX = [];
      this.mouseMovementsY = [];
    },
    resetHonestUsersMovements(){
      this.avgPositionHonestUser = {x: 0, y: 0};
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
    },
    takeSnapshot(){
      var canvas = document.getElementById('canvas');
      var context = canvas.getContext('2d');
      var video = document.getElementById('video');
      document.getElementById("snap").addEventListener("click", function() {
        context.drawImage(video, 0, 0, 640, 480);
      });
    },
    verifyIdentityByCamera(){
      let audio = "";
      if(!this.isRussian){
        this.successAudio.play();
        this.approveTransaction();
      } else {
        this.errorAudio.play();
        this.recuseTransaction();
      }
    },
    openCamera(){
        this.cameraOpened = true;
        var video = document.getElementById('video');
        if(navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
            navigator.mediaDevices.getUserMedia({ video: true }).then(function(stream) {
                video.src = window.URL.createObjectURL(stream);
                video.play();
            });
        }
    },
    approveTransaction(){
      alert('TRANSAÇÃO APROVADA');
    },
    recuseTransaction(){
      alert('TRANSAÇÃO RECUSADA');
    },
    recuseTransactionAndAskCamera(){
      alert('TRANSAÇÃO RECUSADA, TENTE NOVAMENTE UTILIZANDO A AUTENTICAÇÃO FACIAL');
    },
    warningIncorrectCart(){
      alert('CARTÃO DE CRÉDITO INCORRETO');
    },
  },
  watch:{
    'isRussian'(value, valueBefore){
      if(!(valueBefore === false && value === true)){
        this.resetMovements();
        this.resetHonestUsersMovements();
      }
    }
  },
  created(){
    this.trackUser();
  },
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

#snap{
  font-size: 20px;
  background: darkgray;
  color: white;
  border: none;
  padding: 10px;
}

#camera{
  border: 1px solid red;
  margin: 40px auto;
  width: 400px;
  height: 300px;
  z-index: 2000;
}
</style>
