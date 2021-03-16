<template>
  <q-page class="flex flex-center">

    <q-card class="q-ma-none" style="min-width: 15cm;">
      <q-card-section>
        <q-toolbar class="bg-grey-3 q-pa-none">
            <q-avatar>
                <q-icon name="calculate" size="35px" color="red"/>
            </q-avatar>
            <q-toolbar-title class="text-subtitle1 text-black">Calcular número Fibonacci</q-toolbar-title>
        </q-toolbar>

        <q-card-section />

        <div class="col q-pa-md text-body">
          <q-input
            autofocus
            name="numero"
            label="Ingrese Número a calcular"
            v-model="numero"
            type="number"
            :error="error"
            :error-message="msgError"
            @keydown.enter="onCalcular"
          />

          <q-card-section />

          <q-card-actions align="center">
            <q-btn
              color="primary"
              label="Calcular"
              @click="onCalcular"
              :disable="!numero"
            />
          </q-card-actions>

          <!-- <q-card-section /> -->

          <h6 class="q-ma-none q-pb-sm">Resultado cálculo:</h6>
          <div class="salida">{{ result }}</div>
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      numero: '',
      result: '',
      error: false,
      msgError: ''
    }
  },
  methods: {
    onCalcular () {
      // Resetear errores
      this.error = false
      this.msgError = ''

      // Validar número ingresado
      if (this.numero <= 0) {
        this.error = true
        this.msgError = 'Número debe ser mayor que cero'
        return
      }

      // Invocar método ue arrija el resultado
      this.$axios.get('http://localhost:8080/api/v1/fibonacci/calculate/' + this.numero)
        .then(resp => {
          this.result = resp.data
        })
        .catch(err => {
          console.log('Error en: /api/v1/fibonacci/calculate', err)
        })        
    }
  }
}
</script>

<style scoped>
  .salida {
    max-width: 15cm;
    font-family: monospace;
    white-space: pre-wrap;      /* CSS3 */   
    white-space: -moz-pre-wrap; /* Firefox */    
    white-space: -pre-wrap;     /* Opera <7 */   
    white-space: -o-pre-wrap;   /* Opera 7 */    
    word-wrap: break-word;      /* IE */
  }
</style>