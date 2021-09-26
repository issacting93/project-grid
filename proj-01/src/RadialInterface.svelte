
<script>
	import ScifiMod01 from './SVGscifiMod01.svelte';
  import ScifiMod02 from './SVGscifiMod02.svelte';

 
</script>
<div class="radial-container rotating " id="two">
 
    <ul class="health-container ">
      <li class="li-container">
      <div> <ScifiMod01 /></div>   
        </li>
      <li class="li-container">
      <div> <ScifiMod01 /></div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod01 /></div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod01 /></div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod01 /> </div>   
      </li><li class="li-container">
        <div> <ScifiMod01 /> </div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod01 /> </div>   
      </li>
         <li class="li-container">
          <div> <ScifiMod01 /> </div>   
      </li>
    </ul>
    <ul class="health-container-01 ">
      <li class="li-container">
      <div> <ScifiMod02 /></div>   
        </li>
      <li class="li-container">
      <div> <ScifiMod02 /></div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod02 /></div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod02 /></div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod02 /> </div>   
      </li><li class="li-container">
        <div> <ScifiMod02 /> </div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod02 /> </div>   
      </li>
         <li class="li-container">
          <div> <ScifiMod02 /> </div>   
      </li>
    </ul>
  <div class="ring-01">  
    <svg width="100%" height="100%" viewBox="0 0 399 366" fill="none" xmlns="http://www.w3.org/2000/svg">
    <circle cx="199.267" cy="183" r="177.5" stroke="white" stroke-width="11"/>
    <circle cx="8.13333" cy="357.867" r="6.13333" fill="#C4C4C4" stroke="white" stroke-width="4"/>
    <circle cx="390.4" cy="357.867" r="6.13333" fill="#C4C4C4" stroke="white" stroke-width="4"/>
    <circle cx="8.13333" cy="8.13333" r="6.13333" fill="#C4C4C4" stroke="white" stroke-width="4"/>
    <circle cx="390.4" cy="8.13333" r="6.13333" fill="#C4C4C4" stroke="white" stroke-width="4"/>
    </svg>
  </div>
  <div class="ring-02">
    <svg width="100%" height="100%" viewBox="0 0 399 366" fill="none" xmlns="http://www.w3.org/2000/svg">
      <circle cx="199.267" cy="183" r="177.5" stroke="white" stroke-width="2"/>
      <circle cx="8.13333" cy="357.867" r="6.13333" fill="#C4C4C4" stroke="white" stroke-width="4"/>
      <circle cx="390.4" cy="357.867" r="6.13333" fill="#C4C4C4" stroke="white" stroke-width="4"/>
      <circle cx="8.13333" cy="8.13333" r="6.13333" fill="#C4C4C4" stroke="white" stroke-width="4"/>
      <circle cx="390.4" cy="8.13333" r="6.13333" fill="#C4C4C4" stroke="white" stroke-width="4"/>
      </svg>
   </div>
  <div class="ring-03">
    <svg width="280" height="281" viewBox="0 0 280 281" fill="none" xmlns="http://www.w3.org/2000/svg">
      <circle cx="8.5" cy="140.5" r="8" stroke="white" stroke-opacity="0.7"/>
      <circle cx="271.5" cy="140.5" r="8" stroke="white" stroke-opacity="0.7"/>
      <circle cx="140" cy="9" r="8" transform="rotate(90 140 9)" stroke="white" stroke-opacity="0.7"/>
      <circle cx="140" cy="272" r="8" transform="rotate(90 140 272)" stroke="white" stroke-opacity="0.7"/>
      <circle cx="140" cy="140" r="97.9" stroke="#FD8235" stroke-width="0.2"/>
      </svg>
      

  </div>

  <div class="ring-04">
   <svg width="280" height="281" viewBox="0 0 280 281" fill="none" xmlns="http://www.w3.org/2000/svg">
<circle cx="8.5" cy="140.5" r="8" stroke="white" stroke-opacity="0.7"/>
<circle cx="271.5" cy="140.5" r="8" stroke="white" stroke-opacity="0.7"/>
<circle cx="140" cy="9" r="8" transform="rotate(90 140 9)" stroke="white" stroke-opacity="0.7"/>
<circle cx="140" cy="272" r="8" transform="rotate(90 140 272)" stroke="white" stroke-opacity="0.7"/>
<circle cx="140" cy="140" r="97.9" stroke="#FD8235" stroke-width="1"/>
</svg>

  </div>
</div> 


  <style>

.radial-container {
    height: 50vh;
    width: 400px;
    position: relative;
    margin: auto;
    transform: scale(0.3);
}
  .health-container {
     display: flex;
    list-style: none;
    transition: all 0.2s ease-in;
    flex-direction: row;
    justify-content: center;
    width:fit-content;
    align-items: center;
    height: 100%; 
    align-content: space-between;
    margin: auto;
    position: absolute;
    top: 0px;
    bottom: 0px;
    left: 0px;
    right: 0px;
 
}


.health-container-01 {
     display: flex;
    list-style: none;
    transition: all 0.2s ease-in;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    height: 100%; 
    width:fit-content;
    align-content: space-between;
    margin: auto;
    transform:scale(2.5);
    transition-delay:1.3s;
    position: absolute;
    top: 0px;
    bottom: 0px;
    left: 0px;
    right: 0px;
 
}

