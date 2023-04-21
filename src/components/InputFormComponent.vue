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
              v-model="formState.CreatedAt"
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
                    <q-date v-model="formState.CreatedAt">
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
              v-model="formState.DueDate"
              mask="date"
              :rules="[
                (val) =>
                  (val !== null && val !== '') || 'Please DueDate is required',
                'date',
              ]"
              label="Fecha de expiración *"
            >
              <template v-slot:append>
                <q-icon name="event" class="cursor-pointer">
                  <q-popup-proxy
                    cover
                    transition-show="scale"
                    transition-hide="scale"
                  >
                    <q-date v-model="formState.DueDate">
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
              v-model="formState.Dob"
              mask="date"
              :rules="[
                (val) =>
                  (val !== null && val !== '') || 'Please dob is required',
                'date',
              ]"
              label="Fecha de nacimiento *"
            >
              <template v-slot:append>
                <q-icon name="event" class="cursor-pointer">
                  <q-popup-proxy
                    cover
                    transition-show="scale"
                    transition-hide="scale"
                  >
                    <q-date v-model="formState.Dob">
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
              v-model="formState.RFCPagador"
              label="RFC del cliente *"
              :rules="[
                (val) =>
                  (val !== null && val !== '') || 'Please RFC is required',
              ]"
            />
          </div>
          <div class="col-sm-6 col-12 q-pa-sm">
            <q-input
              v-model="formState.Amount"
              label="Monto *"
              :rules="[
                (val) =>
                  (val !== null && val !== '') || 'Please type your amount',
                (val) => val > 0 || 'Please type a real amount',
              ]"
            />
          </div>
          <div class="col-sm-6 col-12 q-pa-sm">
            <q-input
              v-model="formState.CreditLineLimit"
              label="Línea límite *"
              :rules="[
                (val) =>
                  (val !== null && val !== '') ||
                  'Please type your credit line limit',
                (val) => val > 0 || 'Please type a real credit line limit',
              ]"
            />
          </div>
          <div class="col-sm-4 col-12 q-pa-sm">
            <q-input
              v-model="formState.AdvancePercentage"
              label="Porcentaje de adelanto *"
              :rules="[
                (val) =>
                  (val !== null && val !== '') ||
                  'Please type your advance percentage',
                (val) => val > 0 || 'Please type a real advance percentage',
              ]"
            />
          </div>
          <div class="col-sm-4 col-12 q-pa-sm">
            <q-select
              v-model="formState.TaxRegime"
              :options="taxRegimeList"
              label="Régimen fiscal *"
              :rules="[
                (val) =>
                  (val !== null && val !== '') ||
                  'Please select your tax regime',
              ]"
            />
          </div>
          <div class="col-sm-4 col-12 q-pa-sm">
            <q-select
              v-model="formState.Sector"
              :options="sectorList"
              label="Sector *"
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

// function buildData(invoice) {
//   const CreatedAt = invoice.CreatedAt;
//   const RFCPagador = invoice.RFCPagador;
//   const DueDate = invoice.DueDate;
//   const Amount = Number(invoice.Amount);
//   return [CreatedAt, RFCPagador, DueDate, Amount];
// }

export default {
  setup() {
    const myForm = ref(null);
    const $q = useQuasar();

    const result = ref(null);
    const messageStyle = ref(null);

    const formState = reactive({
      CreatedAt: '',
      DueDate: '',
      RFCPagador: '',
      Amount: 0,
      AdvancePercentage: 0, // Porcentaje de adelanto
      CreditLineLimit: 0, // Línea límite
      TaxRegime: null, // Cliente regimen fiscal
      Sector: null, // Cliente sector
      Dob: '', // Fecha de nacimiento del cliente
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
          const { Sector, TaxRegime, ...values } = formState;
          const data = {
            ...values,
            Sector: Sector.value,
            TaxRegime: TaxRegime.value,
          };

          $q.loading.show();
          api
            .post('/predict', data)
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
      formState.CreatedAt = '';
      formState.DueDate = '';
      formState.Dob = '';
      formState.RFCPagador = '';
      formState.Amount = 0;
      formState.AdvancePercentage = 0;
      formState.CreditLineLimit = 0;
      formState.TaxRegime = null;
      formState.Sector = null;
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
      taxRegimeList: [
        { label: 'PM', value: 0 },
        { label: 'PFAE', value: 1 },
      ],
      sectorList: [
        { label: 'Privado', value: 0 },
        { label: 'Micro Empresa', value: 1 },
        { label: 'Rural', value: 2 },
      ],
    };
  },
};
</script>
