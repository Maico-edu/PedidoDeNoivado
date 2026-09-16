
<script setup>
import { ref } from 'vue'

const respondeu = ref(false)

const posicaoNao = ref({
  top: '50%',
  left: '50%'
})

const mensagens = [
  'Tem certeza? 👀',
  'Pensa melhor... 😏',
  'Hmmmm... 🤨',
  'Você tem certeza mesmo?',
  'Olha o outro botão ali...',
  'Vou dar mais uma chance!'
]

const mensagemNao = ref(mensagens[0])

function moverNao() {
  const largura = window.innerWidth
  const altura = window.innerHeight

  const x = Math.random() * (largura - 160) + 80
  const y = Math.random() * (altura - 160) + 80

  posicaoNao.value = {
    left: `${x}px`,
    top: `${y}px`
  }

  mensagemNao.value =
    mensagens[Math.floor(Math.random() * mensagens.length)]
}

function aceitar() {
  respondeu.value = true
}
</script>

<template>
  <main class="pagina">

    <!-- CORAÇÕES DE FUNDO -->
    <div class="coracoes">
      <span>♥</span>
      <span>♡</span>
      <span>♥</span>
      <span>♡</span>
      <span>♥</span>
      <span>♡</span>
      <span>♥</span>
      <span>♡</span>
      <span>♥</span>
      <span>♡</span>
      <span>♥</span>
      <span>♡</span>
    </div>

    <!-- TELA PRINCIPAL -->
    <section v-if="!respondeu" class="cartao">

      <div class="coracao-principal">
        ❤️
      </div>

      <p class="pequeno">
        Tenho uma perguntinha...
      </p>

      <h1 class="pergunta">
        vanderlind, vc me daria
        um beijinho, meu anjinho?
      </h1>

      <p class="subtitulo">
        pergunta importante
      </p>

      <div class="botoes">

        <button
          class="botao-sim"
          @click="aceitar"
        >
          Sim ❤️
        </button>

        <button
          class="botao-nao"
          :style="{
            left: posicaoNao.left,
            top: posicaoNao.top
          }"
          @mouseenter="moverNao"
          @touchstart.prevent="moverNao"
          @click="moverNao"
        >
          Não 😳
        </button>

      </div>

      <p class="mensagem-nao">
        {{ mensagemNao }}
      </p>

    </section>

    <!-- TELA DEPOIS DO SIM -->
    <section v-else class="cartao final">

      <div class="explosao">
        💖
      </div>

      <h1>
        EU SABIAAA QUE VC QUERIA ME BEIJAR! SEUS OLHINHOS NUNCA ME ENGANAM! 😏
      </h1>

      <p class="texto-final">
        nunca me enganou
      </p>

      <div class="coracoes-finais">
        ❤️ 💕 ❤️ 💕 ❤️
      </div>

      <p class="finalzinho">
        Agora não pode voltar atrás hein...
      </p>

    </section>

  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body,
#app {
  width: 100%;
  max-width: 100%;
  min-height: 100%;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  overflow: hidden;
}

/* FUNDO */

