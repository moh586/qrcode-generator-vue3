<template>
  <div>
    <div v-if="imageUrl" :class="myclass">
      <img :src="imageUrl" :class="imgclass" crossorigin="anonymous" />
    </div>
    <div v-if="download">
      <button @click="onDownloadClick" :class="downloadButton">
        {{ ButtonName }}
      </button>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, defineProps, onMounted, watch } from "vue";
import QRCodeStyling from "../core/QRCodeStyling";

const props = defineProps({
  width: {
    type: Number,
    default: 300
  },
  imgclass: {
    type: String,
    default: ""
  },
  myclass: {
    type: String,
    default: ""
  },
  downloadButton: {
    type: String,
    default: ""
  },
  ButtonName: {
    type: String,
    default: "Download3"
  },
  height: {
    type: Number,
    default: 300
  },
  margin: {
    type: Number,
    default: 0
  },
  value: {
    type: String,
    required: true
  },
  image: {
    type: String,
    default: ""
  },
  qrOptions: {
    type: Object,
    default: () => ({
      typeNumber: 0,
      mode: "Byte",
      errorCorrectionLevel: "Q"
    })
  },
  imageOptions: {
    type: Object,
    default: () => ({ hideBackgroundDots: true, imageSize: 0.4, margin: 0 })
  },
  dotsOptions: {
    type: Object,
    default: () => ({
      type: "dots",
      color: "#6a1a4c",
      gradient: {
        type: "linear",
        rotation: 0,
        colorStops: [
          { offset: 0, color: "#6a1a4c" },
          { offset: 1, color: "#6a1a4c" }
        ]
      }
    })
  },
  backgroundOptions: {
    type: Object,
    default: () => ({ color: "#ffffff" })
  },
  cornersSquareOptions: {
    type: Object,
    default: () => ({ type: "dot", color: "#000000" })
  },
  cornersDotOptions: {
    type: Object,
    default: () => ({ type: undefined, color: "#000000" })
  },
  fileExt: {
    type: String,
    default: "png"
  },
  download: {
    type: Boolean,
    default: false
  },

  downloadOptions: {
    type: Object,
    default: () => ({ name: "vqr", extension: "png" })
  }
});

const imageUrl = ref<string>("");
const qrCode = ref<QRCodeStyling>(new QRCodeStyling({
  data: props.value,
  width: props.width,
  height: props.height,
  margin: props.margin,
  qrOptions: props.qrOptions,
  imageOptions: props.imageOptions,
  dotsOptions: props.dotsOptions,
  backgroundOptions: props.backgroundOptions,
  image: props.image,
  cornersSquareOptions: props.cornersSquareOptions,
  cornersDotOptions: props.cornersDotOptions
}));


watch(() => ([props.value,props.dotsOptions,props.cornersSquareOptions,props.cornersDotOptions]), async () => {
  qrCode.value = new QRCodeStyling({
    data: props.value,
    width: props.width,
    height: props.height,
    margin: props.margin,
    qrOptions: props.qrOptions,
    imageOptions: props.imageOptions,
    dotsOptions: props.dotsOptions,
    backgroundOptions: props.backgroundOptions,
    image: props.image,
    cornersSquareOptions: props.cornersSquareOptions,
    cornersDotOptions: props.cornersDotOptions
  })

  imageUrl.value = await qrCode.value.getImageUrl(props.fileExt);
    });

const onDownloadClick = () => {
  qrCode.value.download(props.downloadOptions);
}


onMounted(async () => {
  imageUrl.value = await qrCode.value.getImageUrl(props.fileExt);
})

</script>
