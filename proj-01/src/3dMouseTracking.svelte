<div id="scenes">
    <div class="scene scene1"> 
      <div class="dot"></div>
    </div>
  </div>

  <style>
  html, body {
    width: 100%;
    height: 100%;
    min-width: 500px;
    min-height: 500px;
    overflow: hidden;
  }
   
  #scenes {
    width: 100vw;
    height: 100vh;
    margin: 0 auto;
    position: absolute;
    top: 50%;
    transform: scale(1) translateY(-50%);
    transform-origin: 0 0;
  }
  .scene {
    width: 100%;
    height: 100%;
  }
  
  .scene {
    position: relative;
    overflow: hidden;
  }
   
  .scene1 .dot {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 0.2em;
    height: 0.2em;
    background-color: #97f4ff;
    box-shadow: 0 0 .2em .13em #52a8e8, 0 0 0 0 black, 0 0 0 0 #298df4;
    opacity: 0;
    animation: dot-anim linear 1.834s infinite;
    border-radius: 50%;
  }
  
  @keyframes dot-anim {
    45% {
      opacity: .1;
    }
    46% {
      box-shadow: 0 0 .2em .13em #52a8e8, 0 0 0 0 black, 0 0 5px 10px #298df4;
    }
    50% {
      box-shadow: 0 0 .2em .13em #52a8e8, 0 0 0 101px black, 0 0 0 173px #298df4;
    }
    52% {
      box-shadow: 0 0 .2em .13em #52a8e8, 0 0 0 245px black, 0 0 0 255px #298df4;
    }
    55% {
      opacity:.1;
      box-shadow: 0 0 .2em .13em #52a8e8, 0 0 0 317px black, 0 0 0 320px #298df4;
    }
    81% {
      opacity: 0;
      box-shadow: 0 0 .2em .13em #52a8e8, 0 0 0 549px black, 0 0 0 549px #298df4;
    }
  }</style>