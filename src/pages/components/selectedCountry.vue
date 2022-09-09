<template>
  <div class="container">
    <header class="button q-pb-md">
      <q-btn
        class="btn"
        :style="mode ? 'background: #2b3743' : 'background: white '"
        label="Voltar"
        @click="$emit('active')"
        icon="fa-solid fa-arrow-left"
      />
    </header>

    <article class="content">
      <div class="item">
        <div>
          <img class="img" :src="chosenParents.flags.png" />
        </div>

        <div>
          <h4>{{ chosenParents.name?.common }}</h4>

          <div class="sub">
            <div>
              <p><b>Nome nativo:</b>{{ NativeName }}</p>
              <p><b>População:</b> {{ chosenParents.population }}</p>
              <p><b>Região: </b> {{ chosenParents.region }}</p>
              <p><b>Subegião: </b> {{ chosenParents.subregion }}</p>
              <p><b>Capital: </b> {{ Capital }}</p>
            </div>

            <div>
              <p><b> domínio de nível superior:</b> {{ Tld }}</p>
              <p><b>moedas:</b> {{ Currencies }}</p>

              <p>
                <b> línguas:</b>
                <span
                  v-for="languages in chosenParents.languages"
                  :key="languages"
                >
                  {{ languages.length > 1 ? languages + ',' : languages }}
                </span>
              </p>
            </div>
          </div>

          <div class="NativeName">
            <b> Fronteiras: </b>
            <span v-for="borders in chosenParents.borders" :key="borders">
              <q-chip square>{{ borders }}</q-chip>
            </span>
          </div>
        </div>
      </div>
    </article>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import { iCountrie } from '../interface/iCountrie';
@Component
export default class selectedCountry extends Vue {
  @Prop()
  chosenParents!: iCountrie;

  get NativeName() {
    const nativeName = this.chosenParents.name!.nativeName;
    const value: any = Object.values(nativeName);

    return value[0].common;
  }

  get Currencies() {
    const currencies = this.chosenParents.currencies!;
    const value: any = Object.values(currencies);
    return value[0].name;
  }

  get Capital() {
    const capital = this.chosenParents!.capital;
    const value = Object.values(capital);
    return value[0];
  }

  get Tld() {
    const tld = this.chosenParents!.tld;
    const value = Object.values(tld);
    return value[0];
  }

  get mode() {
    console.log(this.$q.dark.isActive);
    return this.$q.dark.isActive;
  }

  beforeMount() {
    console.log('chosenParents', this.chosenParents);
  }
}
</script>

<style lang="scss" scoped>
.button {
  display: flex;
  justify-content: flex-start;
  width: 100%;
  padding-bottom: 50px;
}

.container {
  display: flex;
  justify-content: flex-start;
  width: 100%;
  flex-wrap: wrap;
  align-items: center;
  gap: 10px;
  min-width: 250px;
  flex-direction: column;
  margin-bottom: 20px;

  .content {
    display: flex;
    justify-content: flex-start;
    width: 100%;
    min-width: 250px;
    flex-wrap: wrap;
    flex-direction: column;

    .item {
      display: flex;
      flex-direction: row;
      width: 100%;
      gap: 60px;
      justify-content: flex-start;
      flex-wrap: wrap;
      align-items: center;
    }
    .sub {
      display: flex;
      width: 100%;
      min-width: 250px;
      flex-wrap: wrap;
      gap: 0 100px;
    }

    .NativeName {
      width: 100%;
      flex-wrap: wrap;
      margin: 30px 0;
    }

    .img {
      width: clamp(400px, 20vw, 45vw);
      height: clamp(400px, 20vw, 45vw);
    }
  }
}
</style>
