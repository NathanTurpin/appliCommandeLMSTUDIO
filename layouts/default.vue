<template>
  <div id="bodyPage">
    <head>
      <script
        src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta1/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-ygbV9kiqUc6oa4msXn9868pTtWMgiQaeYH7/t7LECLbyPA2x65Kgf80OJFdroafW"
        crossorigin="anonymous"
      ></script>
    </head>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <div class="container-fluid">
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarTogglerDemo01"
          aria-controls="navbarTogglerDemo01"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarTogglerDemo01">
          <a class="navbar-brand" href="#">
            <img src="../static/iconLogo.png" class="icon-logo" alt="" width="50" height="40" />
          
          </a>
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
               
              <a class="nav-link active " aria-current="page" href="#"> <h4>Appli Commande</h4>  </a>
            </li>
            <li class="nav-item">
              <a class="nav-link " href="#"><h4>Nos produits</h4> </a>
            </li>
           <li class="nav-item">
              <a class="nav-link " href="#"><h4>A propos</h4> </a>
            </li>
          </ul>
          
        </div>
      </div>
    </nav>
    <aside class="sidenav">
      <div class="img-presentation">
        <img src="../static/resto.jpg" class="img-fluid" alt="" />
        <h2 class="titre-presentation">LOREM IPSUM</h2>
        <p>
          Lorem, ipsum dolor sit amet consectetur adipisicing elit. Earum
          blanditiis laboriosam repellat sequi ducimus. Vel veniam fugiat, eius,
          consequuntur doloremque, cum quae dicta obcaecati neque ea harum
          voluptate architecto dolorum!
        </p>
        <button class="btn-presentation btn btn-outline-secondary">Reserver</button>
      </div>
    </aside>
  <section class="panier">
    <button class="btn btn-outline-success"> Panier</button>
  </section>
    <Nuxt />
  </div>
</template>

<script>
export default {};
</script>

<style>

.navbar-brand {
  margin-right:3%
}
.sidenav {
  min-width: 20%;
  height: 100%;
  position: fixed;
  z-index: 1;
  background: #eee;
  overflow-x: hidden;
  padding: 8px 0;
}
.img-presentation {
  top: 10%;
  position: absolute;
  padding: 2%;
}
.main {
  margin-left: 21%; /* Same width as the sidebar + left position in px */
  /* Increased text to enable scrolling */
  padding: 0px 10px;
}

.panier {
  position: fixed;
  z-index: 2;
  top:90%;
  left:90%;
}
@media screen and (max-width: 550px) {
  /* .sidenav {visibility: hidden} */
  .main {
    padding: 0px 0px;
  }
}
</style>
