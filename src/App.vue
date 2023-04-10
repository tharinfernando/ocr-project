<template>
  <div>
    <input type="file" @change="processImage" />
    <div>
      <img :src="preview.imageUrl" v-if="preview.imageUrl" />
      <div v-if="extractedText">{{ extractedText }}</div>
    </div>
  </div>
</template>

<script>
import Tesseract from "tesseract.js";

export default {
  data() {
    return {
      preview: {
        imageUrl: "",
      },
      extractedText: "",
    };
  },
  methods: {
    async processImage(event) {
      const file = event.target.files[0];
      this.preview.imageUrl = URL.createObjectURL(file);
      const result = await Tesseract.recognize(file);
      this.extractedText = result.text;
    },
  },
};
</script>
