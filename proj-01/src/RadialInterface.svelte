
<script>
	import ScifiMod01 from './scifiMod01.svelte';
 
</script>
<section class="">
    <ul class="health-container">
      <li class="li-container">
    <div> <ScifiMod01 /> </div>   
      </li>
  <li class="li-container">
    <div> <ScifiMod01 /> </div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod01 /> </div>   
      </li>
      <li class="li-container">
        <div> <ScifiMod01 /> </div>   
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
  </section> 


  <style>
      .health-container {
        display: flex;
    list-style: none;
    transition: all 0.2s ease-in;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: fit-content;
    align-content: space-between;
    margin: auto;
 
}


.health-container li div {
  transition: all 0.1s ease-out;
  
 
}
.health-container:hover li div {
  transform:   rotate3d(0, -1, 0,  35deg) scale(0.6);
  transition: all 0.2s ease-in;
  transform-origin: 0 0;
  
 box-shadow: 0px 15px 7px rgba(0, 0, 0, 0.2);
  transform: matrix(-0.84, 0.66, 0.47, 0.82, 0, 0);
}
.health-container:active li div {
  transform:   rotate3d(0, -1, 0,  135deg);
  transition: all 0.2s ease-in;
  transform-origin: 0 0;
  
 box-shadow: 0px 15px 7px rgba(0, 0, 0, 0.2);
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


  </style>