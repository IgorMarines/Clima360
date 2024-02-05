<template>
  <div class="container">
    <h1>Informações Meteorológicas</h1>
    <div class="input-container">
      <input
        type="text"
        v-model="cityInput"
        class="custom-input"
        placeholder="Digite sua cidade aqui"
      />
      <button class="custom-button" @click="handleFetch">
        <span>Buscar Clima</span>
      </button>
    </div>

    <p v-if="errorMessage" class="error-message">{{ this.errorMessage }}</p>
    <div class="responseApiContainer" v-if="!data">
      <h2>Digite um endereço correto para obter as informações meteorologicas.</h2>
    </div>
    <div class="responseApiContainer" v-if="data">
      <h2>Resultado</h2>

      <hr />
      <p>{{ data.name }}, {{ data.sys.country }}</p>
      <p>Temperatura: {{ data.main.temp }}°C</p>
      <p>Condição do tempo: {{ data.weather[0].description }}</p>
      <img :src="`https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`" alt="" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cityInput: '',
      apiKey: import.meta.env.VITE_APIKEY,
      lang: 'pt_br',
      unit: 'metric',
      errorMessage: null,
      data: null
    }
  },
  methods: {
    handleFetch() {
      const api = `https://api.openweathermap.org/data/2.5/weather?q=${this.cityInput}&units=${this.unit}&lang=${this.lang}&appid=${this.apiKey}`

      fetch(api)
        .then((response) => response.json())
        .then((data) => {
          this.data = data
          this.errorMessage = null
        })
        .catch((error) => {
          console.error('Erro:', error)
          this.errorMessage = 'Erro ao obter dados. Verifique a cidade e tente novamente.'
        })
    }
  }
}
</script>

<style scoped>
.container {
  max-width: 100%;
  margin: 0 auto;
  height: 100vh;
  background: url('https://i.pinimg.com/originals/0c/96/7c/0c967c4af27aa805391e3be495936acd.png');
  padding: 20px;
  text-align: center;
}

.container h1 {
  color: #fff;
  font-weight: bold;
}

.input-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.error-message {
  color: red;
  font-weight: bold;
  margin-top: 10px;
}

.custom-input {
  padding: 10px;
  font-size: 16px;
  border: 1px solid #2196f3; /* Cor padrão do Material Design Blue */
  border-radius: 4px;
  flex: 1;
}

.custom-button {
  background-color: #2196f3; /* Cor padrão do Material Design Blue */
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  margin-left: 10px;
  transition: background-color 0.3s;
}

.custom-button:hover {
  background-color: #1565c0; /* Cor mais escura ao passar o mouse */
}

.custom-button span {
  margin-right: 5px;
}

.responseApiContainer {
  background: #ffffff; /* Light Sky Blue */
  color: #000000;
  padding: 15px 30px;
  border-radius: 15px;
  box-shadow:
    rgba(0, 0, 0, 0.25) 0px 54px 55px,
    rgba(0, 0, 0, 0.12) 0px -12px 30px,
    rgba(0, 0, 0, 0.12) 0px 4px 6px,
    rgba(0, 0, 0, 0.17) 0px 12px 13px,
    rgba(0, 0, 0, 0.09) 0px -3px 5px;
}

.responseApiContainer h2 {
  font-weight: bold;
}
</style>
