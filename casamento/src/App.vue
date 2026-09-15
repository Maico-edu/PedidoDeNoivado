<template>
  <div id="app">

    <div class="stars">
      <div class="star" v-for="s in stars" :key="s.id"
        :style="{ top:s.top+'%', left:s.left+'%', width:s.size+'px', height:s.size+'px', animationDuration:s.duration+'s', animationDelay:s.delay+'s' }">
      </div>
      <div class="planet p1"></div>
      <div class="planet p2"></div>
      <div class="planet p3"></div>
      <div class="planet p4"></div>
    </div>

    <!-- HERO -->
    <section>
      <p class="date reveal" v-reveal>{{ formattedStartDate }}</p>
      <h1 class="reveal" v-reveal>{{ nomeA }} &amp; {{ nomeB }}</h1>
      <p class="quote reveal" v-reveal>{{ mensagemAbertura }}</p>
      <span class="scroll-hint">role para ver nossa história ↓</span>
    </section>

    <!-- CAPITULOS -->
    <section v-for="cap in capitulos" :key="cap.titulo">
      <p class="eyebrow reveal" v-reveal>nossa história passou pelo</p>
      <div class="season-icon reveal" v-reveal>{{ cap.icone }}</div>
      <p class="season-name reveal" v-reveal>{{ cap.titulo }}</p>
      <p class="quote reveal" v-reveal>{{ cap.texto }}</p>
    </section>

    <!-- FOTO -->
    <section>
      <div class="frame reveal" v-reveal>
        <img v-if="fotoUrl" :src="fotoUrl" alt="Foto do casal">
        <span v-else>coloque o link ou caminho<br>da foto em "fotoUrl"</span>
      </div>
      <h2 class="reveal" v-reveal>{{ legendaFoto }}</h2>
      <p class="quote reveal" v-reveal>{{ textoFoto }}</p>
    </section>

    <!-- CONTADOR -->
    <section>
      <h1 class="reveal" v-reveal style="font-size:clamp(2rem,6vw,3rem)">Nossa história continua...</h1>
      <p class="quote reveal" v-reveal>{{ mensagemFinal }}</p>

      <p class="counter-label reveal" v-reveal>juntos há</p>
      <div class="counter reveal" v-reveal>
        <div class="unit">
          <span class="value">{{ tempoJuntos.dias }}</span>
          <span class="unit-label">dias</span>
        </div>
        <div class="unit">
          <span class="value">{{ tempoJuntos.horas }}</span>
          <span class="unit-label">horas</span>
        </div>
        <div class="unit">
          <span class="value">{{ tempoJuntos.minutos }}</span>
          <span class="unit-label">min</span>
        </div>
        <div class="unit">
          <span class="value">{{ tempoJuntos.segundos }}</span>
          <span class="unit-label">seg</span>
        </div>
      </div>

      <button class="heart-btn" :class="{ beat: bateu }" @click="bater">
        te amo ❤️
      </button>
    </section>

    <footer>feito com Vue.js, para {{ nomeB }} · edite os dados no topo do script</footer>

  </div>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue';

// ======================================================
//  EDITE AQUI OS DADOS DA SUA HISTÓRIA
// ======================================================
const nomeA = 'Ana';
const nomeB = 'Pedro';
const dataInicio = '2018-06-17T00:00:00'; // data em que a história começou
const mensagemAbertura = 'Uma pequena linha do tempo do nosso amor, para você rolar sempre que sentir saudade.';

const capitulos = [
  {
    icone: '🍂',
    titulo: 'outono',
    texto: 'Entre folhas douradas e tardes amenas, nossa história começou a florescer.'
  },
  {
    icone: '❄️',
    titulo: 'inverno',
    texto: 'Nos aquecemos um ao outro nas noites mais frias, e percebemos que era pra sempre.'
  },
  {
    icone: '🌸',
    titulo: 'primavera',
    texto: 'Tudo desabrochou: nós, os planos e a certeza de que estávamos no caminho certo.'
  }
];

const fotoUrl = ''; // cole aqui o link (ou caminho) de uma foto de vocês
const legendaFoto = 'Nossas primeiras fotos 📸';
const textoFoto = 'Um dos nossos primeiros cliques juntos. Como o tempo voa quando estou ao seu lado.';

const mensagemFinal = 'Cada dia é uma nova oportunidade de escrever mais um capítulo juntos.';
// ======================================================


const formattedStartDate = new Date(dataInicio).toLocaleDateString('pt-BR', {
  weekday: 'long', day: 'numeric', month: 'long', year: 'numeric'
});

