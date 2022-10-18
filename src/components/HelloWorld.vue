<script setup>
import { ref } from 'vue'

defineProps({
  title: String
})

const limits = { 1000: 5, 500: 2, 100: 5, 50: 100, 30: 6 }
const amount = ref(0)
let result = ref({})

const iWantToGet = (amountRequired, limits) => {
  // code here
  let recur = (amountRequired, nominals) => {
    if (amountRequired == 0) return {}; // success
    if (!nominals.length) return; // failure
    let nominal = nominals[0];
    let count = Math.min(limits[nominal], Math.floor(amountRequired / nominal));
    for (let i = count; i >= 0; i--) {
        let result = recur(amountRequired - i*nominal, nominals.slice(1));
        if (result) return i ? { [nominal]: i, ...result } : result;
    }
  }
  result = recur(amountRequired, Object.keys(limits).map(Number).sort((a,b) => b - a));
  console.log(result)

}

</script>

<template>
  <h1>{{title}}</h1>

  <div class="card">
    <input 
      type="number" 
      name="" id="" 
      v-model="amount"      
    >
    <br>
    <button type="button" @click="iWantToGet(amount, limits)">Get Money</button>    
    <p>
      Result will show momentially on brower console.
      <!-- <code>
        {{result}}
      </code>  -->
    </p>
  </div>
  
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
