<script>
	import SpineUnitLeft from './SVGSpineUnitLeft.svelte';
  import SpineUnitRight from './SVGSpineUnitRight.svelte';
  import ScifiFrame from './SVGscifiFrame.svelte';
   
  let visible = false;

  function typewriter(node, { speed = 5 }) {
    const valid = (
      node.childNodes.length === 1 &&
      node.childNodes[0].nodeType === Node.TEXT_NODE
    );

    if (!valid) {
      throw new Error(`This transition only works on elements with a single text node child`);
    }

    const text = node.textContent;
    const duration = text.length / (speed * 0.01);

    return {
      duration,
      tick: t => {
        const i = ~~(text.length * t);
        node.textContent = text.slice(0, i);
      }
    };
  }

window.addEventListener("load", function () {
// Get the container element
var spinebtnContainer = document.getElementById("spine-controller");

// Get all buttons with class="btn" inside the container
var btnSpine = spinebtnContainer.getElementsByClassName("spine-unit");

// Loop through the buttons and add the active class to the current/clicked button
for (var i = 0; i < btnSpine.length; i++) {
  btnSpine[i].addEventListener("mouseover", function () {
    var current = document.getElementsByClassName(" active");
    current[0].className = current[0].className.replace(" active", "");
     this.className += " active";
  });
}

});

var slideIndex = 0;



export function showSlides() {
  var i; 
  //console.log('area element id = ' + id);

  var imageNumber = this.getAttribute("data-image-number");
  //alert("The " + ele.innerHTML + " is a " + imageNumber + ".");
  var slides = document.getElementsByClassName("mySlides");
  var dots = document.getElementsByClassName("dot");
  slides[imageNumber - 1].style.display = "none";
  
  for (i = 0; i < slides.length; i++) {
    slides[i].style.display = "none";
  } 
 
  slides[imageNumber - 1].style.display = "block";
 
 
} 
 
