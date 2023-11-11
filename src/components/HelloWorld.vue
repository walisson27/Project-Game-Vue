<template>
  <body>
  <div class="app-container">
  <nav class="navbar">
    <img class="navbar-img" src="../assets/logo.png" alt="">
  </nav>
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
      <label for="sortOrder">Ordenar por:</label>
      <select v-model="sortOption" id="sortOrder">
        <option value="" selected disabled>Selecione...</option>
        <option value="asc_price">Menor Valor</option>
        <option value="desc_price">Maior Valor</option>
        <option value="asc_title">Título do Jogo (A-Z)</option>
        <option value="desc_title">Título do Jogo (Z-A)</option>
        <option value="desc_discount">Maior Desconto</option>
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
      <button @click.prevent="redirectToDealSite(deal.dealID)">Detalhes</button>
      </div>
    </div>
  </a>
</li>
    </ul>
    <button @click="loadMoreDeals" class="load-more-button">Carregar mais ofertas</button>
  </div>
  </body>
  <footer>
    <div class="container">
      <a href="/" class="logo-footer">
        <img src="../assets/Vector.png" alt="Logo Game Tracker" />
      </a>
      <p>
        © 2021, Game Tracker Inc. Todos os direitos reservados. Nulla facilisi.
        Etiam sagittis congue tempor. Pellentesque habitant morbi tristique
        senectus et netus et malesuada fames ac turpis egestas. Suspendisse eu
        velit et ante luctus egestas. Aenean sed nisi tellus. Duis non ornare
        libero. Quisque suscipit maximus nisi, in gravida felis. Cras ornare
        mauris ac nisi congue, ut condimentum sem tincidunt. Phasellus semper
        tellus malesuada turpis gravida cursus. Duis nec eleifend nunc, vitae
        finibus elit. Aliquam eget diam vitae purus suscipit viverra.
      </p>

      <ul class="footer-ul">
        <li>
          <a href="#">Termos de Serviço</a>
        </li>
        <li>
          <a href="#">Política de Privacidade</a>
        </li>
        <li>
          <a href="#">Trabalhe conosco</a>
        </li>
        <li>
          <a href="#">Contato</a>
        </li>
      </ul>
    </div>
  </footer>
</template>

<style scoped>
.navbar{
  display: flex;
  align-items: center;
  width: 100%;
  height: 50px;
  background: #0B1641;
}

.navbar-img{
  display: flex;
  margin-left: 130px;
}

h1{
  display: flex;
  color: #FFFFFF;
  font-size: 36px;
  margin-top: 22px;
  margin-bottom: 22px;
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
  margin-right: 609px;
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
  background-color:#0B1641;
  border-radius: 8px;
  border: none;
  color: #fff;
  height: 50px;
  font-weight: 100;
  padding: 10px;
  font-size: 12px;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  width: 100%;
}


.ajuste{
  display: flex;
  justify-content: space-around;
  align-items: flex-end;
  margin-bottom: 37px;
}

.ajuste2{
  display: flex;
  flex-direction: column;
}

.app-container{
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
  width: 380px;
  height: 251px;
  background: #0B1641;
  border-radius: 8px;
  margin-left:5px;
}

.deal-image img{
  width: 380px;
  height: 147px;
  border-radius: 8px;
}

.deal-details{
  background: #0B1641;
  width: 380px;
  height: 47px;
  margin-bottom: 20px;
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
footer {
  background-color:#0B1641;
  min-height: 311px;
}

.container {
  max-width: 1200px;
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 5% 5%;
}

p {
  text-align: center;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.7rem;
}

.logo-footer {
  margin: 0 auto;
}

ul {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-decoration: none;
  list-style: none;
  font-size: 1rem;
  font-weight: 700;
  line-height: 2rem;
}

a{
    text-decoration: none;
    color: white
}

@media (min-width: 767px) {
  .container {
    margin: 0 auto;
  }

  p {
    text-align: justify;
  }

  ul {
    flex-direction: row;
    gap: 20px;
  }
}

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
    redirectToDealSite(deal) {
      window.open(deal.link, '_blank'); // Abre o site do item em uma nova guia
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
