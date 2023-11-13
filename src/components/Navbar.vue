<template>
 <nav class="navbar">
      <img class="navbar-img" src="../assets/logo.png" alt="">
 </nav>
</template>
<style scoped>
/*.navbar{
  display: flex;
  align-items: center;
  width: 100%;
  height: 50px;
  background: #0B1641;
}

.navbar-img{
  display: flex;
}*/
  .navbar {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 80px; /* Ajuste a altura conforme necessário */
    background-color: #0B1641; /* Cor de fundo do navbar */
  }

  .navbar-img {
    position: absolute;
    max-width: 100%; /* Ajuste a largura máxima conforme necessário */
    max-height: 100%; /* Ajuste a altura máxima conforme necessário */
  }


/*@media (min-width: 768px){
.navbar{
    display: flex;
    justify-content: start;
}
.navbar-img{
  display: flex;
  margin-left: 620px;
}
}
@media only screen and (max-width: 320px) {
.navbar{
    display: flex;
    align-items: center;
    justify-content: center;
    
    }
  }*/

</style>