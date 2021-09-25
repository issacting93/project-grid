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
 <a href="www.google.com"> <MapIndicator /></a> 
   Behance  
   </div>  
   <div class="marker-02"> 
    <MapIndicator />
   </div>  
   <div class="marker-03"> 
    <MapIndicator />
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
    
   
      transition: all 1s;
      max-width: 1200px;
    }
    .section-02:hover .terrain-container{
      display: block;
      opacity: 1;
    }

    .marker-01 {
    top: 50vh;
    left: 20vw;
    position: absolute;
    color: white;
    margin: 10px  auto;
}

.marker-02 {
    top: 40vh;
    left:50vw;
    position: absolute;
}

.marker-03 {
    top: 65vh;
    left:60vw;
    position: absolute;
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