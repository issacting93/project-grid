 <script>

   import Arrow from "./arrow.svelte";
   import MouseTracking from "./ParallaxComponent.svelte";
   import Blob from "./Blob.svelte";
 </script>

<section class="hero-section" id="one">
<div class="hero-interactive"> 
	<h2 class="hero-text hero-text-01 top"><span> MY NAME IS ISSAC </span> </h2 >
	<h2 class="hero-text   hero-text-02" > <span>  if you’ll indulge me </span></h2>
	<h2 class="hero-text  hero-text-03  "> <span> I’d Like to design the Future </span>  </h2>
 
  <div class="hero-text-01 bottom"> ACCESS PORTFOLIO 
    <a href="#three"><div class="arrow">
  <Arrow/></div></a>
  </div>
</div>
  <MouseTracking />
 
</section>
   

<style>
 .hero-text {
	 color:white;
} 
.hero-interactive {
    margin-top: 30px;
}
.hero-text {
	 
    height: fit-content;
    vertical-align: middle;
    justify-content: center;
    align-content: center;
    flex-direction: column;
    display: flex;
    align-items: center;
    text-align: center; 
    z-index: 11;
    text-transform: uppercase;
    position: absolute;
    left: 0px;
    right: 0px;
	padding: 20px;
    transform: translate(0, -50%);
	letter-spacing: 0.4em;
	font-weight: 100;
	line-height: 36px;
}
 

.hero-text-01 {
	color:white;
	text-align: center;

	position: absolute;
	text-align: center;
	left: 0px;
	right: 0px;
	letter-spacing: 0.4em;
	z-index: 10;

	height: fit-content;
    vertical-align: middle;
    justify-content: center;
    align-content: center;
    flex-direction: column;
    font-family: "Barlow";
   
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
	margin: auto;
	font-weight: 100;
  opacity:0;
  animation: fadein 3s ease-in forwards;
  animation-delay: 3s;
}


.hero-text-01 span,
.hero-text-02 span,
.hero-text-03 span {
  width: 100%;
  float: left;
  font-family: "Barlow";
  font-size:16px;
  font-weight: 100;
  color: #ffffff;
  -webkit-clip-path: polygon(100% 0, 100% 100%, 0 100%, 0 80%);
  clip-path: polygon(100% 0, 100% 100%, 0 100%, 0 80%);
  transform: translateY(-50px);
  opacity: 0;
  animation-name: titleAnimation;
  animation-timing-function: ease;
  animation-duration:3s;
}

.hero-text-01 span {
  animation-delay: 0.6s;
  animation-fill-mode: forwards;
  -webkit-animation-fill-mode: forwards;
}
.hero-text-01 span:first-child {
  animation-delay: 0.7s;
}
.hero-text-01 span:last-child {
 
  animation-delay: 0.5s;
}

.hero-text-02 {
 
  font-weight: 100; 
}
.hero-text-02 span {
  animation-delay: 3.1s;
  animation-fill-mode: forwards;
  -webkit-animation-fill-mode: forwards;
}
.hero-text-02 span:first-child {
  animation-delay: 3.2s;
}
.hero-text-02 span:last-child {
  animation-delay: 3.5s;
}

.hero-text-03 {
   
  position: absolute;
  font-weight: 600;
}
.hero-text-03 span {
  animation-delay: 6.1s;
  animation-fill-mode: forwards;
  -webkit-animation-fill-mode: forwards;
  font-weight: 600;
}
.hero-text-03 span:first-child {
  animation-delay: 6.2s;
}
.hero-text-03 span:last-child {
  
  animation-delay: 6s;
}
@media screen and (max-width: 800px) {

  .hero-text-01.bottom{
    margin-bottom: 30%;
    animation-delay: 1s;
}
}

@keyframes titleAnimation {
  0% {
    transform: translateY(-50px);
    opacity: 0;
    -webkit-clip-path: polygon(100% 0, 100% 100%, 0 100%, 0 80%);
    clip-path: polygon(100% 0, 100% 100%, 0 100%, 0 80%);
  }
  20% {
    transform: translateY(0);
    opacity: 1;
    -webkit-clip-path: polygon(100% 0, 100% 100%, 0 100%, 0 15%);
    clip-path: polygon(100% 0, 100% 100%, 0 100%, 0 15%);
  }
  80% {
    transform: translateY(0);
    opacity: 1;
    -webkit-clip-path: polygon(100% 0, 100% 100%, 0 100%, 0 15%);
    clip-path: polygon(100% 0, 100% 100%, 0 100%, 0 15%);
  }
  100% {
    transform: translateY(50px);
    opacity: 0;
    -webkit-clip-path: polygon(100% 0, 100% 0%, 0 100%, 0 100%);
    clip-path: polygon(100% 0, 100% 0%, 0 100%, 0 100%);
  }
}
</style>