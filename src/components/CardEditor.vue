<!-- components/CardEditor.vue -->
<template>
    <div class="editor">
      <h2>Customize Your Card</h2>
      <div class="form-group" v-for="field in basicFields" :key="field.key">
        <label :for="field.key">{{ field.label }}</label>
        <input
          :id="field.key"
          :type="field.type"
          v-model="localCardData[field.key]"
          @input="emitUpdate"
        />
      </div>
      <div class="form-group">
        <label>Background Color</label>
        <input
          type="color"
          v-model="localCardData.bgColor"
          @input="emitUpdate"
        />
      </div>
      <div class="form-group">
        <label>Text Color</label>
        <input
          type="color"
          v-model="localCardData.textColor"
          @input="emitUpdate"
        />
      </div>
      <div class="form-group">
        <label for="font">Font</label>
        <select
          id="font"
          v-model="localCardData.font"
          @change="emitUpdate"
        >
          <option v-for="font in fonts" :key="font.value" :value="font.value">
            {{ font.label }}
          </option>
        </select>
      </div>
      <div class="form-group">
        <label for="logoUpload">Logo</label>
        <input
          id="logoUpload"
          type="file"
          @change="uploadLogo"
          accept="image/*"
        />
      </div>
      <div class="form-group">
        <label>
          <input
            type="checkbox"
            v-model="localCardData.qrCode"
            @change="emitUpdate"
          />
          Include QR Code
        </label>
      </div>
      <div class="social-media">
        <h3>Social Media</h3>
        <div class="form-group" v-for="platform in socialPlatforms" :key="platform">
          <label :for="platform">
            {{ platform.charAt(0).toUpperCase() + platform.slice(1) }}
          </label>
          <input
            :id="platform"
            v-model="localCardData.socialMedia[platform]"
            @input="emitUpdate"
          />
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'CardEditor',
    props: ['cardData'],
    data() {
      return {
        localCardData: { ...this.cardData },
        basicFields: [
          { key: 'name', label: 'Name', type: 'text' },
          { key: 'jobTitle', label: 'Job Title', type: 'text' },
          { key: 'company', label: 'Company', type: 'text' },
          { key: 'email', label: 'Email', type: 'email' },
          { key: 'phone', label: 'Phone', type: 'tel' },
          { key: 'website', label: 'Website', type: 'url' },
        ],
        fonts: [
          { value: 'Arial, sans-serif', label: 'Arial' },
          { value: '"Times New Roman", serif', label: 'Times New Roman' },
          { value: '"Courier New", monospace', label: 'Courier New' },
          { value: 'Georgia, serif', label: 'Georgia' },
          { value: '"Trebuchet MS", sans-serif', label: 'Trebuchet MS' },
        ],
        socialPlatforms: ['linkedin', 'twitter', 'github'],
      };
    },
    methods: {
      emitUpdate() {
        this.$emit('update-card', this.localCardData);
      },
      uploadLogo(event) {
        const file = event.target.files[0];
        if (file) {
          const reader = new FileReader();
          reader.onload = (e) => {
            this.$emit('upload-logo', e.target.result);
          };
          reader.readAsDataURL(file);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .editor {
    width: calc(100% - 440px);
    max-width: 600px;
    margin-right: 440px;
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  h2, h3 {
    color: #333;
    margin-bottom: 15px;
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
  }
  
  input[type="text"],
  input[type="email"],
  input[type="tel"],
  input[type="url"],
  select {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 14px;
  }
  
  input[type="color"] {
    width: 50px;
    height: 50px;
    padding: 0;
    border: none;
  }
  
  input[type="file"] {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 14px;
  }
  
  .social-media {
    margin-top: 20px;
  }
  
  @media (max-width: 1200px) {
    .editor {
      width: 100%;
      margin-right: 0;
      margin-bottom: 400px;
    }
  }
  </style>