<template>
  <body>
  <main>
  <div class="ajustando">
    <div class="ajuste">
    <div class="ajuste2">
    <h1>Ofertas</h1>
    <div class="search-bar">
      <input v-model="searchTerm" @input="fetchDeals" type="text" placeholder="Pesquisar por título do jogo" />
      <div class="search-icon">
      <img src="../assets/search_24px.svg" alt="Search Icon" />
    </div>
    </div>
    </div>
    <div class="filter">
      <label  for="sortOrder">Ordenar por:</label>
      <select v-model="sortOption" id="sortOrder">
        <option value="desc_discount">% de Desconto</option>
        <option value="asc_price">Menor Valor</option>
        <option value="desc_price">Maior Valor</option>
        <option value="asc_title">Título do Jogo (A-Z)</option>
        <option value="desc_title">Título do Jogo (Z-A)</option>
      </select>
    </div>
  </div>
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
      <button @click.prevent="openDealSite(deal.dealID)">Detalhes</button>
      </div>
    </div>
  </a>
</li>
    </ul>
</div>
    <button @click="loadMoreDeals" class="load-more-button">Carregar mais</button>
  </main>
  </body>
</template>

<style scoped>

h1{
  display: flex;
  color: #FFFFFF;
  font-family: Roboto;
  font-size: 36px;
  font-weight: 300;
  line-height: 42px;
  letter-spacing: 0em;
  text-align: center;
}
body{
  background: linear-gradient(45deg, #0b1641 0%, #c70160 100%);

}
.search-bar{
  display: flex;
  position: relative;
  width: 380px;
  height: 50px;
  padding-top: 22px;
}

input {
  background-color:#0B1641;
  border-radius: 8px;
  border: none;
  color: #fff;
  font-weight: 100;
  padding: 10px;
  width: 100%;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  padding-left: 40px;
}

.search-icon {
  position: absolute;
  left: 9px;
  top: 68%;
  transform: translateY(-50%);
  color: rgba(255, 255, 255, 0.5);
}


.filter {
    display: flex;
    text-align: center;
    align-items: baseline;
}

.filter label {
  font-size: 16px;
  line-height: 20px;
  color: white;
  width: 100%;
}

select {
  display: flex;
  background-color:#0B1641;
  border-radius: 8px;
  border: none;
  color: #fff;
  height: 50px;
  width: 180px;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  font-family: Roboto;
  font-size: 18px;
  font-weight: 100;
  line-height: 21px;
  letter-spacing: 0em;
  text-align: left;

  
}

.ajuste{
    display: flex;
    margin-bottom: 37px;
    justify-content: space-between;
    align-items: flex-end
}

.ajuste2{
  display: flex;
  flex-direction: column;
}

main{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

ul{

}

.deal-list{
  display: grid;
  /*grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 277px;*/
  grid-template-columns: 1fr 1fr 1fr;
  gap: 10px;
}


.deal-item{
  display: flex;
  flex-direction: column;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  overflow: hidden;
  width: 380px;
  height: 251px;
  background: #0B1641;
  border-radius: 8px;
  margin-left:5px;
}

.deal-image img{
  width: 380px;
  height: 147px;
  border-radius: 8px 0px 0px 0px;
}

.deal-details{
  background: #0B1641;
  width: 380px;
  height: 47px;
  margin-top: 7px;
}
.deal-title{
  color: #FFFFFF;
  font-size: 1.4rem;
  font-weight: 300;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  margin-left: 16px;
}
.deal-price{
  display: flex;
  justify-content: flex-end;
}
.deal-discount{
  display: flex;
  flex-direction: row-reverse;
  justify-content: space-between;
  margin-left: 16px;
  margin-right: 16px;
  margin-top: 14px;
}

.preço-normal{
  font-size: 0.7rem;
    line-height: 14px;
    text-align: right;
    font-weight: 100;
    text-decoration: line-through;
}

.valores{
  display: flex;
  flex-direction: row;
  margin-bottom: 10px;
}
.valores2{
  display: flex;
  flex-direction: column;
  margin-right: 10px;
}
button{
  width: 116px;
  height: 39px;
  background-color:#C70160;;
  font-size: 18px;
  font-weight: 700;
  text-transform: uppercase;
  border-radius: 8px;
  color: #fff;
  /*margin-top: 14px;
  margin-bottom: 16px;*/
}
.desconto{
  font-size: 1rem;
  line-height: 21px;
  font-weight: 700;
  text-align: right;
}
.porcetagem{
    display: flex;
    align-items: center;
    justify-content: center;
    background-color:#16857B;
    font-size: 18px;
    font-weight: 500;
    text-transform: uppercase;
    border-radius: 8px;
    width: 84px;  
    height: 39px;
    /*margin-top: 14px;*/
  
}
.load-more-button{
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 40px auto;
    background-color:#0B1641;
    color: #FFFFFF;
    font-weight: 100;
    font-size: 16px;
    border: none;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
    border-radius: 8px;
    cursor: pointer;
    width: 380px;
    height: 50px;
}

@media only screen and (max-width: 600px) {
    body {
      background: linear-gradient(45deg, #0b1641 0%, #c70160 100%);
    }

    .search-bar {
      width: 100%;
    }

    input {
      width: 100%;
    }

    .filter {
      flex-direction: column;
      align-items: flex-start;
    }

    select {
      width: 100%;
      margin-top: 10px; /* Ajuste conforme necessário */
    }

    .ajuste {
      flex-direction: column;
      align-items: flex-start;
      margin-bottom: 20px; /* Ajuste conforme necessário */
    }

    .deal-list {
      grid-template-columns: 1fr;
    }

    .deal-item {
      width: 100%;
    }
  }
/*@media (min-width: 1024px) {
  main {
    width: 1080px;
    max-width: 1080px;
  }
}*/
</style>
<script>
export default {
  data() {
    return {
      deals: [],
      searchTerm: "",
      sortOption: "asc_price",
      pageNumber: 0,
      pageSize: 12,
    };
  },
  computed: {
    sortedDeals() {
    return this.deals.slice().sort((a, b) => {
      if (this.sortOption === "asc_price") {
        return parseFloat(a.salePrice) - parseFloat(b.salePrice);
      } else if (this.sortOption === "desc_price") {
        return parseFloat(b.salePrice) - parseFloat(a.salePrice);
      } else if (this.sortOption === "asc_title") {
        return a.title.localeCompare(b.title);
      } else if (this.sortOption === "desc_title") {
        return b.title.localeCompare(a.title);
      } else if (this.sortOption === "desc_discount") {
        return this.calculateDiscount(b).discountPercentage - this.calculateDiscount(a).discountPercentage;
      }
    });
  },
},
    sortedDeals() {
      return this.deals.slice().sort((a, b) => {
        if (this.sortOption === "asc_price") {
          return parseFloat(a.salePrice) - parseFloat(b.salePrice);
        } else if (this.sortOption === "desc_price") {
          return parseFloat(b.salePrice) - parseFloat(a.salePrice);
        } else if (this.sortOption === "asc_title") {
          return a.title.localeCompare(b.title);
        } else if (this.sortOption === "desc_title") {
          return b.title.localeCompare(a.title);
        }
      });
    },
  
  methods: {
    openDealSite(dealId) {
    const redirectUrl = `https://www.cheapshark.com/redirect?dealID=${dealId}`;
    window.open(redirectUrl, '_blank');
    
  },
    sortedDeals() {
    return this.deals.slice().sort((a, b) => {
      if (this.sortOption === "asc_price") {
        return parseFloat(a.salePrice) - parseFloat(b.salePrice);
      } else if (this.sortOption === "desc_price") {
        return parseFloat(b.salePrice) - parseFloat(a.salePrice);
      } else if (this.sortOption === "asc_title") {
        return a.title.localeCompare(b.title);
      } else if (this.sortOption === "desc_title") {
        return b.title.localeCompare(a.title);
      } else if (this.sortOption === "desc_discount") {
        return this.calculateDiscount(b).discountPercentage - this.calculateDiscount(a).discountPercentage;
      }
    });
  },

      calculateDiscount(deal) {
      const regularPrice = parseFloat(deal.normalPrice);
      const salePrice = parseFloat(deal.salePrice);

      if (!isNaN(regularPrice) && !isNaN(salePrice)) {
        const discountAmount = regularPrice - salePrice;
        const discountPercentage = (discountAmount / regularPrice) * 100;

        return {
          discountAmount: discountAmount.toFixed(2),
          discountPercentage: discountPercentage.toFixed(2),
        };
      } else {
        return {
          discountAmount: 0,
          discountPercentage: 0,
        };
      }
    },
    fetchDeals() {
      this.pageNumber = 0;
      this.loadDeals();
    },
    loadDeals() {
      const apiUrl = `https://www.cheapshark.com/api/1.0/deals?pageNumber=${this.pageNumber}&pageSize=${this.pageSize}&storeID=1&onSale=1&AAA=1${
        this.searchTerm ? `&title=${this.searchTerm}` : ""
      }`;
      fetch(apiUrl)
        .then((response) => response.json())
        .then((data) => {
          if (this.pageNumber === 0) {
            this.deals = data;
          } else {
            this.deals = this.deals.concat(data);
          }
        });
    },
    loadMoreDeals() {
      this.pageNumber++;
      this.loadDeals(); // Carrega mais dados e adiciona à lista de ofertas existente
    },
  },
  mounted() {
    this.loadDeals();
  },
};
</script>

