
<script>
 let LeftArrow = "images/left-arrow.svg"
 window.addEventListener("load", function () {
  // Get the container element
var btnContainer = document.getElementById("menu");

 // Get all buttons with class="btn" inside the container
 var btns = btnContainer.getElementsByClassName("btn");
 
 // Loop through the buttons and add the active class to the current/clicked button
 
 for (var i = 0; i < btns.length; i++) {
   btns[i].addEventListener("click", function () {
     var current = document.getElementsByClassName("active");
     current[0].className = current[0].className.replace("active", "");
     this.className += " active";
   });
  }
    });

</script>
<div class="menu-container">
    <div id="menu" class="menu">
      <button class="btn active"> <a href="#one">HOME <span>	<img src='{LeftArrow}'   > </span> </a></button>
      <button class="btn"> <a href="#three">WORKS<span>	<img src='{LeftArrow}'   ></span> </a></button>
      <button class="btn"> <a href="#about">ABOUT<span>	<img src='{LeftArrow}'   ></span> </a> </button>
  
      <button class="btn"> <a href="#four">CONTACT<span>	<img src='{LeftArrow}'   > </span></a> </button>
    </div>
  </div>

  <style>

    button {
      font-family: "Barlow";
	font-size:16px;
	font-weight: 100;
    }
  </style>