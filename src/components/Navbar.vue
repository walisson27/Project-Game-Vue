<template>
 <nav class="navbar">
    <img class="navbar-img" src="../assets/logo.png" alt="">
 </nav>
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
@media (min-width: 768px){
.navbar{
    display: flex;
    justify-content: start;
}
.navbar-img{
  display: flex;
  margin-left: 620px;
}
}   


</style>