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
        <img src="images/Answers in a bottle.png" style="width:100%">
        <div class="text">Caption Text</div>
      </div>
  
      <div class="mySlides fade">
        <div class="numbertext">2 / 7</div>
        <img src="images/DARK FUTURE.png" style="width:100%">
        <div class="text">Caption Two</div>
      </div>
  
      <div class="mySlides fade">
        <div class="numbertext">3 / 7</div>
        <img src="images/LYNN.png" style="width:100%">
        <div class="text">Caption Three</div>
      </div>
  
      <div class="mySlides fade">
        <div class="numbertext">4 / 7</div>
        <img src="images/ICARUS.png" style="width:100%">
        <div class="text">Caption Three</div>
      </div>
      <div class="mySlides fade">
        <div class="numbertext">5 / 7</div>
        <img src="images/DRIFTER.png" style="width:100%">
        <div class="text">Caption Three</div>
      </div>
      <div class="mySlides fade">
        <div class="numbertext">6 / 7</div>
        <img src="images/THE DIVISION.png" style="width:100%">
        <div class="text">Caption Three</div>
      </div>
      <div class="mySlides fade">
        <div class="numbertext">7 / 7</div>
        <img src="images/SiGIL.png" style="width:100%">
        <div class="text">Caption Three</div>
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
 
  
  </style>