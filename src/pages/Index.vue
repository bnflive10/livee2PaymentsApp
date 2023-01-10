<template>
  <q-page class="q-pa-xl">
    <div class="row">
      <div class="col-8">
        <div class="text-center">
          <img
            src="https://user-images.githubusercontent.com/18400142/132318330-b8536515-d0d3-44ba-817e-ee2d269722f6.png"
            alt=""
            style="width: 100px; height: 100px"
          />
        </div>

        <q-form @submit="onSubmit" class="q-gutter-md">
          <q-input
            filled
            v-model="payload.amount"
            label="Insira o valor a pagar *"
            lazy-rules
            :rules="[
              (val) =>
                (val && val.length > 0) || 'Por favor, preencha este campo',
            ]"
          />

          <q-input
            filled
            type="number"
            v-model="payload.phone"
            label="Numero com MPesa 84/85"
            lazy-rules
            :rules="[
              (val) =>
                (val !== null && val !== '') ||
                'Por favor, preencha este campos',
              (val) =>
                (val > 0 && val.toString().length === 9) ||
                'Insira um numero valido',
            ]"
          />

          <q-select
            filled
            v-model="payload.reference"
            :options="options"
            label="Indique a referencia"
            lazy-rules
            :rules="[
              (val) =>
                (val !== null && val !== 'Selecione') ||
                'Por favor, selecione alguma opcao',
            ]"
          />

          <div class="text-right">
            <q-btn
              label="Submit"
              type="submit"
              color="primary"
              style="width: 180px; height: 40px"
            />
          </div>
        </q-form>
      </div>
    </div>
  </q-page>
</template>

<script>
import { handleMpesaC2bPayment } from "e2payments";

export default {
  name: "Index",
  data() {
    return {
      payload: {
        amount: null,
        phone: null,
        reference: null,
      },
      options: [
        "Selecione",
        "Live",
        "TestarPagamento",
        "PagarCurso",
        "DoarValor",
      ],
    };
  },
  methods: {
    onSubmit() {
      this.$q.loading.show();
      handleMpesaC2bPayment({
        CLIENT_ID: "9830c3d8-6ce9-47c3-84e6-61331496f998",
        CLIENT_SECRET: "6y8wboidrrKbsi7YDtmJepu9AwdfYhXhQJ8LyWL1",
        walletId: 342449,
        ...this.payload,
      }).then((response) => {
        this.$q.loading.hide()

        this.$q.notify({
          color: "green-4",
          textColor: "white",
          icon: "check_circle",
          message: "Parabens, Formulario Submetido...",
        });
      }).catch(err => {
        this.$q.loading.hide()

        alert('error: ', err);

      });
    },
  },
};
</script>
