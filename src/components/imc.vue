<template>
  <div class="container">
    <a-row class="content mt-xl">
      <a-col span="24">
        <h1 class="imc-title">Calculadora de IMC</h1>
        <h2>Insira abaixo o peso (quilos) e a altura (metros)</h2>
      </a-col>
      <a-col span="24">
        <a-input-number
          size="large"
          step="0.01"
          :value="height"
          @change="updateHeight"
          class="mt-lg"
          min="0.3"
          addon-after="m"
        ></a-input-number>
      </a-col>
      <a-col span="24">
        <a-input-number
          size="large"
          :value="weight"
          @change="updateWeight"
          addon-after="kg"
          class="mt-md"
          min="1"
        ></a-input-number>
      </a-col>
      <a-col span="12" offset="6" class="mt-lg">
        <a-button @click="updateIMC" size="large" class="imc-btn"
          >Calcular</a-button
        >
      </a-col>
      <a-col span="24" class="mt-md">
        <h3 underline>{{ imcText }}</h3>
      </a-col>
    </a-row>
  </div>
</template>

<script>
import { InputNumber, Button, Row, Col } from "ant-design-vue";
import axios from "axios";
export default {
  name: "IMC",
  components: {
    AInputNumber: InputNumber,
    AButton: Button,
    ARow: Row,
    ACol: Col,
  },
  data() {
    return {
      height: "1.6",
      weight: "65",
      imcText: "",
      imcLimits: {
        18.5: "abaixo do normal",
        24.9: "normal",
        29.9: "sobrepeso",
        34.9: "obesidade grau I",
        39.9: "obesidade grau II",
        999.9: "obesidade grau III", // Last case
      },
      imc: null,
    };
  },
  methods: {
    async getIMC() {
      try {
        let imc = await axios.get(
          `https://i-m-c.herokuapp.com/imc?altura=${this.height}&peso=${this.weight}`
        );
        this.imc = imc.data;
      } catch (error) {
        this.imcText =
          "Houve um erro ao calcular o IMC. Por favor tente novamente mais tarde.";
      }
    },
    async updateIMC() {
      await this.getIMC();
      for (const limit in this.imcLimits) {
        if (this.imc < limit) {
          this.imcText = `O IMC de alguém com ${this.height}m e ${this.weight}kg é ${this.imc}, classificado como ${this.imcLimits[limit]}`;
          return;
        }
      }
    },
    updateHeight(val) {
      this.height = val;
    },
    updateWeight(val) {
      this.weight = val;
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  background-color: ghostwhite;
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  z-index: 10;
}
h2 {
  color: rgba(35, 34, 34, 0.656);
}
h3 {
  color: rgba(35, 34, 34, 0.94);
  text-decoration: solid;
  font-size: large;
}
.content {
  height: auto;
}
.mt-md {
  margin-top: 16px;
}
.mt-lg {
  margin-top: 32px;
}
.mt-xl {
  margin-top: 10%;
}
.imc-title {
  color: rgb(51, 175, 216);
}
.imc-btn {
  color: white;
  background-color: rgb(62, 177, 215);
  font-weight: 600;
  letter-spacing: 0.075em;
}
</style>
