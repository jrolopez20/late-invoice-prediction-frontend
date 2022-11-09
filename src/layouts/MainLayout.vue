<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img
              src="https://scontent.fcuu3-1.fna.fbcdn.net/v/t39.30808-6/277561253_4282569695179356_3827948783356771795_n.jpg?_nc_cat=103&ccb=1-7&_nc_sid=09cbfe&_nc_ohc=TbhKQip0REIAX-uZ9OT&_nc_ht=scontent.fcuu3-1.fna&oh=00_AT-jX3bYTz_tyrrnfyCDji5Z_fyLgzgKW50QOLR_kZKFqQ&oe=635B5406"
            />
          </q-avatar>
          Forecasting Account Receivables - Demo
        </q-toolbar-title>

        <q-btn
          dense
          flat
          round
          icon="help"
          @click="toggleRightDrawer"
          matTooltip="text"
        >
          <q-tooltip>Mostrar ejemplos de prueba</q-tooltip>
        </q-btn>
      </q-toolbar>
    </q-header>

    <q-drawer show-if-above v-model="rightDrawerOpen" side="right" bordered>
      <div class="q-pa-sm" style="max-width: 350px">
        <q-list bordered class="rounded-borders">
          <q-expansion-item
            switch-toggle-side
            expand-separator
            label="A tiempo"
          >
            <q-card>
              <q-card-section>
                <table>
                  <tr>
                    <td><b>Fecha:</b></td>
                    <td>2021-02-18</td>
                  </tr>
                  <tr>
                    <td><b>RFC:</b></td>
                    <td>VILC600421R15</td>
                  </tr>
                  <tr>
                    <td><b>Vence:</b></td>
                    <td>2021-03-29</td>
                  </tr>
                  <tr>
                    <td><b>Monto:</b></td>
                    <td>64004.16</td>
                  </tr>
                </table>
              </q-card-section>
            </q-card>
          </q-expansion-item>

          <q-expansion-item
            switch-toggle-side
            expand-separator
            label="Pago retrasado de 1-30 días"
          >
            <q-card>
              <q-card-section>
                <table>
                  <tr>
                    <td><b>Fecha:</b></td>
                    <td>2022-01-17</td>
                  </tr>
                  <tr>
                    <td><b>RFC:</b></td>
                    <td>TRA021010QA8</td>
                  </tr>
                  <tr>
                    <td><b>Vence:</b></td>
                    <td>2022-05-02</td>
                  </tr>
                  <tr>
                    <td><b>Monto:</b></td>
                    <td>357090.92</td>
                  </tr>
                </table>
              </q-card-section>
            </q-card>
          </q-expansion-item>

          <q-expansion-item
            switch-toggle-side
            expand-separator
            label="Pago retrasado de 31-60 días"
          >
            <q-card>
              <q-card-section>
                <table>
                  <tr>
                    <td><b>Fecha:</b></td>
                    <td>2017-01-12</td>
                  </tr>
                  <tr>
                    <td><b>RFC:</b></td>
                    <td>DDI140206JB6</td>
                  </tr>
                  <tr>
                    <td><b>Vence:</b></td>
                    <td>2018-03-01</td>
                  </tr>
                  <tr>
                    <td><b>Monto:</b></td>
                    <td>5633.04</td>
                  </tr>
                </table>
              </q-card-section>
            </q-card>
          </q-expansion-item>

          <q-expansion-item
            switch-toggle-side
            expand-separator
            label="Pago retrasado de 61-90 días"
          >
            <q-card>
              <q-card-section>
                <table>
                  <tr>
                    <td><b>Fecha:</b></td>
                    <td>2019/03/04</td>
                  </tr>
                  <tr>
                    <td><b>RFC:</b></td>
                    <td>DIB141028514</td>
                  </tr>
                  <tr>
                    <td><b>Vence:</b></td>
                    <td>2019/05/17</td>
                  </tr>
                  <tr>
                    <td><b>Monto:</b></td>
                    <td>3132.00</td>
                  </tr>
                </table>
              </q-card-section>
            </q-card>
          </q-expansion-item>

          <q-expansion-item
            switch-toggle-side
            expand-separator
            label="Pago retrasado más de 90 días"
          >
            <q-card>
              <q-card-section>
                <table>
                  <tr>
                    <td><b>Fecha:</b></td>
                    <td>2018-01-23</td>
                  </tr>
                  <tr>
                    <td><b>RFC:</b></td>
                    <td>MOHD820109SF5</td>
                  </tr>
                  <tr>
                    <td><b>Vence:</b></td>
                    <td>2018-04-17</td>
                  </tr>
                  <tr>
                    <td><b>Monto:</b></td>
                    <td>67345.54</td>
                  </tr>
                </table>
              </q-card-section>
            </q-card>
          </q-expansion-item>
        </q-list>
      </div>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'MainLayout',

  components: {},

  setup() {
    const rightDrawerOpen = ref(false);

    return {
      rightDrawerOpen,
      toggleRightDrawer() {
        rightDrawerOpen.value = !rightDrawerOpen.value;
      },
    };
  },
});
</script>