</script>
<section class="section-03" id="three">
  <div class="title"> WORKS </div>
  <div class="frame-container">   <ScifiFrame /></div>
  <div class="text-container">
    <!--
    <div class="top-left-el">
      <div class="title"> Portfolio </div>
      <div class="sub-title">27-21012-2421</div>
    
    </div>

    <div class="el-01">
      <div class="ripple"></div>
      {#if visible}
      <div class="connecting-01" >
        <span in:typewriter out:typewriter>
          connecting... 
        </span>
          
       </div>{/if}
    </div>-->
  </div>
    <div class="slideshow-container">
  
      <div class="mySlides fade">
        <div class="numbertext">1 / 7</div>
        <img src="images/STEALTH.png" style="width:100%" alt="Stealth game concept">
        <div class="text"> <a href="  https://www.behance.net/gallery/112545991/Pyke-Stealth-game"> STEALTH GAME CONCEPT </a> </div>
      </div>
     
      <div class="mySlides fade">
        <div class="numbertext">2 / 7</div>
        <img src="images/DARK FUTURE.png" style="width:100%" alt = "Dark Future">
        <div class="text"> <a href="  https://www.behance.net/gallery/71387117/Dark-Future"> DARK FUTURE </a> </div>
      </div>
    
      <div class="mySlides fade">
        <div class="numbertext">3 / 7</div>
        <img src="images/LYNN.png" style="width:100%" alt="lynn">
        <div class="text"> <a href="https://zacting.itch.io/lynn"> LYNN </a> </div>
      </div>
     
      <div class="mySlides fade">
        <div class="numbertext">4 / 7</div>
        <img src="images/ICARUS.png" style="width:100%" alt='Icarus'>
        <div class="text"> <a href= "https://www.behance.net/gallery/99721631/STEAMPUNK-HUD-DESIGNS"> ICARUS </a> </div>
      </div>
      <div class="mySlides fade">
        <div class="numbertext">5 / 7</div>
        <img src="images/DRIFTER.png" style="width:100%" alt="drifter"> 
        <div class="text"> <a href= "https://www.youtube.com/watch?v=yK0yUYrHmXQ"> DRIFTER </a> </div>
      </div>
      <div class="mySlides fade">
        <div class="numbertext">6 / 7</div>
        <img src="images/THE DIVISION.png" style="width:100%" alt="The Division Fan art">
        <div class="text"> <a href="https://www.behance.net/gallery/108280961/DIVISION-III-Fan-art">The DIVISION (Fan Art) </a> </div>
      </div>
      <div class="mySlides fade">
        <div class="numbertext">7 / 7</div>
        <img src="images/Black Monarch.png" style="width:100%" alt="Black Monarch">
        <div class="text"> <a href="https://www.behance.net/gallery/108694895/The-Black-Monarch">The Black Monarch </a> </div>
      </div>
  
    </div>
    <div class="spine-container" id="spine-controller">
      <div class=" ">
  
        <div class="spine">
          <ul class="spine-left">
            
            <li> 
                <SpineUnitLeft />
            </li> 
            <li> <SpineUnitLeft />
            </li>    
            <li>
                <SpineUnitLeft />
             </li>    
            <li>
                <SpineUnitLeft /> 
            </li>   
            <li>
                <SpineUnitLeft /> 
            </li>    
            <li>
                <SpineUnitLeft /> 
            </li>    
            <li>
                <SpineUnitLeft />
            </li> 
          </ul>
          <ul class="spine-right">
            <li> 
                <SpineUnitRight/> </li>
             <li> 
                <SpineUnitRight/> </li> 
             <li> 
                <SpineUnitRight/> </li> 
             <li> 
                <SpineUnitRight/>  </li>
             <li> 
                <SpineUnitRight/> </li> 
             <li> 
                <SpineUnitRight/> </li>
             <li>
                <SpineUnitRight/> 
            </li> 
          </ul>
        </div>
      </div>
      <ul class="spine-controller-ul dot-nav">
        <li class="spine-unit dot" on:mouseenter={showSlides}   data-image-number="1">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot"  on:mouseenter={showSlides}   data-image-number="2">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot" on:mouseenter={showSlides}  data-image-number="3">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot"  on:mouseenter={showSlides}  data-image-number="4">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot"  on:mouseenter={showSlides}  data-image-number="5">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot"  on:mouseenter={showSlides}  data-image-number="6">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot" on:mouseenter={showSlides}  data-image-number="7">
          <span>⚀</span>
        </li>
      </ul>
  
    </div>
    <!-- 
      
    <div class="select-text"> Select Memory </div>

    -->
  </section>

  <style>
  .frame-container{
    height:100%;
    position:absolute;
    left:0px;
    right:0px;
    top:0px;
    bottom:0px;
    opacity: .6;
}
.spine-unit{
  transition:all 1s;
}
.section-03:hover .frame-container{
  display: block;
}
.section-03 .frame-container{
  display: none;
}

.frame-container:hover{
  
    opacity: .1;
}
.text{
  z-index: 11;
  text-transform: capitalize;
}

.text a {
    background: rgba(1,1,1,0.5);
    width: fit-content;
    margin: auto;
    padding: 10px;
    border: 1px solid white;
}
.text:hover a {
  font-weight:600;
  background: white;
  color:black;
}
 

@media only screen and (max-width: 800px) {
  #spine-controller {
    transform: scale(0.5) translate(0px, -100px) ;
}
#spine-controller:hover {
  transform: scale(0.5) translate(0px, -100px) ;
}

.spine-container:hover .spine-controller-ul li{
  transform: scale(2) ;
    margin-top: 75px;
    
}
.text{
   font-size: 24px;
    position: absolute;
    vertical-align: middle;
    left: 0px;
    right: 0px;
    text-align: center;
    display: inline-grid;
    align-items: baseline;
    height: auto;
    align-content: space-evenly;
  
}
.text a {
    background: rgba(1,1,1,0.5);
    width: fit-content;
    margin: auto;
    padding: 10px;
    border: 1px solid white;
}
}

  </style>