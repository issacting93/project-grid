
<script>
/**
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
  */
  
</script>
<div class="menu-container">
    <div id="menu" class="menu">
      <button class="btn active"> <a href="#one">HOME <span>◀</span> </a></button>
      <button class="btn"> <a href="#two">ABOUT<span>◀</span> </a> </button>
      <button class="btn"> <a href="#three">WORKS<span>◀</span> </a></button>
      <button class="btn"> <a href="#four">CONTACT<span>◀</span></a> </button>
    </div>
  </div>