<template>
    <ul class="deal-list">
      <li v-for="deal in sortedDeals" :key="deal.dealID" class="deal-item">
      <a :href="deal.link" target="_blank">
    <div class="deal-image">
      <img :src="deal.thumb" :alt="deal.title" />
    </div>
    <div class="deal-details">
      <span class="deal-title">{{ deal.title }}</span>
      <div class="deal-discount">
      <div class="valores">
      <div class="valores2">
      <span class="preço-normal">${{ deal.normalPrice }}</span>
      <span class="desconto">${{ Math.min(Math.abs(calculateDiscount(deal).discountAmount - deal.normalPrice, 100)).toFixed(2) }}</span>
      </div>                  
      <span class="porcetagem">-{{parseInt(calculateDiscount(deal).discountPercentage) }}%</span>
      </div>
      <button @click.prevent="redirectToDealSite(deal.dealID)">Detalhes</button>
      </div>
    </div>
  </a>
</li>
    </ul>
</template>
<style scoped>

</style>