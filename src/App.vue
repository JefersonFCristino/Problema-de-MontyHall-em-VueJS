<template>
  <div id="app">
    <h1>Problema de Monty Hall</h1>

    <p>Você pode testar alguém escolhendo a porta premiada sem deixar a outra pessoa ver, gerar aleatoriamente ou pedir para outra pessoa testar você (já que essa pessoa irá saber exatamente a porta certa sem o risco de abrir logo de início). Caso não saiba do que se trata este problema, logo abaixo deste formulário temos uma explicação :)</p>

    <p><strong>Instruções:</strong> <br> Selecione clicando na porta (serve apenas para marcar a porta escolhida). Para abrir, clique na maçaneta</p>

    <div class="form">
      <div v-if="!started">
        <label for="portAmount">Quantas portas?</label>
        <input type="text" id="portAmount" size="2" v-model.number="portAmount" />
        <!-- v-model.number está monitorando e salvando como um búmero e não uma string -->
      </div>

      <div v-if="!started">
        <label for="selectedPort">Escolher porta premiada</label>
        <input
          v-show="!random"
          type="text"
          id="selectedPort"
          size="2"
          v-model.number="selectedPort"
        />

        <input v-if="random" type="text" id="selectedPort" size="2" value="?" disabled />

        <button v-if="random" @click="random = false">Escolher</button>
      </div>

      <div v-if="!started">
        <label for="selectedPort">Gerar porta premiada aleatoriamente</label>
        <button id="selectedPort" @click="randomlyGenerate">?</button>
        <!-- <input v-if="true" type="text" id="selectedPort" size="2" value="?"> -->
      </div>

      <button v-if="!started" @click="started = true">Iniciar</button>
      <button v-if="started" @click="started = false">Reiniciar</button>
    </div>

    <div class="doors" v-if="started">
      <div v-for="i in portAmount" :key="i">
        <Door :hasGift="i === selectedPort" :number="i" />
      </div>
    </div>

    <div v-if="!started" class="area-explanation">
      <h1>Explicação</h1>
      <h3>O Problema onde escolher o errado sai mais vantajoso...</h3>

      <p>O problema inspirado em um antigo programa de TV consiste em 3 portas, duas vazias ou com algo não desejado e uma com o prêmio. Você escolhe uma, o apresentador por sua vez sabe em qual porta está o prêmio e abre uma porta vazia restando apenas 2, você pode escolher ficar com a porta escolhida no início ou trocar pela outra. No final, saberemos o resultado</p>

      <p>O Objetivo do problema é mostrar que trocar a porta sempre será mais vantajoso, ainda existe uma possibilidade de perder porém as chances de ganhar aumentam.</p>

      <p>Como existem 3 portas e uma delas tem o prêmio, temos 33,33..% de escolher a certa e 66,66..% de escolher a errada. Mas caso troquemos de porta essa possibilidade se inverte, ficando 66% de chance de acertar.</p>

      <h3>Existem três casos:</h3>

      <h4>Considerando as portas 1, 2 e 3, vamos supor que a porta 1 seja a premiada</h4>

      <p>
        Caso 1 - Você escolhe a porta 1: o apresentador elimina a porta 3 e sobra o prêmio(escolhido) e a porta 2.
        <br />
        <strong>Se trocar perde. Resultado: Perde</strong>
      </p>

      <p>
        Caso 2 - Você escolhe a porta 2: o apresentador elimina a porta 3 e sobra a porta 1(premiada) e a porta 2.
        <br />
        <strong>Se trocar ganha. Resultado: Ganha</strong>
      </p>

      <p>
        Caso 3 - Você escolhe a porta 3: o apresentador elimina a porta 2 e sobra a porta 1(premiada) e a porta 3.
        <br />
        <strong>Se trocar ganha. Resultado: Ganha</strong>
      </p>

      <p>Então podemos ver que sempre será mais vantajoso trocar de porta. Como já dito acima, ainda existe a chance de errar porém suas chances de acertar aumentam, dobram por sinal. Por isso é mais vantajoso escolher a errada de primeira, mas se escolhemos a premiada ou a errada, isso não temos como saber....</p>

      <h1>Vídeo</h1>

      <h3>Caso ainda tenha alguma dúvida, talvez esse vídeo possa te ajudar</h3>

      <iframe
        class="video-explanation"
        src="https://www.youtube.com/embed/Hh7pDPnKK-4"
        frameborder="0"
        allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
      ></iframe>
    </div>
  </div>
</template>

<script>
import Door from "./components/Door";

export default {
  name: "App",
  components: { Door },
  data: function() {
    return {
      started: false,
      portAmount: 3,
      selectedPort: null,
      random: false
    };
  },
  methods: {
    randomlyGenerate() {
      const randomDoor = Math.floor(
        Math.random() * (this.portAmount - 1 + 1) + 1
      );

      this.selectedPort = randomDoor;
      this.random = true;
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  font-family: "Montserrat", sans-serif;
}

body {
  color: #fff;
  background: linear-gradient(to right, rgb(21, 153, 87), rgb(21, 87, 153));
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#app h1 {
  border: 1px solid #000;
  background-color: #0004;
  padding: 20px;
  margin-bottom: 60px;
  text-align: center;
}

#app > p {
  text-align: justify;
  text-align-last: center;
  margin: 0 10% 60px 10%;
}

.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 80px;
  font-size: 1.6rem;
  padding: 20px;
}

.form div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.form div label {
  margin-bottom: 30px;
}

.form input,
.form button {
  margin-bottom: 25px;
  font-size: 1.7rem;
  text-align: center;
}

.doors {
  align-self: stretch;
  display: flex;
  justify-content: space-around;

  flex-wrap: wrap;
}

/************************** */

.area-explanation {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 0 10% 0 10%;
  width: 100%;
  text-align: justify;
  text-align-last: center;
}

.area-explanation p,
h1,
h3,
h4 {
  margin-bottom: 50px;
}

@media (min-width: 0) {

  #app > p {
    font-size: 1.05em;
  }

  .area-explanation {
    font-size: 1em;
  }

  .video-explanation {
    width: 100%;
    height: 300px;
    margin: 50px 0 150px 0;
  }
}

@media (min-width: 768px) {

  #app > p {
    font-size: 1.350em;
  }

  .area-explanation {
    font-size: 1.3em;
  }

  .video-explanation {
    height: 450px;
  }
}

@media (min-width: 1200px) {
  .video-explanation {
    width: 90%;
    height: 600px;
  }
}
</style>