<template>
  <div class="app">
    <div class="card">
      <h1 class="title">Simple QR Code Generator</h1>
      <div class="input-container">
        <label for="url">Enter URL:</label>
        <input
          type="text"
          id="url"
          v-model="url"
          placeholder="Enter URL..."
          @keyup.enter="generateQRCode"
        />
        <button @click="generateQRCode">Get QR Code</button>
      </div>
      <div v-if="errorMsg" class="error-msg">{{ errorMsg }}</div>
      <div class="qr-code-container">
        <img v-if="qrCode" :src="qrCode" alt="QR Code" class="qr-code-img" />
        <p v-if="displayedUrl" class="url-display">{{ displayedUrl }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import QRCode from "qrcode"

export default {
  data() {
    return {
      url: window.location.href, // User input URL
      displayedUrl: window.location.href, // Finalized URL for display
      qrCode: null,
      errorMsg: null,
    }
  },
  mounted() {
    this.generateQRCode() // Generate QR code on component mount
  },
  methods: {
    async generateQRCode() {
      if (!this.url.trim()) {
        this.errorMsg = "Please enter a valid URL."
        this.qrCode = null // Clear any existing QR code
        return
      }
      this.errorMsg = null // Reset error message
      this.displayedUrl = this.url // Update displayed URL only when QR code is generated
      try {
        this.qrCode = await QRCode.toDataURL(this.url, {
          width: 512,
          margin: 2,
        })
      } catch (error) {
        console.error("Failed to generate QR code:", error)
        this.errorMsg = "An error occurred while generating the QR code."
        this.qrCode = null // Clear QR code on error
      }
    },
  },
}
</script>

<style scoped>
.app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: linear-gradient(
    135deg,
    #111,
    #222
  ); /* Dark gradient background */
}

.card {
  background-color: #2c2c2c;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.8); /* Enhanced shadow for depth */
  width: 90%;
  max-width: 800px; /* Responsive max-width */
}

.title {
  margin-bottom: 2rem;
  color: #66ccff;
  text-align: center;
  font-size: 24px;
}

.input-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 2rem;
}

.input-container label {
  margin-right: 1rem;
  color: #fff;
}

.input-container input {
  flex-grow: 1;
  padding: 0.75rem;
  border-radius: 6px;
  border: none;
  background-color: #444;
  color: #fff;
  margin-right: 1rem;
  transition: background-color 0.3s ease; /* Smooth transition for interaction */
}

.input-container input:focus {
  background-color: #555; /* Lighter on focus */
  outline: none;
}

.input-container button {
  padding: 0.75rem 1rem;
  border-radius: 6px;
  border: none;
  background-color: #66ccff;
  color: #333;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.4);
}

.input-container button:hover {
  background-color: #5abaff;
  transform: translateY(-2px);
}

.error-msg {
  color: #ff6666;
  margin-bottom: 1rem;
  text-align: center;
}

.qr-code-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 1rem;
  min-height: 256px; /* Allocate space for QR code */
}

.qr-code-container > p {
  margin-top: 0.8rem;
  color: #fff;
  font-size: 0.8rem;
}

.qr-code-img {
  width: 90%;
  max-width: 256px; /* Max width for QR code */
  height: auto;
}

.url-display {
  word-break: break-word; /* Break long words (like URLs) at any point */
  overflow-wrap: break-word; /* Ensure wrapping for older browsers */
  text-align: center; /* Optional: center-align the URL */
  color: #fff; /* Ensure it's readable */
  max-width: 100%; /* Limit width to container size */
  margin-top: 0.5rem; /* Add spacing above the text */
  font-size: 0.9rem; /* Adjust font size if needed */
  line-height: 1.4; /* Improve readability for multiple lines */
}
</style>
