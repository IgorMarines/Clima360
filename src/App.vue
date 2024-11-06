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
      <h2>Digite um endereço correto para obter as informações meteorológicas.</h2>
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
    };
  },
  methods: {
    handleFetch() {
      const api = `https://api.openweathermap.org/data/2.5/weather?q=${this.cityInput}&units=${this.unit}&lang=${this.lang}&appid=${this.apiKey}`;

      fetch(api)
        .then((response) => response.json())
        .then((data) => {
          this.data = data;
          this.errorMessage = null;
        })
        .catch((error) => {
          console.error('Erro:', error);
          this.errorMessage = 'Erro ao obter dados. Verifique a cidade e tente novamente.';
        });
    }
  }
};
</script>

<style scoped>
/* Base Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

body {
  background-color: #2c3e50; /* Dark Blue */
  color: #ecf0f1; /* Light Grey */
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

/* Container */
.container {
  background-color: rgba(44, 62, 80, 0.85);
  padding: 40px;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.4);
  max-width: 500px;
  width: 100%;
  text-align: center;
}

h1 {
  color: #f39c12; /* Gold */
  font-size: 2.2rem;
  margin-bottom: 20px;
  font-weight: bold;
  text-transform: uppercase;
}

/* Input Section */
.input-container {
  display: flex;
  justify-content: center;
  margin-bottom: 30px;
}

.custom-input {
  padding: 12px;
  font-size: 16px;
  border: 2px solid #f39c12;
  border-radius: 25px;
  margin-right: 12px;
  background-color: rgba(236, 240, 241, 0.2);
  color: #fff;
  transition: 0.3s ease-in-out;
}

.custom-input::placeholder {
  color: rgba(255, 255, 255, 0.7);
}

.custom-input:focus {
  outline: none;
  border-color: #e67e22; /* Orange */
}

.custom-button {
  background-color: #e67e22; /* Orange */
  color: #fff;
  border: none;
  padding: 12px 25px;
  border-radius: 25px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.custom-button:hover {
  background-color: #d35400; /* Darker Orange */
}

.custom-button span {
  margin-right: 5px;
}

/* Error Message */
.error-message {
  color: #e74c3c; /* Red */
  font-weight: bold;
  margin-top: 20px;
}

/* API Response Section */
.responseApiContainer {
  background-color: rgba(236, 240, 241, 0.3);
  color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  width: 100%;
  margin-top: 20px;
  text-align: left;
}

.responseApiContainer h2 {
  font-size: 1.6rem;
  margin-bottom: 15px;
}

.responseApiContainer hr {
  border: 1px solid #fff;
  margin: 10px 0;
}

.responseApiContainer img {
  max-width: 60px;
  margin-top: 15px;
}
</style>
