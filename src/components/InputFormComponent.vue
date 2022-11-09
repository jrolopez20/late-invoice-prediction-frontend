<template>
  <q-form ref="myForm" autofocus @submit="onSubmit" @reset="onReset">
    <q-card class="my-card">
      <q-card-section class="bg-primary text-white">
        <div class="text-h6">Invoice information</div>
      </q-card-section>

      <q-card-section v-if="result">
        <q-banner
          inline-actions
          rounded
          class="text-white"
          :class="messageStyle"
        >
          <div v-html="result"></div>

          <template v-slot:action>
            <q-btn flat label="Dismiss" @click="onDismiss()" />
          </template>
        </q-banner>
      </q-card-section>

      <q-card-section>
        <div class="row">
          <div class="col-sm-6 col-12 q-pa-sm">
            <q-input
              v-model="formState.Fecha"
              mask="date"
              :rules="[
                (val) =>
                  (val !== null && val !== '') || 'Please Fecha is required',
                'date',
              ]"
              label="Fecha *"
            >
              <template v-slot:append>
                <q-icon name="event" class="cursor-pointer">
                  <q-popup-proxy
                    cover
                    transition-show="scale"
                    transition-hide="scale"
                  >
                    <q-date v-model="formState.Fecha">
                      <div class="row items-center justify-end">
                        <q-btn
                          v-close-popup
                          label="Close"
                          color="primary"
                          flat
                        />
                      </div>
                    </q-date>
                  </q-popup-proxy>
                </q-icon>
              </template>
            </q-input>
          </div>
          <div class="col-sm-6 col-12 q-pa-sm">
            <q-input
              v-model="formState.RFCCliente"
              label="RFCCliente *"
              :rules="[
                (val) =>
                  (val !== null && val !== '') ||
                  'Please RFCCliente is required',
              ]"
            />
          </div>

          <div class="col-sm-6 col-12 q-pa-sm">
            <q-input
              v-model="formState.FechaVencimiento"
              mask="date"
              :rules="[
                (val) =>
                  (val !== null && val !== '') ||
                  'Please FechaVencimiento is required',
                'date',
              ]"
              label="FechaVencimiento *"
            >
              <template v-slot:append>
                <q-icon name="event" class="cursor-pointer">
                  <q-popup-proxy
                    cover
                    transition-show="scale"
                    transition-hide="scale"
                  >
                    <q-date v-model="formState.FechaVencimiento">
                      <div class="row items-center justify-end">
                        <q-btn
                          v-close-popup
                          label="Close"
                          color="primary"
                          flat
                        />
                      </div>
                    </q-date>
                  </q-popup-proxy>
                </q-icon>
              </template>
            </q-input>
          </div>
          <div class="col-sm-6 col-12 q-pa-sm">
            <!-- type="number" -->
            <q-input
              v-model="formState.Monto"
              label="Monto *"
              :rules="[
                (val) =>
                  (val !== null && val !== '') || 'Please type your amount',
                (val) => val > 0 || 'Please type a real amount',
              ]"
            />
          </div>
        </div>
      </q-card-section>

      <q-separator />

      <q-card-actions align="right">
        <q-btn type="reset" label="Reset" color="primary" flat />
        <q-btn type="submit" label="Accept" color="primary" />
      </q-card-actions>
    </q-card>
  </q-form>
</template>

<script>
import { useQuasar } from 'quasar';
import { ref, reactive } from 'vue';
import { api } from 'boot/axios';

function buildData(invoice) {
  const Fecha = invoice.Fecha;
  const RFCCliente = invoice.RFCCliente;
  const FechaVencimiento = invoice.FechaVencimiento;
  const Monto = Number(invoice.Monto);

  return [Fecha, RFCCliente, FechaVencimiento, Monto];
}

export default {
  setup() {
    const myForm = ref(null);
    const $q = useQuasar();

    const result = ref(null);
    const messageStyle = ref(null);

    const formState = reactive({
      Fecha: '',
      RFCCliente: '',
      FechaVencimiento: '',
      Monto: 0,
    });

    function predictionToString(prediction) {
      messageStyle.value = 'bg-red';
      switch (prediction) {
        case 1:
          messageStyle.value = 'bg-green';
          return 'en tiempo';
        case 2:
          messageStyle.value = 'bg-orange';
          return 'de 1 a 30 días';
        case 3:
          return 'de 31 a 60 días';
        case 4:
          return 'de 61 a 90 días';
        default:
          return 'más de 90 días';
      }
    }

    function onSubmit() {
      myForm.value.validate().then((success) => {
        if (success) {
          const data = [buildData({ ...formState })];

          $q.loading.show();
          api
            .post('/finance_factoring', data)
            .then((response) => {
              $q.loading.hide();
              const predicted = response.data;
              result.value = `Esta factura es posible que se pague <b>${predictionToString(
                predicted[0]
              )}</b>`;

              $q.notify({
                color: 'green-4',
                textColor: 'white',
                icon: 'cloud_done',
                message: 'Operation completed',
              });
            })
            .catch(() => {
              $q.loading.hide();
              $q.notify({
                color: 'negative',
                message: 'Operation failed',
                icon: 'report_problem',
              });
            });
        }
      });
    }

    function onReset() {
      formState.Fecha = '';
      formState.RFCCliente = '';
      formState.FechaVencimiento = '';
      formState.Monto = 0;
      myForm.value.resetValidation();
    }

    function onDismiss() {
      result.value = null;
    }

    return {
      formState,
      myForm,
      onSubmit,
      onDismiss,
      onReset,
      result,
      messageStyle,
    };
  },
};
</script>
