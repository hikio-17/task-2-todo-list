<template>
  <div class="container">
    <ul class="menu">
      <li>
        <router-link to="/">
          Home
        </router-link>
      </li>
      <li>
        <router-link to="/about">
          About
        </router-link>
      </li>
      <li>
        <router-link to="/contact">
          Contact
        </router-link>
      </li>
    </ul>
    <h1> Untuk Pengalaman Belajar Menyenangkan Hubungi Kang Arya</h1>
    <ul class="contact">
      <li>Facebook</li>
      <li>Instagram</li>
      <li>Whatsapp</li>
    </ul>
  </div>
</template>

 <style scoped>
     .container {
       width: 75%;
       margin: 100px auto;
       text-align: center;
     }

     .menu {
       list-style: none;
       margin-bottom: 20px;
     }

     .menu li {
       display: inline;
     }

     .menu li a {
       text-decoration: none;
       padding: 20px;
     }
 </style>
