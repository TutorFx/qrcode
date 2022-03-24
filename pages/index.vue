<template>
<div class="light-bg fill-height">
  <div class="d-flex align-center justify-center fill-height">
    <v-card width="100%" class="window">
      <v-card-title class="d-flex justify-space-between light-bg--text px-lg-8">Gerador de QR Code             
        <div>
          <v-btn elevation="0" :disabled="value == ''" @click="downloadQrCode()">
            <v-icon>mdi mdi-download</v-icon> PNG
          </v-btn>
        </div>
      </v-card-title>
      <v-divider></v-divider>
      <div class="editorContainer">
        <v-container class="fill-height">
        <v-row align="center">
          <v-col cols="12" lg="6">
            <Vue-qrcode
              class="qr-canvas"
              ref="qrcode"
              :value="value == '' ? 'www.gabrielserejo.com' : value"
              :options="options"
            /> 
            <v-divider class="d-lg-none d-block"></v-divider>
          </v-col>
          <v-col cols="12" lg="6">
            <v-container>
              <v-row class="pa-3">
                <v-col cols="12" lg="7">
                  <v-text-field
                    placeholder="ex: www.google.com.br"
                    class="mb-n6"
                    label="URL"
                    filled
                    v-model="value"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" lg="5">
                  <v-slider
                    class="my-3"
                    max="2500"
                    min="600"
                    v-model="options.width"
                    step="200"
                    ticks
                    :label="`${options.width}px`"
                  ></v-slider>
                </v-col>
                <v-col cols="12" lg="6">
                  <v-color-picker
                    v-model="options.color.dark"
                    dot-size="25"
                    width="360px"
                  ></v-color-picker>
                </v-col>
                <v-col cols="12" lg="6">
                  <v-color-picker
                    v-model="options.color.light"
                    dot-size="25"
                    width="360px"
                  ></v-color-picker>
                </v-col>
              </v-row>
            </v-container>
          </v-col>
        </v-row>
      </v-container>
      </div>
    </v-card>
  </div>
</div>
</template>

<script>
import VueQrcode from "@chenfengyuan/vue-qrcode";
import { saveAs } from "file-saver";
export default {
  components: {
    VueQrcode,
  },
  data() {
    return {
      value: "",
      options: {
        width: 600,
        color: {
          dark: "#000",
          light: "#ffffff00",
        },
      },
      sizeDebounce: 300,
    };
  },
  methods: {
    downloadQrCode(format = "png") {
      var canvas = document.querySelector("canvas");
      const link = this.value;
      canvas.toBlob(function (blob) {
        saveAs(blob, `QR ${link}.${format}`);
      });
    },
  },
};
</script>

<style lang="scss">
@import "~vuetify/src/styles/settings/_variables.scss";
.qr-canvas {
  max-width: 100% !important;
  height: auto !important;
}
.editorContainer {
  overflow: auto;
  height: calc(100vh - 300px);
}
.window {
  max-width: 400px;
  @media #{map-get($display-breakpoints, 'xs-only')} {
    max-width: 100%;
  }
  @media #{map-get($display-breakpoints, 'lg-and-up')} {
    max-width: 1200px!important;
  }
}
@media #{map-get($display-breakpoints, 'xs-only')} {
  .editorContainer {
    height: calc(100vh - 70px);
  }
}
</style>
