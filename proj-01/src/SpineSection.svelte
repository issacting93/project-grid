<script>
	import SpineUnitLeft from './SpineUnitLeft.svelte';
    import SpineUnitRight from './SpineUnitRight.svelte';
    
   


</script>
<section class="section-03" id="three">

    <div class="slideshow-container">
  
      <div class="mySlides fade">
        <div class="numbertext">1 / 7</div>
        <img src="https://www.w3schools.com/howto/img_nature_wide.jpg" style="width:100%">
        <div class="text">Caption Text</div>
      </div>
  
      <div class="mySlides fade">
        <div class="numbertext">2 / 7</div>
        <img src="https://www.w3schools.com/howto/img_fjords_wide.jpg" style="width:100%">
        <div class="text">Caption Two</div>
      </div>
  
      <div class="mySlides fade">
        <div class="numbertext">3 / 7</div>
        <img src="https://www.w3schools.com/howto/img_mountains_wide.jpg" style="width:100%">
        <div class="text">Caption Three</div>
      </div>
  
      <div class="mySlides fade">
        <div class="numbertext">4 / 7</div>
        <img src="https://www.w3schools.com/howto/img_mountains_wide.jpg" style="width:100%">
        <div class="text">Caption Three</div>
      </div>
      <div class="mySlides fade">
        <div class="numbertext">5 / 7</div>
        <img src="https://www.w3schools.com/howto/img_mountains_wide.jpg" style="width:100%">
        <div class="text">Caption Three</div>
      </div>
      <div class="mySlides fade">
        <div class="numbertext">6 / 7</div>
        <img src="https://www.w3schools.com/howto/img_mountains_wide.jpg" style="width:100%">
        <div class="text">Caption Three</div>
      </div>
      <div class="mySlides fade">
        <div class="numbertext">7 / 7</div>
        <img src="https://www.w3schools.com/howto/img_mountains_wide.jpg" style="width:100%">
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
        <li class="spine-unit dot" onmouseover=" showSlides(this);" data-image-number="1">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot" onmouseover=" showSlides(this);" data-image-number="2">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot" onmouseover=" showSlides(this);" data-image-number="3">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot" onmouseover=" showSlides(this);" data-image-number="4">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot" onmouseover=" showSlides(this);" data-image-number="5">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot" onmouseover=" showSlides(this);" data-image-number="6">
          <span>⚀</span>
        </li>
        <li class="spine-unit dot" onmouseover=" showSlides(this);" data-image-number="7">
          <span>⚀</span>
        </li>
      </ul>
  
    </div>
    <div class="select-text"> Select Memory </div>
  </section>