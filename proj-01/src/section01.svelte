 <script>

   import Arrow from "./arrow.svelte";
   import MouseTracking from "./3dMouseTracking.svelte";
 </script>

<section class="hero-section" id="one">
  
 <div class="hero-text-01 top"> MY NAME IS ISSAC</div>
 <div class="hero-text">
   
  <div class="" >if you’ll indulge me</div>
  <div class="strong-text">I’d Like to design the Future </div>
  <MouseTracking />
 </div>
 
  <div class="hero-text-01 bottom"> ACCESS PORTFOLIO 
    <a href="#about"><div class="arrow">
  <Arrow/></div></a>
  </div>
 
</section>
   

<style>
 .hero-text {
	color: white;
}

.hero-text {
	padding: 40px;
	text-align: center;
	height: 100%;
	vertical-align: middle;
	display: flex;
	justify-content: center;
	align-content: center;
	flex-direction: column;
	font-family: Barlow;
	font-style: normal;
	font-weight: normal;
	font-size: 24x;
	line-height: 36px;
	display: flex;
	align-items: center;
	text-align: center;
	letter-spacing: 0.1em;
	text-transform: uppercase;
}

.strong-text {
	font-weight: bold;
	font-size: 24px;
	line-height: 36px;
	display: flex;
	align-items: center;
	text-align: center;
}

.hero-text-01 {
	color: white;
	text-align: center;
	margin: auto;
	position: absolute;
	text-align: center;
	left: 0px;
	right: 0px;
  letter-spacing: 0.4em;
}

.hero-text-01.top {
	top: 10vh;
}

.arrow {
	width: 54px;
	height: 54px;
	cursor: pointer;
}

.hero-text-01.bottom {
	display: flex;
	flex-direction: column;
	width: fit-content;
	bottom: 10vh;
	align-items: center;
}


</style>