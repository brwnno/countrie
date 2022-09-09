<template>
  <q-header
    elevated
    class="toolbar"
    :style="mode ? 'background: #2b3743' : 'background: white '"
  >
    <q-toolbar class="text">
      <q-toolbar-title
        :class="mode ? 'text-white ' : 'text-black'"
        class="texts q-pa-lg"
        >Onde no mundo ?</q-toolbar-title
      >
      <q-btn
        class="btn"
        :color="mode ? 'white ' : 'black'"
        flat
        align="between"
        :icon="mode ? 'fa-solid fa-sun' : 'fa-solid fa-moon'"
        :label="mode ? 'Modo claro' : 'Modo escuro'"
        @click="$emit('darkMode')"
      />
    </q-toolbar>
  </q-header>
</template>
<script lang="ts">
import { Vue, Component, Ref, Watch } from 'vue-property-decorator';

import { Dark } from 'quasar';

@Component({
  components: { Dark },
})
export default class Header extends Vue {
  get mode() {
    console.log(this.$q.dark.isActive);
    return this.$q.dark.isActive;
  }
}
</script>
<style lang="scss" scoped>
.toolbar {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  justify-content: center;
  align-content: center;
  align-items: center;

  .activeDarkMode {
    background: hsl(0, 0%, 98%);
  }
  .disableDarkMode {
    background: white;
  }
}
.text {
  display: flex;
  flex-wrap: wrap;
  width: 80%;
  align-content: center;
  align-items: center;

  .texts {
    font-size: clamp(0.9rem, 1.7vw, 1.5rem);
  }
}

.btn {
  width: min(200px, calc(51% + 4px));
  font-size: clamp(0.7rem, 1.2vw, 0.9rem);
}
</style>
