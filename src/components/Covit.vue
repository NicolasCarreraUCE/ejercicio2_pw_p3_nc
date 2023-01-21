<template>
  <div class="tabla-estados">
    <ul>
      <li v-for="(estado, indice) in codigoEstados" :key="indice">{{ estado }}</li>
    </ul>
  </div>
  <input type="text" v-on:keyup.enter="consulatar" v-model="codigo" placeholder="Ingrese codigo de estado"/>
  <div class="consulta">
    <label for="">Casos positivos:</label>
    <input type="text" :value="datos.positive"/>
    <label for="">Total de resultados de pruebas:</label>
    <input type="text" :value="datos.totalTestResults"/>
    <label for="">Actualmente hospitalizados:</label>
    <input type="text" :value="datos.hospitalizedCurrently"/>
    <label for="">Muertos:</label>
    <input type="text" :value="datos.death"/>
    <label for="">Casos positivos virales:</label>
    <input type="text" :value="datos.totalTestsViral"/>
    <label for="">Número de aumento en muertes:</label>
    <input type="text" :value="datos.deathIncrease"/>
  </div>
  <button @click="setCodigoEstados">Probar</button>
</template>

<script>
export default {
  data() {
    return {
      codigoEstados: [],
      codigo: '',
      datos: {
        positive: '',
        totalTestResults: '',
        hospitalizedCurrently: '',
        death: '',
        totalTestsViral: '',
        deathIncrease: ''
      },
    };
  },
  methods: {
    async consulatar() {
        const estado = await this.APIConsulta(this.codigo)
        console.log(estado.positive);
        this.datos = estado
    },
    async setCodigoEstados(){
        const lista = await this.APIListarCodigosEstado()
        this.codigoEstados = lista
    },
    async APIListarCodigosEstado() {
      const estados = await fetch(
        "https://api.covidtracking.com/v1/states/current.json"
      ).then((r) => r.json());
      const codigoEstados = [];
      for (let i = 0; i <= 55; i++) {
        codigoEstados.push(estados[i].state);
      }
      return codigoEstados;
    },
    async APIConsulta(codigoEstado) {
      const estado = await fetch(
        "https://api.covidtracking.com/v1/states/" + codigoEstado + "/current.json "
      ).then((r) => r.json());
      const consulta = {
        positive: estado.positive,
        totalTestResults: estado.totalTestResults,
        hospitalizedCurrently: estado.hospitalizedCurrently,
        death: estado.death,
        totalTestsViral: estado.totalTestsViral,
        deathIncrease: estado.deathIncrease
      }
      return consulta
    },
  },
};
</script>

<style>
.consulta label,input {
    display: block;
} 
input {
    margin: 30px auto;
}
ul {
    list-style: none;
}
.tabla-estados {
    display: grid;
    grid-auto-columns: auto auto auto auto;
}
</style>