// campo de estrelas
const stars = [];
for (let i = 0; i < 90; i++) {
  stars.push({
    id: i,
    top: Math.random() * 100,
    left: Math.random() * 100,
    size: (Math.random() * 2 + 1).toFixed(1),
    duration: (Math.random() * 3 + 2).toFixed(1),
    delay: (Math.random() * 4).toFixed(1)
  });
}

// contador ao vivo
const now = ref(new Date());
let intervalId;
onMounted(() => {
  intervalId = setInterval(() => { now.value = new Date(); }, 1000);
});
onBeforeUnmount(() => clearInterval(intervalId));

const tempoJuntos = computed(() => {
  const diffMs = now.value - new Date(dataInicio);
  const segTotal = Math.max(0, Math.floor(diffMs / 1000));
  const dias = Math.floor(segTotal / 86400);
  const horas = Math.floor((segTotal % 86400) / 3600);
  const minutos = Math.floor((segTotal % 3600) / 60);
  const segundos = segTotal % 60;
  return { dias, horas, minutos, segundos };
});

// botão "te amo"
const bateu = ref(false);
function bater() {
  bateu.value = true;
  setTimeout(() => bateu.value = false, 500);
}

// diretiva local v-reveal (Vue 3.3+: prefixo vNome vira diretiva "v-nome" no template)
const vReveal = {
  mounted(el) {
    const obs = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          el.classList.add('is-visible');
          obs.unobserve(el);
        }
      });
    }, { threshold: .3 });
    obs.observe(el);
  }
};
</script>

<style scoped>
:root{
  --night-1:#0a0c16;
  --night-2:#141830;
  --gold:#c9a66b;
  --rose:#d98fa0;
  --cream:#f2efe8;
  --muted:#8b90ac;
}

*{ box-sizing:border-box; }

#app{
  height:100vh;
  overflow-y:scroll;
  scroll-snap-type:y proximity;
  position:relative;
  background:var(--night-1);
  color:var(--cream);
  font-family:'Manrope', sans-serif;
  scroll-behavior:smooth;
}

/* ---------- campo de estrelas ---------- */
.stars{
  position:fixed;
  inset:0;
  overflow:hidden;
  pointer-events:none;
  z-index:0;
  background:radial-gradient(ellipse at 50% 0%, var(--night-2) 0%, var(--night-1) 70%);
}
.star{
  position:absolute;
  background:var(--cream);
  border-radius:50%;
  opacity:.15;
  animation:twinkle linear infinite;
}
@keyframes twinkle{
  0%, 100%{ opacity:.15; }
  50%{ opacity:.9; }
}

