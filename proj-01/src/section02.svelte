<script>
  import Terrain from "./terrain.svelte";
  import GridComponent from "./SVGgridComponent.svelte";
</script>
<section class="section-02" id="two">
 
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
      <Terrain />
      <GridComponent />

    </div>
    <div class="about"> 
      <div class="col-main">
        <div class="col-1 heroType">  2049 </div>
        <div class="col-1 body"> <i> "Once upon a time, people were talking to graves and nobody batted an eyelid, right?"  </i>  </div>
      </div>
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
      opacity: 0;
      transition: all 1s;
    }
    .section-02:hover .terrain-container{
      display: block;
      opacity: 1;
    }


    .col-main{
      width: 1200px;
      margin: auto; 
      height: 50%;
      display: flex;
      align-content: stretch;
      position: absolute;
      top: 0px;
      bottom: 0px;
      left: 0px;
      right: 0px;
      
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
 
    .col-1{ 
      margin:20px;
      width:50%;
      height:100%;

    } 
.heroType{
  font-size:  calc(64px + 2.5vw);;

}
  .body  {
    font-size: 44px;
    text-align: left;
    font-family: "Barlow";
    -webkit-text-fill-color: white;
    line-height: 170%;
    font-weight: 100;
}
   /* On smaller screens, decrease text size */
   @media only screen and (max-width: 960px) {
     .about{
      width: calc(100vw - 20px);
     } 
	.col-main{
		flex-direction: column;
    width: 100%;
    text-align: left;
	  }
	  
	.heroType{
		font-size:  calc(32px + 22.5vw);;
	
	}
  .col-1{
    width:100%;
  }

	.body  {
	  font-size: 24px;
	  text-align: left;
	  font-family: "Barlow";
	  -webkit-text-fill-color: white;
	  line-height: 170%;
	  font-weight: 100;
  
  }
   
 
  }
  </style>