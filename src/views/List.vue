<template>
  <v-card class="mx-5 my-5">
    <h1 class="mx-3 my-2">Fatura Listesi</h1>
    <router-link to="home" class="text-decoration-none">
      <v-btn class="mx-3 my-2" color="orange" fab small dark>
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
    </router-link>
    <v-card-title>
      <v-text-field v-model="search" append-icon="mdi-magnify" label="Ara" single-line hide-details></v-text-field>
    </v-card-title>
    <v-data-table :headers="headers" :items="tranItems" class="elevation-1" :search="search" @click:row="expandRow">

    </v-data-table>
  </v-card>
</template>
<script>
import { mapState, mapActions } from "vuex";

export default {
  name: "List",

  data() {
    return {
      search: "",
      headers: [
        {
          text: "No",
          align: "start",
          //sortable: false,
          value: "MASTER_TRAN_ID",
        },
        { text: "Tarih", value: "MASTER_TRAN_DATE" },
        { text: "Partner", value: "PARTNER_NAME" },
        { text: "Firma", value: "SUBCOMP_LABEL" },
        { text: "Ref.Tutar", value: "MASTER_TRAN_AMOUNT_R" },
        { text: "Tutar", value: "MASTER_TRAN_AMOUNT" },
        { text: "Birim", value: "SUBCURR_LABEL" },
        { text: "Hesap Türü", value: "SUBACC_LABEL" },
        { text: "Dönem", value: "MASTER_TRAN_DONEM" },
      ],
    };
  },

  methods: {
    ...mapActions([
      'loadFaturaDetay',
    ]),
    expandRow(row) {
      this.tranItems.map((item, index) => {
        item.selected = item === row
        this.$set(this.tranItems, index, item)
      })
      this.loadFaturaDetay(row.MASTER_TRAN_ID)
      
      this.$router.push({name: 'FaturaDetay', params: {id : row.MASTER_TRAN_ID  }})    

    }
  
  
  },

  mounted() {
    this.$store.dispatch("loadTranList");
  },
  computed: {
    ...mapState(["tranItems"]),
  },
};
</script>

<style>
h1 {
color:gray;
}
</style>