/* ---------- planetas ---------- */
.planet{
  position:absolute;
  border-radius:50%;
  animation:drift ease-in-out infinite;
  filter:blur(.3px);
}
.planet.p1{
  width:120px; height:120px;
  top:8%; left:-40px;
  background:radial-gradient(circle at 32% 30%, #e8c99a, #c9a66b 55%, #8a6d3f 100%);
  box-shadow:0 0 50px rgba(201,166,107,.25);
  animation-duration:26s;
}
.planet.p2{
  width:70px; height:70px;
  top:62%; right:-20px;
  background:radial-gradient(circle at 35% 30%, #f0b8c4, #d98fa0 55%, #8a4a58 100%);
  box-shadow:0 0 34px rgba(217,143,160,.22);
  animation-duration:19s;
  animation-delay:-4s;
}
.planet.p3{
  width:44px; height:44px;
  top:30%; right:12%;
  background:radial-gradient(circle at 35% 30%, #b9c3e8, #6c7bb8 55%, #2f3868 100%);
  box-shadow:0 0 24px rgba(108,123,184,.25);
  animation-duration:15s;
  animation-delay:-8s;
}
.planet.p3::after{
  content:'';
  position:absolute;
  inset:-10px -2px;
  border:1px solid rgba(185,195,232,.3);
  border-radius:50%;
  transform:rotate(-18deg);
}
.planet.p4{
  width:26px; height:26px;
  bottom:6%; left:14%;
  background:radial-gradient(circle at 35% 30%, #cfd6ea, #9aa4c8 60%, #565f8a 100%);
  box-shadow:0 0 16px rgba(154,164,200,.2);
  animation-duration:12s;
  animation-delay:-2s;
}
@keyframes drift{
  0%, 100%{ transform:translate(0,0); }
  50%{ transform:translate(14px,-18px); }
}
@media (prefers-reduced-motion: reduce){
  .planet, .star{ animation:none; }
}

/* ---------- secoes ---------- */
section{
  position:relative;
  z-index:1;
  min-height:100vh;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
  text-align:center;
  padding:8vh 7vw;
  scroll-snap-align:start;
}

.reveal{
  opacity:0;
  transform:translateY(18px);
  transition:opacity 1s ease, transform 1s ease;
}
.reveal.is-visible{
  opacity:1;
  transform:translateY(0);
}

.eyebrow{
  font-size:.8rem;
  letter-spacing:.14em;
  color:var(--muted);
  margin:0 0 1.2rem;
  text-transform:lowercase;
}

.date{
  font-family:'Cormorant Garamond', serif;
  font-style:italic;
  font-weight:500;
  font-size:clamp(1.6rem, 5vw, 2.4rem);
  color:var(--gold);
  margin:0 0 .6rem;
}

h1{
  font-family:'Cormorant Garamond', serif;
  font-weight:600;
  font-size:clamp(2.4rem, 8vw, 4rem);
  line-height:1.15;
  margin:0 0 1.4rem;
  max-width:14ch;
}

.season-icon{
  font-size:clamp(2.2rem, 7vw, 3rem);
  margin-bottom:.8rem;
  filter:drop-shadow(0 0 18px rgba(201,166,107,.35));
}

.season-name{
  font-family:'Cormorant Garamond', serif;
  font-weight:600;
  font-size:clamp(2.6rem, 9vw, 4.2rem);
  margin:0 0 1.6rem;
  color:var(--cream);
}

p.quote{
  font-size:clamp(1rem, 2.6vw, 1.2rem);
  line-height:1.7;
  color:var(--muted);
  max-width:34ch;
  margin:0;
}

.scroll-hint{
  position:absolute;
  bottom:6vh;
  font-size:.75rem;
  color:var(--muted);
  letter-spacing:.08em;
  animation:bob 2.4s ease-in-out infinite;
}
@keyframes bob{
  0%,100%{ transform:translateY(0); opacity:.5; }
  50%{ transform:translateY(8px); opacity:1; }
}

/* ---------- foto ---------- */
.frame{
  width:min(78vw, 320px);
  aspect-ratio:4/5;
  background:linear-gradient(160deg, var(--night-2), #1b1f3a);
  border:1px solid rgba(242,239,232,.12);
  border-radius:6px;
  display:flex;
  align-items:center;
  justify-content:center;
  margin-bottom:2rem;
  transform:rotate(-2deg);
  box-shadow:0 20px 60px rgba(0,0,0,.5);
  overflow:hidden;
}
.frame img{
  width:100%;
  height:100%;
  object-fit:cover;
}
.frame span{
  color:var(--muted);
  font-size:.85rem;
  padding:1rem;
}

h2{
  font-family:'Cormorant Garamond', serif;
  font-style:italic;
  font-weight:500;
  font-size:clamp(1.6rem, 5vw, 2.2rem);
  margin:0 0 .8rem;
}

/* ---------- contador final ---------- */
.counter-label{
  font-size:.8rem;
  letter-spacing:.1em;
  color:var(--muted);
  margin:2.6rem 0 1rem;
}
.counter{
  display:flex;
  gap:clamp(.8rem, 4vw, 2rem);
}
.counter .unit{
  display:flex;
  flex-direction:column;
  align-items:center;
  min-width:3.4rem;
}
.counter .value{
  font-family:'Cormorant Garamond', serif;
  font-weight:600;
  font-size:clamp(1.8rem, 6vw, 2.6rem);
  color:var(--gold);
  font-variant-numeric:tabular-nums;
}
.counter .unit-label{
  font-size:.68rem;
  color:var(--muted);
  margin-top:.3rem;
}

.heart-btn{
  margin-top:3rem;
  padding:.9rem 2.2rem;
  border-radius:999px;
  border:1px solid rgba(217,143,160,.5);
  background:rgba(217,143,160,.08);
  color:var(--cream);
  font-family:'Manrope', sans-serif;
  font-size:.95rem;
  cursor:pointer;
  transition:background .3s ease, transform .2s ease;
}
.heart-btn:hover{
  background:rgba(217,143,160,.18);
  transform:scale(1.04);
}
.heart-btn.beat{
  animation:beat .5s ease;
}
@keyframes beat{
  0%,100%{ transform:scale(1); }
  30%{ transform:scale(1.15); }
}

footer{
  position:relative;
  z-index:1;
  text-align:center;
  padding:4vh 0 6vh;
  font-size:.75rem;
  color:var(--muted);
  scroll-snap-align:end;
}
</style>