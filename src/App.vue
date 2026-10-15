<script setup>
import { computed, reactive, ref } from 'vue'

const values = reactive({
  raceHours: '1',
  raceMinutes: '0',
  lapMinutes: '2',
  lapSeconds: '0',
  consumption: '3,0',
})

const extra1 = ref(true)
const extra2 = ref(false)
const result = ref(null)
const error = ref('')
const resultLabel = computed(() => result.value === null ? '—' : result.value)

function parse(value) {
  return Number(value.trim().replace(',', '.'))
}

function calculate() {
  const raceHour = parse(values.raceHours)
  const raceMinutes = parse(values.raceMinutes)
  const lapTimeMinute = parse(values.lapMinutes)
  const lapTimeSeconds = parse(values.lapSeconds)
  const consumption = parse(values.consumption)

  if ([raceHour, raceMinutes, lapTimeMinute, lapTimeSeconds, consumption].some(value => !Number.isFinite(value))) {
    error.value = 'Digite apenas números válidos.'
    result.value = null
    return
  }
  if (raceHour < 0 || raceMinutes < 0 || lapTimeMinute < 0 || lapTimeSeconds < 0 || consumption <= 0) {
    error.value = 'Os tempos não podem ser negativos e o consumo deve ser maior que zero.'
    result.value = null
    return
  }

  const race = raceHour * 3600 + raceMinutes * 60
  const lap = lapTimeMinute * 60 + lapTimeSeconds
  if (race <= 0 || lap <= 0) {
    error.value = 'A duração da corrida e o tempo de volta devem ser maiores que zero.'
    result.value = null
    return
  }

  let fuel = (race / lap) * consumption
  if (extra1.value) fuel += consumption
  if (extra2.value) fuel += consumption
  result.value = Math.ceil(fuel)
  error.value = ''
}

function onEnter(event) {
  if (event.key === 'Enter') calculate()
}
</script>

<template>
  <main class="page-shell">
    <header class="page-header">
      <p class="eyebrow">ASSETTO CORSA COMPETIZIONE</p>
      <h1>Calculadora de combustível</h1>
      <p class="subtitle">Calcule o combustível mínimo necessário para completar sua corrida.</p>
    </header>

    <section class="panel" aria-label="Dados da corrida">
      <div class="section-heading">
        <b>01</b>
        <div><h2>Duração da corrida</h2><p>Informe o tempo total previsto.</p></div>
      </div>
      <div class="grid">
        <label class="field"><span>Horas</span><div class="input-wrap"><input v-model="values.raceHours" inputmode="decimal" @keydown="onEnter"><small>h</small></div></label>
        <label class="field"><span>Minutos</span><div class="input-wrap"><input v-model="values.raceMinutes" inputmode="decimal" @keydown="onEnter"><small>min</small></div></label>
      </div>

      <div class="divider"></div>
      <div class="section-heading">
        <b>02</b>
        <div><h2>Volta e consumo</h2><p>Use sua média de consumo por volta.</p></div>
      </div>
      <div class="grid">
        <label class="field"><span>Minutos por volta</span><div class="input-wrap"><input v-model="values.lapMinutes" inputmode="decimal" @keydown="onEnter"><small>min</small></div></label>
        <label class="field"><span>Segundos por volta</span><div class="input-wrap"><input v-model="values.lapSeconds" inputmode="decimal" @keydown="onEnter"><small>s</small></div></label>
        <label class="field"><span>Consumo por volta</span><div class="input-wrap"><input v-model="values.consumption" inputmode="decimal" @keydown="onEnter"><small>L</small></div></label>
      </div>

      <div class="extras">
        <label class="toggle"><span><strong>Adicionar volta de apresentação</strong><small>Adiciona uma volta de apresentação</small></span><input v-model="extra1" type="checkbox" aria-label="Adicionar volta de apresentação"></label>
        <label class="toggle"><span><strong>Adicionar volta adicional</strong><small>Adiciona uma volta adicional</small></span><input v-model="extra2" type="checkbox" aria-label="Adicionar volta adicional"></label>
      </div>
    </section>

    <div v-if="error" class="error" role="alert"><span>!</span>{{ error }}</div>
    <section class="result" :class="{ empty: result === null }" aria-live="polite">
      <div class="result-value"><small>COMBUSTÍVEL MÍNIMO</small><strong>{{ resultLabel }}</strong><em>litros</em></div>
      <button type="button" @click="calculate">Calcular combustível <span aria-hidden="true">→</span></button>
    </section>
    <p class="note">O resultado é arredondado para o próximo litro inteiro.</p>
  </main>
</template>
