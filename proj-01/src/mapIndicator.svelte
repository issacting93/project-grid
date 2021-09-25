<div class="button-ripple"> 

</div>

<style>
    

.button-ripple {
  background-color: white;
  width: 1em;
  height: 1em;
  border-radius: 50%;
  animation: buttpn-ripple 1.5s linear;
  transition-timing-function: ease-in;
  animation-iteration-count: infinite;
  transition-duration: 0.7s;
  transition: all 2.3s;
  border:1ps solid white;
  
  
}

@keyframes  buttpn-ripple {
  0% {
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.5), 0 0 0 0.1em rgba(255, 255, 255, 0.3), 0 0 0 0.3em rgba(255, 255, 255, 0.3), 0 0 0 0.5em rgba(255, 255, 255, 0.3);
  }
  90% {
    box-shadow: 0 0 0 0.1em rgba(255, 255, 255, 0), 0 0 0 0.3em rgba(255, 255, 255, 0), 0 0 0 0.5em rgba(255, 255, 255, 0), 0 0 0 2em rgba(255, 255, 255, 0);
  }
}
@keyframes ripple-large {
  0% {
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.3), 0 0 0 1em rgba(255, 255, 255, 0.3), 0 0 0 1.5em rgba(255, 255, 255, 0.3), 0 0 0 2.5em rgba(255, 255, 255, 0.3), 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  }
  100% {
    box-shadow: 0 0 0 1em rgba(255, 255, 255, 0.3), 0 0 0 1.5em rgba(255, 255, 255, 0.3), 0 0 0 2.5em rgba(255, 255, 255, 0.3), 0 0 0 8em rgba(255, 255, 255, 0), 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  }
}
@keyframes ripple-clicked {
  0% {
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.3), 0 0 0 1em rgba(255, 255, 255, 0.3), 0 0 0 1em rgba(255, 255, 255, 0.3), 0 0 0 2em rgba(255, 255, 255, 0.3), 0 14px 8px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  }
  50% {
    box-shadow: 0 0 0 1111em white, 0 0 0 111.5em rgba(255, 255, 255, 0.3), 0 0 0 112.5em rgba(255, 255, 255, 0.3), 0 0 0 118em rgba(255, 255, 255, 0), 0 14px 128px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  }
  100% {
    box-shadow: 0 0 0 1111em white;
  }
}
</style>