.pagina {
  width: 100%;
  max-width: 100vw;
  height: 100vh;
  min-height: 600px;

  display: flex;
  justify-content: center;
  align-items: center;

  position: relative;
  overflow: hidden;

  background:
    radial-gradient(circle at 20% 20%, #ffb3c1 0%, transparent 25%),
    radial-gradient(circle at 80% 80%, #ffccd5 0%, transparent 25%),
    linear-gradient(135deg, #fff5f7, #ffffff, #ffe4e9);
}

/* CARTÃO */

.cartao {
  width: min(90%, 650px);
  min-height: 500px;

  padding: 55px 35px;

  background: rgba(255, 255, 255, 0.88);

  border: 2px solid rgba(220, 20, 60, 0.15);

  border-radius: 30px;

  box-shadow:
    0 20px 60px rgba(150, 0, 30, 0.18),
    0 0 0 10px rgba(255, 255, 255, 0.3);

  backdrop-filter: blur(10px);

  text-align: center;

  position: relative;
  z-index: 5;

  animation: aparecer 0.8s ease;
}

/* CORAÇÃO PRINCIPAL */

.coracao-principal {
  font-size: 75px;

  animation:
    bater 1.2s infinite,
    flutuar 3s ease-in-out infinite;

  margin-bottom: 15px;
}

.pequeno {
  color: #c1123f;

  font-size: 18px;

  font-weight: bold;

  margin-bottom: 15px;
}

/* PERGUNTA */

.pergunta {
  color: #a8002b;

  font-size: clamp(30px, 4.5vw, 45px);

  line-height: 1.2;

  margin-bottom: 20px;
}

.subtitulo {
  color: #666;

  font-size: 17px;

  margin-bottom: 45px;
}

/* BOTÕES */

.botoes {
  height: 100px;

  position: relative;

  display: flex;

  justify-content: center;

  align-items: center;

  gap: 25px;
}

button {
  border: none;

  padding: 16px 40px;

  border-radius: 50px;

  font-size: 19px;

  font-weight: bold;

  cursor: pointer;

  transition: 0.25s ease;

  box-shadow:
    0 8px 20px rgba(120, 0, 30, 0.15);
}

/* SIM */

.botao-sim {
  background: #c1123f;

  color: white;

  position: relative;

  z-index: 3;
}

.botao-sim:hover {
  transform: scale(1.08);

  background: #a8002b;

  box-shadow:
    0 10px 30px rgba(193, 18, 63, 0.35);
}

/* NÃO */

.botao-nao {
  background: white;

  color: #c1123f;

  border: 2px solid #c1123f;

  position: fixed;

  z-index: 20;

  transform: translate(-50%, -50%);

  transition:
    left 0.35s ease,
    top 0.35s ease;
}

.botao-nao:hover {
  transform: translate(-50%, -50%) scale(1.05);
}

.mensagem-nao {
  color: #999;

  font-size: 14px;

  margin-top: 15px;

  min-height: 20px;
}

/* CORAÇÕES DE FUNDO */

.coracoes span {
  position: absolute;

  color: #e6395f;

  font-size: 25px;

  opacity: 0.25;

  animation: subir 8s linear infinite;
}

.coracoes span:nth-child(1) {
  left: 5%;
  animation-delay: 0s;
}

.coracoes span:nth-child(2) {
  left: 15%;
  animation-delay: 2s;
}

.coracoes span:nth-child(3) {
  left: 25%;
  animation-delay: 4s;
}

.coracoes span:nth-child(4) {
  left: 35%;
  animation-delay: 1s;
}

.coracoes span:nth-child(5) {
  left: 45%;
  animation-delay: 5s;
}

.coracoes span:nth-child(6) {
  left: 55%;
  animation-delay: 3s;
}

.coracoes span:nth-child(7) {
  left: 65%;
  animation-delay: 6s;
}

.coracoes span:nth-child(8) {
  left: 75%;
  animation-delay: 2s;
}

.coracoes span:nth-child(9) {
  left: 85%;
  animation-delay: 4s;
}

.coracoes span:nth-child(10) {
  left: 95%;
  animation-delay: 1s;
}

.coracoes span:nth-child(11) {
  left: 30%;
  animation-delay: 7s;
}

.coracoes span:nth-child(12) {
  left: 70%;
  animation-delay: 5s;
}

/* TELA FINAL */

.final {
  min-height: 450px;

  display: flex;

  flex-direction: column;

  justify-content: center;

  align-items: center;
}

.explosao {
  font-size: 90px;

  animation:
    aparecerCoracao 0.8s ease,
    bater 1s infinite;

  margin-bottom: 15px;
}

.final h1 {
  font-size: clamp(35px, 5vw, 55px);
}

.texto-final {
  color: #555;

  font-size: 24px;

  font-weight: bold;

  margin-top: 10px;
}

.coracoes-finais {
  font-size: 30px;

  margin-top: 35px;

  animation:
    flutuar 2s infinite ease-in-out;
}

.finalzinho {
  color: #999;

  margin-top: 30px;
}

/* ANIMAÇÕES */

@keyframes aparecer {
  from {
    opacity: 0;
    transform: translateY(30px) scale(0.95);
  }

  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

@keyframes aparecerCoracao {
  0% {
    transform: scale(0);
  }

  70% {
    transform: scale(1.3);
  }

  100% {
    transform: scale(1);
  }
}

@keyframes bater {
  0%,
  100% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.15);
  }
}

@keyframes flutuar {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-10px);
  }
}

@keyframes subir {
  from {
    transform: translateY(110vh) rotate(0deg);

    opacity: 0;
  }

  20% {
    opacity: 0.3;
  }

  80% {
    opacity: 0.3;
  }

  to {
    transform: translateY(-120px) rotate(360deg);

    opacity: 0;
  }
}

/* CELULAR */

@media (max-width: 600px) {

  .pagina {
    width: 100%;
    max-width: 100vw;

    min-height: 100vh;

    padding: 15px;
  }

  .cartao {
    width: 94%;

    min-height: 480px;

    padding: 40px 20px;

    border-radius: 25px;
  }

  .coracao-principal {
    font-size: 55px;
  }

  .pequeno {
    font-size: 16px;
  }

  .pergunta {
    font-size: 30px;

    line-height: 1.2;
  }

  .subtitulo {
    font-size: 15px;

    margin-bottom: 30px;
  }

  button {
    padding: 14px 30px;

    font-size: 17px;
  }

  .texto-final {
    font-size: 20px;
  }

  .finalzinho {
    font-size: 14px;
  }
}
</style>
