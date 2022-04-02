<template>
  <header>
    <nav class='container'>
      <a class='logo' href='https://medods.ru/'>
        <span>M</span>
        <span>E</span>
        <span>D</span>
        <span>O</span>
        <span>D</span>
        <span>S</span>
      </a>
    <div class="nav-toggle"><span></span></div>

      <ul class="menu">
        <li><a href="">О нас</a></li>
        <li><a href="">Портфолио</a></li>
      </ul>
    </nav>
    
  </header>
</template>

<script>
//let promise = new Promise(function(resolve, reject)=> )
  let promise = new Promise((resolve)=> {
    setTimeout( () => {
        let nav = document.querySelector('.nav-toggle');
        resolve(nav);  
    })
  })
  promise.then(nav => {
    nav.onclick= function() {
      let menu = document.querySelector('.menu');
      menu.classList.toggle('active'); 
    };
  },
  error => {
    alert('Rejected' + error);
  });
</script>



<style lang="sass" scoped>
header
   width: 100%;
   height: 50px;
   background: rgb(142, 227, 248);
   box-shadow: 3px 3px 1px rgba(0, 0, 0, .05);
   padding: 15px 0;
   margin-bottom: 15px;
   position: relative;

.logo 
   display: block;
   float: left;

.logo span 
   color: rgb(255, 255, 255);
   display: inline-block;
   width: 30px;
   height: 30px;
   line-height: 30px;
   border-radius: 50%;
   margin: 5px 0;
   text-align: center;
   text-shadow: 2px 2px 1px rgba(0, 0, 0, .4);

.logo span:nth-child(odd) 
   background: #0a93d3;

.logo span:nth-child(even) 
   background: #ffb85a;

.menu 
   float: right;

.menu li 
   display: inline-block;
   margin-right: 30px;

.menu a 
   color: #016897;
   text-transform: uppercase;
   letter-spacing: 1px;
   font-weight: 900;
   display: block;
   line-height: 20px;

.menu a:hover 
   color: #EF5A42;


.nav-toggle 
   display: none;
   position: relative;
   float: right;
   width: 40px;
   height: 40px;
   margin-right: 20px;
   background: #0a93d3;
   cursor: pointer;

.nav-toggle span 
   display: block;
   position: absolute;
   top: 20px;
   left: 8px;
   right: 8px;
   height: 2px;
   background: white;

.nav-toggle span:before, .nav-toggle span:after 
   content: "";
   position: absolute;
   display: block;
   left: 0;
   width: 100%;
   height: 2px;
   background: white;

.nav-toggle span:before 
   top: -10px;

.nav-toggle span:after 
   bottom: -10px;

.menu.active 
   max-height: 100px;

@media (max-width: 768px) 
   .nav-toggle
      display: block;

   header 
      padding: 10px 0;
   
   .menu 
      max-height: 0;
      background: rgb(250, 233, 187);
      float: none;
      position: absolute;
      overflow: hidden;
      top: 70px;
      right: 0;
      left: 0;
      margin: 0;
      padding: 0;
      z-index: 3;
   
   .menu li 
      display: block;
      text-align: center;
      border-bottom: 1px solid #ffffff;
      margin-right: 0;
   
   .menu a 
   line-height: 30px;

@media (max-width: 480px) 
   header 
      padding: 50px 0;
   
   .logo 
      float: none;
      margin: 0 auto 15px;
      display: table;

   .logo span 
      margin: 100px, 100px;

   .menu 
      top: 150px;
</style>
