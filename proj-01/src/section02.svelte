<script>
  import Terrain from "./terrain.svelte";
  import GridComponent from "./SVGgridComponent.svelte";
  import MapIndicator from "./mapIndicator.svelte";
</script>
<section class="section-02" id="four">
 
  <div class="title">  FIND ME </div>
    <ul class="unit-list-top" >
      <li> 00 </li>
       <li> 01 </li>  
      <li> 02 </li>  
      <li> 03 </li>  
      <li> 04 </li>  
      <li> 05 </li>  
      <li> 06 </li>  
      <li> 07 </li>
    </ul>
    <div class="terrain-container" >
   <div class="marker-01"> 
 <a href="https://www.behance.net/zacting" target="_blank"> <MapIndicator />
   Behance  </a> 
   </div>  
   <div class="marker-02"> 
    <a href=" https://www.instagram.com/izacting/" target="_blank"> <MapIndicator />
    Instagram</a> 
   </div>  
   <div class="marker-03"> 
    <a href=" https://www.linkedin.com/in/issac-ting-b489a98b/" target="_blank"> <MapIndicator />
    LinkedIn
  </a> 
   </div>  
      <Terrain />
      <GridComponent />

    </div>
     
     <ul class="unit-list-bottom" >
      <li> 10 </li>
      <li> 20 </li>  
      <li> 30 </li>  
      <li> 40 </li>  
      <li> 50 </li>  
      <li> 60 </li>  
      <li> 70 </li>  
      <li> 80 </li>
    </ul>
  </section>

  <style>

.section-02  .terrain-container{
    
   display:none;
      transition: all 1s;
      max-width: 1200px;
    }
    .section-02:hover .terrain-container{
      display: block;
      opacity: 1;
    }

.marker-01 {
    top: 50vh; 
    left:0;
    right: 0;
    position: absolute;
    color: white;
    margin: 10px  auto;
    width: 100%;
    z-index: 11;
    display:flex;
    width: auto;
    animation: fadein .2s ease-in forwards;
    animation-delay: 0.3s;
   
    transform: translate(60%, 10px);
}

.marker-02 {
    top: 40vh;
    color: white;
    left:0;
    right: 0;
    position: absolute;
    transform: translate(20%, 50px);
    z-index: 11;
    opacity: 0;
    animation: fadein .2s ease-in forwards;
    animation-delay:1.3s;
}

.marker-03 {
    top: 65vh;
    left:0;
    color: white;
    right: 0;
    position: absolute;
    transform: translate(50%, 10%);
    z-index: 11;
    opacity: 0;
    animation: fadein .2s ease-in forwards;
    animation-delay:0.9s;
}
      .terrain-container {
          height: 100vh;
          width:100vw;
        max-width: 1600px; 
        margin: auto;
        position: absolute;
        left:0px;
        right:0px;
      }
   
  
   /* On smaller screens, decrease text size */
   @media only screen and (max-width: 960px) {
 
 
 
  }
  </style>