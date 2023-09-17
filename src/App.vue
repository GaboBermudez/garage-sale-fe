<template>
  <div>
    <div class="flex justify-content-center flex-column">
      <h2>🌈 VENTA DE GARASH ✨</h2>
      <div class="card flex justify-content-center flex-column">
        <h3 class="mb-1">compradora</h3>
        <Dropdown v-model="buyerSelected" :options="chicas" optionLabel="name" class="w-full" />
      </div>
      <div class="card flex justify-content-center flex-column">
        <h3 class="mb-1">vendedora</h3>
        <Dropdown v-model="sellerSelected" :options="chicas" optionLabel="name" class="w-full" />
      </div>
      <div class="card flex justify-content-center flex-column">
        <h3 class="mb-1" for="buyer">monto</h3>
        <InputText type="text" v-model="monto" />
      </div>
      <div class="card flex justify-content-center flex-column">
        <Button label="comprar" @click="comprar" />
      </div>
    </div>
    <div>
      <DataTable :value="compras" tableStyle="">
        <Column field="buyer" header="Compradora"></Column>
        <Column field="seller" header="Vendedora"></Column>
        <Column field="monto" header="Monto"></Column>
      </DataTable>
    </div>
  </div>
</template>
<script setup>
  import { ref, onMounted } from 'vue'
  import Dropdown from 'primevue/dropdown'
  import InputText from 'primevue/inputtext'
  import Button from 'primevue/button'
  import DataTable from 'primevue/datatable'
  import Column from 'primevue/column'
  import axios from 'axios'

  const monto = ref()
  const buyerSelected = ref()
  const chicas = ref([{ name: 'JAE' }, { name: 'NATI' }, { name: 'MARINA' }, { name: 'MARIPOSA' }, { name: 'VALE' }])
  const sellerSelected = ref()
  const compras = ref()

  onMounted(async () => {
    await loadCompras()
  })

  const comprar = async () => {
    // codigo para comprar
    const payload = { buyer: buyerSelected.value.name, seller: sellerSelected.value.name, monto: monto.value }
    await axios.post('https://garage-sale-api-production.up.railway.app/comprar', payload)
    buyerSelected.value = null
    sellerSelected.value = null
    monto.value = ''
    await loadCompras()
  }

  const loadCompras = async () => {
    const result = await axios.get('https://garage-sale-api-production.up.railway.app/compras')
    compras.value = result.data
  }
</script>

<style scoped></style>
