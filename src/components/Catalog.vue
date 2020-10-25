<template>
  <div>
    <div>
      <b-form-group
        label-cols-sm="1"
        label-cols-lg="1"
        label="Filtrar"
        label-for="filterInput"
        class="p-2 m-0"
      >
        <b-input-group>
          <b-form-input
            v-model="filter"
            type="search"
            id="filterInput"
            placeholder="Digite para filtrar"
          ></b-form-input>
          <b-input-group-append>
            <b-button :disabled="!filter" @click="filter = ''">Limpar</b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form-group>
    </div>
    <div>
      <b-table
        selectable
        :sticky-header="'calc(100vh - 55px)'"
        responsive
        :items="pokeList"
        :fields="fields"
        :sort-by.sync="sortBy"
        :sort-desc.sync="sortDesc"
        :striped="true"
        :outlined="true"
        :head-variant="'dark'"
        :filter="filter"
        :select-mode="'single'"
        @row-selected="onRowSelected"
      >
        <template #cell(selected)="{ rowSelected }">
          <template v-if="rowSelected">
            <span aria-hidden="true">&check;</span>
            <span class="sr-only">Selected</span>
          </template>
          <template v-else>
            <span aria-hidden="true">&nbsp;</span>
            <span class="sr-only">Not selected</span>
          </template>
        </template>
      </b-table>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pokeList: Array,
  },
  data() {
    return {
      sortBy: "id",
      sortDesc: false,
      fields: [
        { key: "id", sortable: true },
        { key: "name", label: "Nome", sortable: true },
        { key: "typesToString", label: "Tipo", sortable: true },
      ],
      filter: null,
      filterOn: [],
      selected: [],
    };
  },
  methods: {
    onRowSelected(items) {
      this.selected = items;
      this.$emit("row-selected", items[0]);
    },
  },
};
</script>

<style scoped>
td {
  text-transform: capitalize;
}
</style>