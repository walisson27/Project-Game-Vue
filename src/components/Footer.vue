<template>
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

<style>
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
@media only screen and (min-width: 320px) {

  ul {
    gap: 8px;
  }
}

</style>