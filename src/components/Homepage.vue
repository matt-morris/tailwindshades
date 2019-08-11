<template>
  <div>

    <div class=" bg-theme-lighter py-4 mb-4 text-theme">
      <div class="container">
        <div class="flex justify-between">
          <div>
            <h1 class="text-3xl mb-0 leading-none tracking-wide text-white light:text-theme-darker">tailwindshades</h1>
            <p class="mt-0">
              A tool to help generate color shades for tailwindcss
            </p>
          </div>
          <div class="text-right mt-1 self-center">
            <button
              type="button"
              class="btn focus:outline-none"
              :class="config.theme === 'light' ? 'bg-blue-400 text-blue-100' : 'border border-theme-600'"
              @click="changeTheme('light')"
            >
              Light
            </button>
            <button
              type="button"
              class="btn focus:outline-none ml-2"
              :class="config.theme === 'dark' ? 'bg-blue-600 text-white' : 'border border-theme-600'"
              @click="changeTheme('dark')"
            >
              Dark
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="container text-theme">

      <div v-if="!colors.length">
        <chrome-picker v-model="selection" :disable-alpha="true"></chrome-picker>

        <pre>{{ selection.hex }}</pre>

        <!-- <label for="">paste hex color code (full)</label>
          <input type="text" v-model="hexinput"> -->

        <button class="btn btn-blue" @click="addColor">Generate</button>

        <!-- <br>
        <br>
        <button class="btn btn-red" @click="clear">Clear</button> -->
      </div>

      <div v-for="(color, i) in colors" :key="'color-' + i">
        <color-shades :color="color"/>
      </div>
    </div>

  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import { Chrome } from 'vue-color'
import ColorShades from '@/components/ColorShades'

export default {
  components: {
    ChromePicker: Chrome,
    ColorShades,
  },
  data() {
    return {
      selection: {},
      hexinput: '',
      colors: ['#6658dd'],
    }
  },
  computed: {
    ...mapGetters({
      config: 'config/config',
    })
  },
  methods: {
    clear() {
      this.colors = []
    },
    addColor() {
    // if (false === /^#[0-9A-F]{6}$/i.test(this.base)) {
      //   return
      // }
      if (!this.selection.hex) {
        return
      }
      // pass hex only
      this.colors.push(this.selection.hex)
    },
    changeTheme(theme) {
      this['config/changeTheme'](theme)
    },
    ...mapActions([
      'config/changeTheme'
    ])
  }
}
</script>