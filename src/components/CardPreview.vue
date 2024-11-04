<!-- components/CardPreview.vue -->
<template>
  <div class="preview">
    <h2>Preview</h2>
    <div class="business-card" :style="cardStyle">
      <div class="card-header">
        <h3 class="name">{{ cardData.name }}</h3>
        <img v-if="cardData.logo" :src="cardData.logo" alt="Logo" class="logo">
      </div>
      <p class="job-title">{{ cardData.jobTitle }}</p>
      <p class="company">{{ cardData.company }}</p>
      <div class="contact-info">
        <p class="email">{{ cardData.email }}</p>
        <p class="phone">{{ cardData.phone }}</p>
      </div>
      <div class="card-footer">
        <p class="website">{{ cardData.website }}</p>
        <img v-if="cardData.qrCode" :src="generateQRCode()" alt="QR Code" class="qr-code">
      </div>
      <div v-if="hasSocialMedia" class="social-media">
        <a v-for="(link, platform) in cardData.socialMedia" :key="platform" :href="link" target="_blank">
          {{ platform }}
        </a>
      </div>
    </div>
    <div class="download-options">
      <button @click="downloadCard('png')" class="download-btn">Download as PNG</button>
      <button @click="downloadCard('pdf')" class="download-btn">Download as PDF</button>
    </div>
  </div>
</template>

  
  <script>
  import QRCode from 'qrcode';
  
  export default {
    name: 'CardPreview',
    props: ['cardData'],
    computed: {
      cardStyle() {
        return {
          backgroundColor: this.cardData.bgColor,
          color: this.cardData.textColor,
          fontFamily: this.cardData.font,
        };
      },
      hasSocialMedia() {
        return Object.values(this.cardData.socialMedia).some(link => link !== '');
      }
    },
    methods: {
      downloadCard(format) {
        this.$emit('download-card', format);
      },
      generateQRCode() {
        const data = `BEGIN:VCARD
  VERSION:3.0
  N:${this.cardData.name}
  ORG:${this.cardData.company}
  TITLE:${this.cardData.jobTitle}
  TEL:${this.cardData.phone}
  EMAIL:${this.cardData.email}
  URL:${this.cardData.website}
  END:VCARD`;
        
        let qrCodeDataURL = '';
        QRCode.toDataURL(data, { width: 100, height: 100 }, (err, url) => {
          if (!err) qrCodeDataURL = url;
        });
        return qrCodeDataURL;
      }
    }
  };
  </script>
  
  <style scoped>
  .preview {
    position: fixed;
    top: 100px;
    right: 20px;
    width: 600px;
    background-color: #f0f0f0;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    padding: 20px;
  }
  
  h2 {
    color: #333;
    margin-bottom: 15px;
  }
  
  .business-card {
    width: 100%;
    aspect-ratio: 16 / 9;
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    position: relative;
    overflow: hidden;
    transition: transform 0.3s ease;
  }
  
  .business-card.size-small {
    transform: scale(0.8);
  }

  .business-card.size-large {
    transform: scale(1.2);
  }

  .business-card.size-extra-large {
    transform: scale(1.4);
  }

  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 15px;
  }
  
  .name {
    font-size: 28px;
    font-weight: bold;
    margin: 0;
  }
  
  .logo {
    max-width: 60px;
    max-height: 60px;
    object-fit: contain;
  }
  
  .job-title {
    font-size: 18px;
    text-align: center;
    margin-bottom: 15px;
  }
  .company {
    font-size: 20px;
    text-align: center;
    margin-bottom: 15px;
    font-weight: 600;
  }
  .contact-info {
    display: flex;
    justify-content: space-between;
    font-size: 14px;
    margin-bottom: 15px;
  }
  
  .card-footer {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    position: absolute;
    bottom: 20px;
    left: 20px;
    right: 20px;
  }
  
  .website {
    font-size: 16px;
    margin: 0;
  }
  
  .qr-code {
    width: 55px;
    height: 60px;
  }
  
  .social-media {
    position: absolute;
    bottom: 50px;
    left: 20px;
    font-size: 12px;
  }
  
  .social-media a {
    margin-right: 10px;
    color: inherit;
    text-decoration: none;
  }
  
  .download-options {
    margin-top: 20px;
    display: flex;
    justify-content: space-between;
  }
  
  .download-btn {
    background-color: #4a90e2;
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
    transition: background-color 0.3s ease;
  }
  
  .download-btn:hover {
    background-color: #3a7bc8;
  }
  </style>