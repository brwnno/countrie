<template>
  <q-layout view="lHh lpr lFf">
    <q-page-container class="q-mb-md">
      <headerCountrie @darkMode="modeDark = !modeDark" />
      <q-page class="q-mb-md">
        <main class="container">
          <article class="box q-pt-lg q-pb-lg" v-if="active">
            <Search
              :search.sync="search"
              @clear="clear()"
              @input="orderedBy = $event"
              class="q-mt-md q-mb-md"
              :count="SearchCountries.length"
            />
            <span class="div">
              <q-intersection
                v-for="(item, index) in SearchCountries"
                :key="index"
                transition="scale"
                class="intersection"
              >
                <q-card
                  class="card"
                  @click="openChosenParents(item)"
                  :style="mode ? 'background: #2b3743' : 'background: white '"
                >
                  <img :src="item.flags.png" />

                  <q-card-section class="section" v-if="!loading">
                    <div class="title">{{ item.name.common }}</div>
                    <div class="subtitle">
                      <b>População:</b> {{ item.population }}
                    </div>
                    <div class="subtitle">
                      <b>Região: </b> {{ item.region }}
                    </div>
                    <div class="subtitle">
                      <b>Capital: </b>
                      <span v-for="capital in item.capital" :key="capital">
                        {{ item.capital.length > 1 ? capital + ',' : capital }}
                      </span>
                    </div>
                  </q-card-section>

                  <q-card-section v-else>
                    <q-spinner-hourglass color="purple" size="4em" />
                  </q-card-section>
                </q-card>
              </q-intersection>
            </span>
          </article>

          <article class="box q-pt-lg" v-else>
            <selectedCountry
              :chosenParents="chosenParents"
              @active="openChosenParentsState()"
            />
          </article>
        </main>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import { Vue, Component, Ref, Watch } from 'vue-property-decorator';
import axios from 'axios';
import selectedCountry from './components/selectedCountry.vue';
import Search from 'pages/components/search.vue';
import headerCountrie from './components/header.vue';
import { Dark } from 'quasar';
import { iCountrie } from './interface/iCountrie';

@Component({
  components: { selectedCountry, Search, headerCountrie, Dark },
})
export default class PageIndex extends Vue {
  countries: iCountrie[] = [];
  orderedBy: { label: string; action: string } | null = null;
  search = '';
  active = true;
  modeDark = false;
  chosenParents = {};
  loading = true;

  get Countries() {
    return axios
      .get('https://restcountries.com/v3.1/all')
      .then((res) => {
        this.countries = res.data;
        this.loading = false;
      })
      .catch((error) => {
        console.log(error);
      });
  }

  order(countrie: boolean | iCountrie[]) {
    if (!this.orderedBy || typeof countrie === 'boolean') return countrie;

    switch (this.orderedBy.label) {
      case 'Africa':
        return countrie.filter(({ region }) => region === 'Africa');

      case 'Americas':
        return countrie.filter(({ region }) => region === 'Americas');

      case 'Asia':
        return countrie.filter(({ region }) => region === 'Asia');

      case 'Europe':
        return countrie.filter(({ region }) => region === 'Europe');

      case 'Oceania':
        return countrie.filter(({ region }) => region === 'Oceania');

      default:
        return countrie;
    }
  }

  get SearchCountries(): any {
    if (!this.search) return this.order([...this.countries]);

    return this.order(
      this.countries.filter((item) => {
        try {
          const searchName = new RegExp(this.search, 'gi').test(
            item.name.common!
          );
          const searchCapital = new RegExp(this.search, 'gi').test(
            item.capital!
          );

          return searchName || searchCapital;
        } catch (error) {
          return false;
        }
      })
    );
  }

  clear() {
    this.search = '';
  }

  openChosenParents(item: object) {
    this.active = false;
    this.$set(this, 'chosenParents', item);
  }

  openChosenParentsState() {
    this.active = true;
    this.$q.dark.set(this.active);
  }

  @Watch('modeDark')
  darkMode() {
    console.log('darkMode()');
    this.$q.dark.set(this.modeDark);
    this.$q.dark.toggle();
  }

  get mode() {
    console.log(this.$q.dark.isActive);
    return this.$q.dark.isActive;
  }

  beforeMount() {
    this.Countries;
  }
}
</script>

<style lang="scss" scoped>
@import url(https://fonts.google.com/specimen/Nunito+Sans);
* {
  font-family: 'Nunito,Sans';
}
.container {
  display: flex;
  justify-content: center;
  width: 100%;

  .box {
    display: flex;
    flex-wrap: wrap;
    width: 80%;
    align-content: center;
    align-items: center;
  }

  .intersection {
    display: flex;
    flex-wrap: wrap;
    align-content: center;
    align-items: center;
    width: 20%;
    min-width: 250px;

    flex-direction: row;
    height: 340px;
  }

  .div {
    display: flex;
    flex-direction: row;
    width: 100%;
    // gap: 20px;
    // justify-content: space-between;
    gap: 100px;
    justify-content: flex-start;
    flex-wrap: wrap;
    margin-bottom: 10px;
  }

  // .active {
  //   display: flex;
  //   flex-direction: row;
  //   width: 100%;
  //   gap: 100px;
  //   justify-content: flex-start;
  //   flex-wrap: wrap;
  // }

  .card {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    align-content: center;
    align-items: center;
    flex-direction: row;
    line-height: 1.7;
    height: 340px;

    &:hover {
      cursor: pointer;
      box-shadow: 0 0 1em gray;
    }

    .title {
      font-size: 16px;
      font-weight: 800;
      margin-bottom: 5px;
    }

    .subtitle {
      font-size: 14px;
      font-weight: 300;
    }

    img {
      height: 200px;
      border-bottom: 1px solid #cecece;
    }
  }
}
body.body--light {
  background: white;
}

body.body--dark {
  background: #2b3743;
}
</style>
