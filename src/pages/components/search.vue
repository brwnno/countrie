<template>
  <span class="boxs">
    <q-input
      outlined
      unelevated
      placeholder="Pesquisar pelo nome de um país ou pela capital"
      @click="$emit('click')"
      v-model="_search"
      class="input"
    >
      <template v-slot:prepend>
        <q-icon name="search" />
      </template>
      <template v-slot:append>
        <q-btn
          round
          color="grey-7"
          flat
          icon="close"
          dense
          @click="$emit('clear')"
        />
      </template>
    </q-input>

    <q-select
      clearable
      outlined
      color="grey-7"
      v-model="model"
      :options="options"
      label="filtrar por região"
      map-options
      emit-action
      class="select"
      @input="$emit('input', model)"
    />
    <small
      class="small q-pa-md"
      :style="mode ? 'color: white' : 'color: gray'"
      v-if="model || _search.length > 0"
      >{{ Cont }}</small
    >
  </span>
</template>

<script lang="ts">
import { Vue, Component, Prop, PropSync } from 'vue-property-decorator';

@Component
export default class search extends Vue {
  @PropSync('search')
  _search!: string;

  @Prop()
  count!: number;

  model = '';

  options = [
    {
      label: 'Africa',
      action: 'Africa',
    },
    {
      label: 'Americas',
      action: 'Americas',
    },
    {
      label: 'Asia',
      action: 'Asia',
    },
    {
      label: 'Europe',
      action: 'Europe',
    },
    {
      label: 'Oceania',
      action: 'Oceania',
    },
  ];

  get Cont() {
    return this.count > 1
      ? 'Foram encontrados ' + this.count + ' Países'
      : 'Foi encontrado ' + this.count + ' País';
  }

  get mode() {
    console.log(this.$q.dark.isActive);
    return this.$q.dark.isActive;
  }
}
</script>

<style lang="scss" scoped>
.boxs {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  gap: 10px;
  justify-content: space-between;
  align-content: flex-start;
  align-items: flex-start;
  min-height: 170px;
  max-height: 150px;

  .input {
    display: flex;
    width: 50%;
    min-width: 250px;
    justify-content: flex-start;
  }
  .select {
    display: flex;
    width: 20%;
    min-width: 180px;
    justify-content: flex-end;
  }

  .small {
    font-weight: 500;
    font-size: 14px;

    display: flex;
    width: 100%;
    justify-content: flex-start;
  }
}
</style>
