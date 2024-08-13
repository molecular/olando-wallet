
<script setup lang="ts">
  import tokenItemNFT from './tokenItems/tokenItemNFT.vue'
  import tokenItemFT from './tokenItems/tokenItemFT.vue'
  import { useStore } from 'src/stores/store'

  const store = useStore()
</script>

<template>
  <div style="word-break: break-all; text-align: center;">
    Token receiving address:
  </div>
  <qr-code id="qrCode" :contents="store.wallet?.tokenaddr" 
    style="display: block; width: 230px; height: 230px; margin: 5px auto 0 auto; background-color: #fff;">
    <img :src="displayeBchQr? 'images/bch-icon.png':'images/tokenicon.png'" slot="icon" /> <!-- eslint-disable-line -->
  </qr-code>
  <div style="word-break: break-all; text-align: center;">
    <span @click="() => copyToClipboard(store.wallet?.tokenaddr)" style="cursor:pointer;">
      <span class="depositAddr">{{ store.wallet?.tokenaddr ?? "" }}</span>
      <img class="copyIcon" src="images/copyGrey.svg"> 
    </span>
  </div>
  <div v-if="store.nrBcmrRegistries == undefined" style="text-align: center;">Loading tokendata ...</div>
  <div v-if="store.tokenList?.length == 0" style="text-align: center;"> No tokens in this wallet </div>
  <div v-if="store.nrBcmrRegistries != undefined" :key="(store.tokenList?.[0]?.tokenId ?? '') + (store.tokenList?.length ?? 0)">
    <div v-for="tokenData in store.tokenList" :key="tokenData.tokenId.slice(0,6)">
      <tokenItemFT v-if="'amount' in tokenData" :tokenData="tokenData" :key="store.bcmrRegistries?.[tokenData.tokenId]?.name"/>
      <tokenItemNFT v-else :tokenData="tokenData" :key="store.bcmrRegistries?.[tokenData.tokenId]?.description"/>
    </div>
  </div>
</template>