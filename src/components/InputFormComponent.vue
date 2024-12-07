<template>
  <q-form
    ref="myForm"
    autofocus
    @submit="onSubmit"
    @reset="onReset"
    style="padding: 10px"
  >
    <q-card class="my-card">
      <q-card-section class="bg-primary text-white">
        <div class="text-h6">Accounts receivable prediction form</div>
        <span>Please introduce the invoice information</span>
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
              label="RFC del pagador *"
              :rules="[
                (val) =>
                  (val !== null && val !== '') || 'Please RFC is required',
              ]"
            />
          </div>
          <div class="col-sm-4 col-12 q-pa-sm">
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
          <div class="col-sm-4 col-12 q-pa-sm">
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
        </div>
      </q-card-section>

      <q-card-section v-if="result">
        <iframe
          :srcdoc="explanation"
          style="width: 100%; height: 250px"
        ></iframe>
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

export default {
  setup() {
    const myForm = ref(null);
    const $q = useQuasar();

    const result = ref(null);
    const explanation = ref(null);
    const messageStyle = ref(null);

    const formState = reactive({
      CreatedAt: '2022/11/01',
      DueDate: '2023/02/01',
      RFCPagador: 'XSASD324',
      Amount: 120,
      AdvancePercentage: 1500, // Porcentaje de adelanto
      CreditLineLimit: 2566000, // Línea límite
      Dob: '1985/12/04', // Fecha de nacimiento del cliente
    });

    function predictionToString(prediction) {
      messageStyle.value = 'bg-red';
      switch (prediction) {
        case 1:
          messageStyle.value = 'bg-green';
          return 'before time';
        case 2:
          messageStyle.value = 'bg-green';
          return 'on time';
        case 3:
          messageStyle.value = 'bg-orange';
          return '1-7 days late';
        case 4:
          return '8-21 days late';
        case 5:
          return 'More than 21 days late';
        default:
          return 'Unknow prediction';
      }
    }

    function onSubmit() {
      myForm.value.validate().then((success) => {
        if (success) {
          const data = {
            porcentaje_adelanto: formState.AdvancePercentage,
            factura_importe: formState.Amount,
            fecha_inicio: formState.CreatedAt,
            linea_limite: formState.CreditLineLimit,
            fecha_nacimiento: formState.Dob,
            fecha_fin: formState.DueDate,
            pagador_rfc: formState.RFCPagador,
          };

          $q.loading.show();
          api
            .post('/predict', data)
            .then((response) => {
              $q.loading.hide();
              const data = response.data;
              // console.log(data.explainer_html);
              explanation.value = data.explainer_html;
              result.value = `This invoice may be paid: <b>${predictionToString(
                data.prediction[0]
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
      myForm.value.resetValidation();
      result.value = null;
      explanation.value = null;
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
      explanation,
      messageStyle,
    };
  },
};
</script>
