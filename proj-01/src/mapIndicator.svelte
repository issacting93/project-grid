<div class="button-ripple"> 

</div>

<style>
    

.button-ripple {
  background-color: white;
  width: 1em;
  height: 1em;
  border-radius: 50%;
  animation:button-ripple  4.5s linear;
  transition-timing-function: ease-in;
  animation-iteration-count: infinite;
  transition-duration: 0.7s;
  transition: all  .3s;
  border:4px solid black;
  
  
}

.button-ripple:hover{
  transform: scale(.7);
  border:4px solid white;
  animation: ripple-large 3.5s linear;
  transition-timing-function: ease-in;
  animation-iteration-count: infinite;
}

.button-ripple:active {
  transform: scale(1.2);
  border:4px solid white;
  animation: ripple-clicked 1.5s linear;
  transition-timing-function: ease-in;
  animation-iteration-count: infinite;
}




@keyframes  button-ripple {
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
    transform: scale(.9);
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.5), 0 0 0 0.1em rgba(255, 255, 255, 0.3), 0 0 0 0.3em rgba(255, 255, 255, 0.3), 0 0 0 0.5em rgba(255, 255, 255, 0.3);

     }
  
  100% {
    transform: scale(1.2);
    box-shadow: 0 0 0 0.1em rgba(255, 255, 255, 0), 0 0 0 0.3em rgba(255, 255, 255, 0), 0 0 0 0.5em rgba(255, 255, 255, 0), 0 0 0 2em rgba(255, 255, 255, 0);

      }
}
</style>