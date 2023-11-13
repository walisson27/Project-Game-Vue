<template>
  <body>
    <main>
      <section class="section-main">
        <h1>Ofertas</h1>
        <section class="sub-section">
          <head class="head-bar">
            <article class="search-bar">
              <input v-model="searchTerm" @input="fetchDeals" type="text" placeholder="Procura" />
              <div class="search-icon">
                <img src="../assets/search_24px.svg" alt="Ícone de Busca" />
              </div>
            </article>
          </head>
          <aside class="filter">
            <label for="sortOrder">Ordenar por:</label>
            <select v-model="sortOption" id="sortOrder">
              <option value="desc_discount">% de Desconto</option>
              <option value="asc_price">Menor Valor</option>
              <option value="desc_price">Maior Valor</option>
              <option value="asc_title">Título do Jogo (A-Z)</option>
              <option value="desc_title">Título do Jogo (Z-A)</option>
            </select>
          </aside>
        </section>
        <div v-if="errorMessage" class="error-message">
          {{ errorMessage }}
        </div>
        <ul v-if="!errorMessage" class="deal-list">
          <li v-for="deal in sortedDeals" :key="deal.dealID"  class="deal-item">
            <a :href="deal.link" target="_blank">
              <figure class="deal-image">
                <img :src="deal.thumb" :alt="deal.title" />
              </figure>
              <div class="deal-details">
                <h2 class="deal-title">{{ deal.title }}</h2>
                <div class="deal-discount">
                  <div class="deal-ajust">
                    <div class="sub-ajust">
                      <span class="preço-normal">${{ deal.normalPrice }}</span>
                      <span class="desconto">${{ Math.min(Math.abs(calculateDiscount(deal).discountAmount - deal.normalPrice, 100)).toFixed(2) }}</span>
                    </div>                  
                    <span class="porcentagem">-{{ parseInt(calculateDiscount(deal).discountPercentage) }}%</span>
                  </div>
                  <button class="button-detalhes" @click.prevent="openDealSite(deal.dealID)">Detalhes</button>
                </div>
              </div>
            </a>
          </li>
        </ul>
      </section>
      <button @click="loadMoreDeals" class="load-more-button">Carregar mais</button>
    </main>
  </body>
</template>
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
        .then((response) => {
          if (!response.ok) {
            throw new Error(`Erro na requisição: ${response.status}`);
          }
          return response.json();
        })
        .then((data) => {
          if (this.pageNumber === 0) {
            this.deals = data;
          } else {
            this.deals = this.deals.concat(data);
          }

          // Se não há resultados, exibe mensagem de erro
          if (data.length === 0) {
            this.errorMessage = "Nenhum resultado encontrado.";
          } else {
            this.errorMessage = null;
          }
        })
        .catch((error) => {
          console.error("Erro na requisição:", error.message);
          this.errorMessage = "Erro ao buscar ofertas. Tente novamente mais tarde.";

          this.deals = [];
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


<style scoped>

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

.sub-section{
    display: flex;
    margin-bottom: 37px;
    justify-content: space-between;
    align-items: flex-end
}

.head-bar{
  display: flex;
  flex-direction: column;
}

main{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
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

.deal-ajust{
  display: flex;
  flex-direction: row;
  margin-bottom: 10px;
}
.sub-ajust{
  display: flex;
  flex-direction: column;
  margin-right: 10px;
}

.desconto{
  font-size: 1rem;
  line-height: 21px;
  font-weight: 700;
  text-align: right;
}
.porcentagem{
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
}

 @media (max-width: 1194px  ) {
    body {
      background: linear-gradient(45deg, #0b1641 0%, #c70160 100%);
    }
    h1{
      display: flex;
      justify-content: center;
      color: #FFFFFF;
      font-family: Roboto;
      font-size: 18px;
      font-weight: 300;
      line-height: 21.09px;
      margin-top: 22px;
    }
    select{
      font-size: 14px;
    }
    .search-bar {
      width: 174px;
      margin-left: 8px;
      /*height: 36px;*/
    }

    input {
      width: 100%;
    }

    .filter {
      flex-direction: column;
      align-items: flex-start;
      width: 122px;
      margin-right: 8px;
      /*height: 68px;*/
    }

    select {
      width: 100%;
      margin-top: 10px; 
    }

    .deal-image img{
      width: 304px;
      height: 147px;
    }
    .deal-list {
      grid-template-columns: 1fr;
    }
    .deal-details{
    background: #0B1641;
    width: 304px;
    height: 47px;
    margin-top: 7px
    }
    .deal-item {
      width: 304px;
      margin-left: 8px;
      margin-right: 8px;
    }
    .load-more-button{
      width: 304px;
    }

  }

  
/*@media only screen and (max-width: 600px) {
    body {
      background: linear-gradient(45deg, #0b1641 0%, #c70160 100%);
      width: 100%;
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
      margin-top: 10px; /* Ajuste conforme necessário 
    }

    .ajuste {
      flex-direction: column;
      align-items: flex-start;
      margin-bottom: 20px;  Ajuste conforme necessário 
    }

    .deal-list {
      grid-template-columns: 1fr;
    }

    .deal-item {
      width: 380px;
    }
  }

  @media (max-width: 768px) {
  main {
    width: 960px;
    max-width: 960px;
  }

  h1 {
    text-align: start;
    margin-top: 20px;
  }

  nav {
    gap: 40%;
    margin: 30px 0px;
  }

  .list {
    grid-template-columns: 1fr 1fr 1fr;
    gap: 10px;
  }
}*/

</style>
