<template>
  <h1 v-show="false">{{ this.APIListarCodigosEstado() }}</h1>
  <div>
    <ul class="tabla-estados">
      <li v-for="(estado, indice) in codigoEstados" :key="indice">
        {{ estado }}
      </li>
    </ul>
  </div>
  <input
    type="text"
    class="busqueda"
    v-on:keyup.enter="consulatar"
    v-model="codigo"
    placeholder="Ingrese codigo de estado"
  />
  <div class="consulta" v-show="consulta">
    <div class="consulta-item">
      <label for="">Casos positivos:</label>
      <input
        type="text"
        readonly
        onmousedown="return false;"
        :value="datos.positive"
      />
    </div>
    <div class="consulta-item">
      <label for="">Total de resultados de pruebas:</label>
      <input
        type="text"
        readonly
        onmousedown="return false;"
        :value="datos.totalTestResults"
      />
    </div>
    <div class="consulta-item">
      <label for="">Actualmente hospitalizados:</label>
      <input
        type="text"
        readonly
        onmousedown="return false;"
        :value="datos.hospitalizedCurrently"
      />
    </div>
    <div class="consulta-item">
      <label for="">Muertos:</label>
      <input
        type="text"
        readonly
        onmousedown="return false;"
        :value="datos.death"
      />
    </div>
    <div class="consulta-item">
      <label for="">Casos positivos virales:</label>
      <input
        type="text"
        readonly
        onmousedown="return false;"
        :value="datos.totalTestsViral"
      />
    </div>
    <div class="consulta-item">
      <label for="">Número de aumento en muertes:</label>
      <input
        type="text"
        readonly
        onmousedown="return false;"
        :value="datos.deathIncrease"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      codigoEstados: this.APIListarCodigosEstado(),
      codigo: "",
      consulta: false,
      datos: {
        positive: "",
        totalTestResults: "",
        hospitalizedCurrently: "",
        death: "",
        totalTestsViral: "",
        deathIncrease: "",
      },
    };
  },
  methods: {
    async consulatar() {
      const estado = await this.APIConsulta(this.codigo);
      this.datos = estado;
      this.consulta = true;
    },
    async APIListarCodigosEstado() {
      const estados = await fetch(
        "https://api.covidtracking.com/v1/states/current.json"
      ).then((r) => r.json());
      const codigoEstados = [];
      for (let i = 0; i <= 55; i++) {
        codigoEstados.push(estados[i].state);
      }
      this.codigoEstados = codigoEstados;
    },
    async APIConsulta(codigoEstado) {
      const estado = await fetch(
        "https://api.covidtracking.com/v1/states/" +
          codigoEstado +
          "/current.json "
      ).then((r) => r.json());
      const consulta = {
        positive: estado.positive,
        totalTestResults: estado.totalTestResults,
        hospitalizedCurrently: estado.hospitalizedCurrently,
        death: estado.death,
        totalTestsViral: estado.totalTestsViral,
        deathIncrease: estado.deathIncrease,
      };
      return consulta;
    },
  },
  mounted(){
    console.log('mounted');
  },
  beforeCreate(){
    console.log('beforeCreate');
  },
  created(){
    console.log('created');
  },
  beforeMount(){
    console.log('beforeMount');
  },
  beforeUpdate(){
    //console.log('beforeUpdate');
  },
  updated(){
    //console.log('updated');
  },
  activated(){
    console.log('activated');
  },
  deactivated(){
    console.log('deactivated');
  },
  beforeCreate(){
    console.log('beforeCreate');
  }
};
</script>

<style>
.consulta label,
input {
  display: block;
  text-align: left;
}
ul {
  list-style: none;
}
.tabla-estados {
  display: grid;
  grid-template-columns: auto auto auto auto;
  background-color: #fff0ff;
  color: #4b1c71;
  width: 200px;
  margin: auto;
  padding: 10px;
}
.busqueda {
  font-size: large;
  padding: 10px;
  margin: 30px auto;
}
.consulta {
  background-color: #dfe6e9;
  width: 50vw;
  margin: auto;
}
.consulta-item {
  margin: 30px;
}
</style>