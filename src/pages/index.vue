<template>
  <v-container>
    <h2 class="mb-5">Pesquisa de Pessoas</h2>
    <v-form @submit.prevent="searchPeople">
      <v-row>
        <v-col cols="12" md="8">
          <v-text-field v-model="searchTerm" label="Pesquisar por Nome, ID ou CPF"></v-text-field>
        </v-col>
        <v-col cols="12" md="4">
          <v-select v-model="searchBy" :items="searchOptions" label="Pesquisar por"></v-select>
        </v-col>
      </v-row>
      <v-btn type="submit" color="primary">Pesquisar</v-btn>
    </v-form>

    <v-card class="mt-5" v-for="person in people" :key="person.id">
      <v-tabs
        v-model="tab"
        bg-color="primary"
      >
        <v-tab value="one">Dados Pessoais</v-tab>
        <v-tab value="two">Endereços</v-tab>
      </v-tabs>

      <v-card-text>
        <v-window v-model="tab">
          <v-window-item value="one">
            <v-list>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>Nome: {{ person.name }}</v-list-item-title>
                  <v-list-item-subtitle>Nome Social: {{ person.social_name }}</v-list-item-subtitle>
                  <v-list-item-subtitle>CPF: {{ person.cpf }}</v-list-item-subtitle>
                  <v-list-item-subtitle>Telefone: {{ person.phone }}</v-list-item-subtitle>
                  <v-list-item-subtitle>Email: {{ person.email }}</v-list-item-subtitle>
                  <v-list-item-subtitle>Pai: {{ person.father_name }}</v-list-item-subtitle>
                  <v-list-item-subtitle>Mãe: {{ person.mother_name }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-window-item>

          <v-window-item value="two">
            <v-list v-for="address in person.addresses" :key="address.id">
              <v-list-item >
                <v-list-item-content>
                  <v-list-item-title>{{ address.address_type  }}</v-list-item-title>
                  <v-list-item-title>{{ address.street  }} , {{ address.number }} , {{ address.complement }} , {{ address.neighborhood }}</v-list-item-title>
                  <v-list-item-title>{{ address.cep }} - {{ address.city }} - {{ address.state }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-window-item>
        </v-window>
      </v-card-text>

    </v-card>
    <v-alert class="mt-5" v-if="!people.length" type="info">Nenhuma pessoa encontrada.</v-alert>
  </v-container>
</template>

<script>
import { ref } from 'vue';
import axios from '@/plugins/axios';

export default {
  data() {
    return {
      searchTerm: '',
      searchBy: 'name',
      searchOptions: ['name', 'id', 'cpf'],
      people: [],
      tab: ref('one'),
    };
  },
  methods: {
    searchPeople() {
      const url = `/people?${this.searchBy}=${this.searchTerm}`;
      axios.get(url)
        .then(response => {
          this.people = response.data;
        })
        .catch(error => {
          console.error('Error fetching people:', error);
        });
    }
  }
}
</script>
