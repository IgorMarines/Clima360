<template>
  <div class="container">
    <div style="background-color: #000; padding: 15px; border-radius: 15px;">
      <h1>Informações Meteorológicas</h1>
      <div class="input-container">
        <input type="text" v-model="cityInput" class="custom-input" placeholder="Digite sua cidade aqui" />
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
        <img :src="`https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`" alt="Clima" />
      </div>
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
*
{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

body
{
  color: #ecf0f1;
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-size: 16px;
  line-height: 1.5;
}

/* Container */
.container
{
  background-image: url('https://i.pinimg.com/originals/0c/96/7c/0c967c4af27aa805391e3be495936acd.png');
  background-size: cover;
  padding: 40px;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.6);
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  backdrop-filter: blur(10px);
}

h1
{
  color: #f39c12;
  /* Gold */
  font-size: 2.4rem;
  margin-bottom: 20px;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 2px;
}

/* Input Section */
.input-container
{
  display: flex;
  justify-content: center;
  margin-bottom: 30px;
}

.custom-input
{
  padding: 14px;
  font-size: 16px;
  border: 2px solid #f39c12;
  border-radius: 25px;
  margin-right: 12px;
  background-color: rgba(236, 240, 241, 0.6);
  color: #fff;
  transition: 0.3s ease-in-out;
  width: 220px;
}

.custom-input::placeholder
{
  color: rgba(255, 255, 255, 0.7);
}

.custom-input:focus
{
  outline: none;
  border-color: #e67e22;
  /* Orange */
}

.custom-button
{
  background-color: #e67e22;
  /* Orange */
  color: #fff;
  border: none;
  padding: 14px 28px;
  border-radius: 25px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s, transform 0.3s;
  display: flex;
  align-items: center;
}

.custom-button:hover
{
  background-color: #d35400;
  /* Darker Orange */
  transform: scale(1.05);
}

.custom-button span
{
  margin-right: 8px;
  font-weight: bold;
}

/* Error Message */
.error-message
{
  color: #e74c3c;
  /* Red */
  font-weight: bold;
  margin-top: 20px;
  text-align: center;
  font-size: 1.1rem;
}

/* API Response Section */
.responseApiContainer
{
  background-color: rgba(236, 240, 241, 0.7);
  color: #fff;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  width: 100%;
  margin-top: 20px;
  text-align: left;
}

.responseApiContainer h2
{
  font-size: 1.8rem;
  margin-bottom: 15px;
  font-weight: 600;
}

.responseApiContainer hr
{
  border: 1px solid #fff;
  margin: 10px 0;
}

.responseApiContainer img
{
  max-width: 80px;
  margin-top: 15px;
  transition: transform 0.3s ease;
}

.responseApiContainer img:hover
{
  transform: scale(1.1);
}
</style>
