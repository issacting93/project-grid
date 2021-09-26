<script>

    import RadialInterface from "./RadialInterface.svelte";
</script>


<section class="section-03" id="about">
    <div class=" title"> ABOUT </div>
    <div class="about-container"> 
        <RadialInterface />
        

    
           
<div class="about-text">I grew up with a deep love for science fiction. I believe that our capacity to dream of a future together has helped drive our development as a species.
    <br>
    As such, I am deeply passionate about designing for things both in the near and far future by boldly going into new technologies. I love to explore the space between design and technology. 
</div>
</div> 
</section>
 

<style>
 .about-text{
     max-width: 700px;
     font-weight: 100;
font-size: 16px;
line-height: 24px;
text-align: center;
color:white;
margin:auto;
 }
 .about-container {
    display: flex;
    height: 60%;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-top: -100px;
}
section.section-03 {
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: "Barlow";
font-style: normal;
}

@media only screen and (max-width: 800px) {
    .about-container {
    display: flex;
    height: 100%;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-top: -100px;
    padding: 20px;
} 

.about-text  {
  
    color: white;
    margin: auto;
    width: calc(100% - 40px );
}
}
</style>