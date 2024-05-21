<script setup lang="ts">
import Input from '~/components/TheInput.vue'

const params = useRoute('/hi/[name]').params
const router = useRouter()

const weight = ref('')
const repeat = ref('')

const result = reactive([
  { name: 'Brzycki', func: calculateOneRepMaxBrzycki, value: '0' },
  { name: 'Lombardi', func: calculateOneRepMaxLombardi, value: '0' },
  { name: 'Mayhew', func: calculateOneRepMaxMayhew, value: '0' },
  { name: 'OConner', func: calculateOneRepMaxOConner, value: '0' },
])

function calc() {
  const w = Number.parseFloat(weight.value)
  const r = Number.parseInt(repeat.value)
  if (Number.isNaN(w) || Number.isNaN(r)) {
    return alert('Please input a valid number')
  }
  else {
    for (const item of result)
      item.value = item.func(w, r).toFixed(2)
  }
}

function calculateOneRepMaxBrzycki(weight: number, reps: number): number {
  return weight * (36 / (37 - reps))
}

function calculateOneRepMaxLombardi(weight: number, reps: number): number {
  return weight * reps ** 0.10
}

function calculateOneRepMaxMayhew(weight: number, reps: number): number {
  return (100 * weight) / (52.2 + 41.9 * Math.exp(-0.055 * reps))
}

function calculateOneRepMaxOConner(weight: number, reps: number): number {
  return weight * (1 + 0.025 * reps)
}
</script>

<template>
  <div>
    <div i-carbon-pedestrian inline-block text-4xl />
    <p>
      Hi, {{ params.name }}
    </p>
    <p text-sm op50 />

    <div mt-8 flex flex-col items-center justify-center>
      <Input v-model="weight" placeholder="weight" />
      <Input v-model="repeat" placeholder="repeat" my-4 />
      <button w-250px py-2 text-sm btn @click="calc">
        Calc
      </button>
    </div>

    <div my-4>
      <p v-for="item in result" :key="item.name">
        <span text-sx font-bold op50>{{ `${item.name}:` }}</span>
        <span mx-4>{{ item.value }}</span>
      </p>
    </div>

    <div>
      <button m-3 mt-8 text-sm btn @click="router.back()">
        Back
      </button>
    </div>
  </div>
</template>
