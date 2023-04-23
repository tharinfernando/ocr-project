<template>
  <div>
    <input type="file" @change="processImage" />
    <div>
      <img :src="preview.imageUrl" v-if="preview.imageUrl" />
      <div v-if="extractedText">{{ extractedText }}</div>
      <div class="copy-icon" @click="copyText">
        <CopyIcon class="w-2"/>
      </div>
    </div>
  </div>
</template>

<script>
import Tesseract from "tesseract.js";
import CopyIcon from "./components/Icons/CopyIcon.vue";

export default {
  name: "app",
  components: { 
    CopyIcon,
  },
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
  copyText() {
    const textarea = document.createElement("textarea");
    textarea.value = this.extractedText;
    document.body.appendChild(textarea);
    textarea.select();
    document.execCommand("copy");
    document.body.removeChild(textarea);
  },
},
};
</script>
