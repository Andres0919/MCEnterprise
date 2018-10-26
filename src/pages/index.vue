<template>
  <q-page class="flex flex-center">
    <!-- <img alt="Quasar logo" src="~assets/quasar-logo-full.svg"> -->
    <q-modal v-model="basicModal" :content-css="{padding: '50px', minWidth: '50vw'}">
      <div class="q-display-1 q-mb-md">{{country.name}}</div>
      <!-- {{country.name}} - {{country.alpha2Code}} -->
      <q-table title="Table Title" :data="tableData" :columns="column" row-key="name" >
        <q-tr>
          <span></span>
        </q-tr>
      </q-table>
      <q-btn color="primary" @click="basicModal = false" label="Close" />
    </q-modal>
    <q-table :title="title" :data="countries" :columns="columns" row-key="name">
      <q-tr slot="body" slot-scope="props" :props="props">
        <q-td key="Country" :props="props">
          <span>{{ props.row.name }}</span>
        </q-td>
        <q-td key="buttons" :props="props">
          <div class="row items-center justify-between no-wrap">
            <q-btn size="sm" round dense color="tertiary" icon="search" @click.native="showModal(props.row.name)" v-ripple.mat class="q-mr-sm" />
          </div>
        </q-td>
      </q-tr>
    </q-table>
  </q-page>
</template>
<script>
import axios from 'axios';
export default {
  name: 'PageIndex',
  data: () => ({
    title: 'Countries',
    apiAllUrl: 'https://restcountries.eu/rest/v2/all',
    apiNameUrl: 'https://restcountries.eu/rest/v2/name/',
    basicModal: false,
    columns: [
      { name: 'Country', required: true, label: 'Country', align: 'left', field: 'calories', sortable: true },
      { name: 'buttons', required: true, label: '', align: 'left', field: 'buttons', sortable: false }
    ],
    countries: [],
    country: [],
    column: [
      {
        name: 'desc',
        required: true,
        label: 'Dessert (100g serving)',
        align: 'left',
        field: 'name',
        sortable: true
      }
    ],
    tableData: [
      {
        name: 'Frozen Yogurt',
        calories: 159,
        fat: 6.0,
        carbs: 24,
        protein: 4.0,
        sodium: 87,
        calcium: '14%',
        iron: '1%'
      }
    ]
  }),
  methods: {
    getAllCountries() {
      axios
        .get(this.apiAllUrl)
        .then(response => {
          this.countries = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
    showModal(name) {
      axios
        .get(this.apiNameUrl + name)
        .then(response => {
          this.country = response.data[0];
        })
        .catch(error => {
          console.log(error);
        });
      this.basicModal = true;
    }
  },
  mounted: function() {
    this.getAllCountries();
  }
};
</script>
