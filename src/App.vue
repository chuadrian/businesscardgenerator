<template>
  <div id="app">
    <header>
      <h1 class="logo">BizGen</h1>
    </header>
    <main>
      <CardEditor 
        :cardData="cardData" 
        @update-card="updateCard"
        @upload-logo="handleLogoUpload"
      />
      <CardPreview 
        :cardData="cardData" 
        @download-card="downloadCard"
      />
    </main>
  </div>
</template>

<script>
import CardEditor from './components/CardEditor.vue';
import CardPreview from './components/CardPreview.vue';
import { jsPDF } from 'jspdf';
import html2canvas from 'html2canvas';

export default {
  name: 'App',
  components: {
    CardEditor,
    CardPreview
  },
  data() {
    return {
      cardData: {
        name: 'John Doe',
        jobTitle: 'Software Developer',
        company: 'Tech Solutions Inc.',
        email: 'john@example.com',
        phone: '+1 (555) 123-4567',
        website: 'www.johndoe.com',
        bgColor: '#ffffff',
        textColor: '#000000',
        font: 'Arial, sans-serif',
        logo: null,
        qrCode: false,
        socialMedia: {
          linkedin: '',
          twitter: '',
          github: ''
        }
      }
    };
  },
  methods: {
    updateCard(newData) {
      this.cardData = { ...this.cardData, ...newData };
    },
    handleLogoUpload(logo) {
      this.cardData.logo = logo;
    },
    downloadCard(format) {
      const card = document.querySelector('.business-card');
      html2canvas(card).then(canvas => {
        if (format === 'png') {
          const link = document.createElement('a');
          link.download = 'business-card.png';
          link.href = canvas.toDataURL();
          link.click();
        } else if (format === 'pdf') {
          const imgData = canvas.toDataURL('image/png');
          const pdf = new jsPDF({
            orientation: 'landscape',
            unit: 'px',
            format: [canvas.width, canvas.height]
          });
          pdf.addImage(imgData, 'PNG', 0, 0, canvas.width, canvas.height);
          pdf.save('business-card.pdf');
        }
      });
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Audiowide&display=swap');

:root {
  --primary-blue: #1e3d59;
  --secondary-blue: #3c6382;
  --accent-red: #ff6b6b;
  --dark-shade: #2c3e50;
  --light-blue: #f5f9ff;
  --gradient-blue: linear-gradient(135deg, var(--primary-blue), var(--secondary-blue));
}

body {
  margin: 0;
  padding: 0;
  font-family: 'Inter', sans-serif;
  background-color: var(--light-blue);
  min-height: 100vh;
  color: var(--dark-shade);
}

#app {
  min-height: 100vh;
  padding: 0;
  display: flex;
  flex-direction: column;
}

header {
  background: var(--gradient-blue);
  padding: 1rem 2rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.logo {
  margin: 0;
  color: white;
  font-family: 'Audiowide', cursive;
  font-size: 2.5em;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

main {
  display: flex;
  gap: 2rem;
  padding: 2rem;
  flex: 1;
  height: calc(100vh - 5rem);
}

/* Override CardEditor styles */
:deep(.editor) {
  width: 45% !important;
  margin-right: 0 !important;
  background: white;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  padding: 2rem !important;
  height: 100%;
  overflow-y: auto;
}

/* Override CardPreview styles */
:deep(.preview) {
  position: relative !important;
  top: 0 !important;
  right: 0 !important;
  width: 55% !important;
  background: white;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  padding: 2rem !important;
  height: 100%;
  overflow-y: auto;
}

:deep(.business-card) {
  transform: scale(1.2);
  transform-origin: top center;
  margin-top: 2rem;
}

:deep(.form-group) {
  margin-bottom: 1.5rem !important;
}

:deep(label) {
  color: var(--primary-blue) !important;
  font-weight: 600 !important;
  margin-bottom: 0.5rem !important;
}

:deep(input), :deep(select) {
  border: 2px solid #e1e8f0 !important;
  border-radius: 8px !important;
  padding: 0.75rem !important;
  transition: all 0.3s ease !important;
}

:deep(input:focus), :deep(select:focus) {
  border-color: var(--secondary-blue) !important;
  box-shadow: 0 0 0 3px rgba(60, 99, 130, 0.1) !important;
}

:deep(.download-btn) {
  background: var(--gradient-blue) !important;
  border-radius: 8px !important;
  padding: 0.75rem 1.5rem !important;
  font-weight: 600 !important;
  transition: transform 0.2s ease !important;
}

:deep(.download-btn:hover) {
  transform: translateY(-2px) !important;
}

:deep(h2) {
  color: var(--primary-blue) !important;
  font-size: 1.75rem !important;
  margin-bottom: 2rem !important;
}

@media (max-width: 1200px) {
  main {
    flex-direction: column;
    height: auto;
  }

  :deep(.editor),
  :deep(.preview) {
    width: 100% !important;
    height: auto;
  }

  :deep(.business-card) {
    transform: scale(1);
    margin-top: 1rem;
  }
}

/* Reset default styles */
* {
  box-sizing: border-box;
}

/* Accessibility improvements */
:focus {
  outline: none;
}

/* Print styles */
@media print {
  body * {
    visibility: hidden;
  }
  
  .business-card, .business-card * {
    visibility: visible;
  }
  
  .business-card {
    position: absolute;
    left: 0;
    top: 0;
  }
}
</style>