.ring-01 {
    display: flex;
    list-style: none;
    transition: all 0.2s ease-in;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    position: absolute;
    height: 100%;
    align-content: space-between;
    margin: auto;
    top: 0px;
    bottom: 0px;
    left: 0px;
    right: 0px;
    width: 120px;
    transform:scale(1.5) rotate(45deg);
}

.radial-container:hover .ring-01 {
  transform:scale(2.5) rotate(-180deg);
}
.ring-02 {
    display: flex;
    list-style: none;
    transition: all 0.2s ease-in;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    position: absolute;
    height: 100%;
    transition-delay:0.1s;
    align-content: space-between;
    margin: auto;
    top: 0px;
    bottom: 0px;
    left: 0px;
    right: 0px;
    width: 120px;
    transform:scale(2.25) rotate(0deg);
}

.radial-container:hover .ring-02 {
  transform:scale(1.5) rotate(45deg);
  transition-delay:0.2s;
}


.ring-03 {
    display: flex;
    list-style: none;
    transition: all 0.2s ease-in;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    position: absolute;
    height: 100%;
    align-content: space-between;
    margin: auto;
    top: 0px;
    bottom: 0px;
    transition-delay:0.2s;
    left: 0px;
    right: 0px;
    width: 120px;
    transform:scale(0.15) rotate(0deg);
}

.radial-container:hover .ring-03 {
  transform:scale(0.65) rotate(180deg);
}


.ring-04 {
    display: flex;
    list-style: none;
    transition: all 0.2s ease-in;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    position: absolute;
    height: 100%;
    align-content: space-between;
    margin: auto;
    top: 0px;
    bottom: 0px;
    transition-delay:0.3s;
    left: 0px;
    right: 0px;
    width: 120px;
    transform:scale(3.15) rotate(0deg);
}

.radial-container:hover .ring-04 {
  transform:scale(0.5) rotate(270deg);
  transition-delay:0.3s;
}
.radial-container li div {
  transition: all 0.1s ease-out;
  transform: rotate(55deg);
 }

.radial-container:hover li div {
  transform:   rotate(35deg) scale(0.6);
  transition: all 0.2s ease-in;
  transform-origin: 0 0;
   
  transform: matrix(-0.84, 0.66, 0.47, 0.82, 0, 0);
}
.radial-container:active li div {
  transform:   rotate(135deg);
  transition: all 0.2s ease-in;
  transform-origin: 0 0; 
  transform: matrix(-0.84, 0.66, 0.47, 0.82, 0, 0);
}
.li-container div {
    float: right;
  transition: all 0.1s ease-out;
}

.li-container{
  width:200px;
  height:200px; 
  position: absolute;
} 
li:nth-child(1) {
  transition-delay: 0s;
  
}
li:nth-child(2) {
  transition-delay: 0.05s;
  transform:rotate(45deg);
}
li:nth-child(3) {
  transition-delay: 0.1s;
   transform:rotate(90deg);
}
li:nth-child(4) {
  transition-delay: 0.15s;
   transform:rotate(135deg);
}
li:nth-child(5) {
  transition-delay: 0.2s;
   transform:rotate(180deg);
}
li:nth-child(6) {
  transition-delay: 0.25s;
   transform:rotate(225deg);
}
li:nth-child(7) {
  transition-delay: 0.3s;
     transform:rotate(270deg);
}
li:nth-child(8) {
  transition-delay: 0.35s;
     transform:rotate(315deg);
}
li:nth-child(9) {
  transition-delay: 0.4s;
     transform:rotate(360deg);
}


.health-container:hover li:nth-child(1)  div {
  transition-delay: 0s;
  
}
.health-container:hover li:nth-child(2)  div {
  transition-delay: 0.05s;
 
}
.health-container:hover li:nth-child(3)  div {
  transition-delay: 0.1s;
   
}
.health-container:hover li:nth-child(4)  div {
  transition-delay: 0.15s;
  
}
.health-container:hover li:nth-child(5)  div  {
  transition-delay: 0.2s;
  
}
.health-container:hover li:nth-child(6)  div  {
  transition-delay: 0.25s;
   
}
.health-container:hover li:nth-child(7)  div {
  transition-delay: 0.3s;
     
}
.health-container:hover li:nth-child(8)  div {
  transition-delay: 0.35s;
     
}
.health-container:hover li:nth-child(9)  div {
  transition-delay: 0.4s;
 
}


 li:nth-child(1)  div {
  transition-delay: 0s;
  
}
 li:nth-child(2)  div {
  transition-delay: 0.05s;
 
}
 li:nth-child(3)  div {
  transition-delay: 0.1s;
   
}
 li:nth-child(4)  div {
  transition-delay: 0.15s;
  
}
 li:nth-child(5)  div  {
  transition-delay: 0.2s;
  
}
 li:nth-child(6)  div  {
  transition-delay: 0.25s;
   
}
 li:nth-child(7)  div {
  transition-delay: 0.35s;
     
}
  li:nth-child(8)  div {
  transition-delay: 0.4s;
     
}
  li:nth-child(9)  div {
  transition-delay: 0.45s;
 
}
.rotating {
  -webkit-animation: rotating 50s linear infinite;
  -moz-animation: rotating  50s linear infinite;
  -ms-animation: rotating 50s linear infinite;
  -o-animation: rotating 50s linear infinite;
  animation: rotating 50s linear infinite;
}


@media screen and (max-width: 800px) {
.radial-container{
    margin-top: 130px;
}
}

  </style>