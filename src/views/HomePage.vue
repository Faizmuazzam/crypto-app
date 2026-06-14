<template>
  <ion-page>
    <ion-content class="ion-padding bg-white">
      <div class="container">
        <button class="refresh-btn" @click="getCryptos">Refresh</button>

        <div class="crypto-table">
          <div v-for="coin in cryptos" :key="coin.id" class="crypto-row">
            <div class="rank">
              <small>Rank</small>
              <strong>{{ coin.rank }}</strong>
            </div>

            <div class="info">
              <small>{{ coin.name }}</small>
              <strong>{{ coin.symbol }}</strong>
            </div>

            <div class="price">
              <small>USD</small>
              <strong>
                {{ Number(coin.price_usd).toFixed(2) }}
              </strong>
            </div>
          </div>
        </div>
      </div>

      <ion-loading :is-open="loading" message="Loading..." />
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { IonPage, IonContent, IonLoading } from "@ionic/vue";

interface Crypto {
  id: string;
  rank: number;
  name: string;
  symbol: string;
  price_usd: string;
}

const cryptos = ref<Crypto[]>([]);

const loading = ref(false);

const getCryptos = async () => {
  try {
    loading.value = true;
    const response = await fetch("https://api.coinlore.net/api/tickers/");
    const result = await response.json();
    cryptos.value = result.data;
  } catch (error) {
    console.error(error);
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  getCryptos();
});
</script>

<style scoped>
.bg-white {
  --background: #ffffff;
}
.container {
  max-width: 350px;
  margin: 40px auto;
}

.refresh-btn {
  display: block;
  margin: 0 auto 15px;
  background: #0057d8;
  color: white;
  border: none;
  padding: 14px 24px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.crypto-table {
  border: 1px solid #d4c089;
  background: #f7e8b4;
  color: #000;
}

.crypto-row {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #d4c089;
  padding: 8px;
}

.crypto-row:last-child {
  border-bottom: none;
}

.rank,
.info,
.price {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.rank {
  width: 50px;
  text-align: center;
}

.info {
  flex: 1;
}

.price {
  width: 150px;
}

small {
  font-size: 11px;
}

strong {
  font-size: 20px;
}
</style>
