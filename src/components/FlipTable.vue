<template>
  <div>
    <div id="container">
  <div class="guy">
    (<div class="arm">╯</div>°□°）<div class="arm">╯</div>
  </div>
  <div id="table-flip">┻━┻</div>
    <div class="guy">
  </div>
</div>
  </div>
</template>

<script>
  
</script>

<style >

#container{
  margin-top: 2.2em;
}

div{
  display: inline-block;
}

#table-flip{
  animation-timing-function: linear;
  animation: table-flip 2s infinite;
  position: relative;
}

.arm{
  animation-timing-function: linear;
  animation: arm-rotate 2s infinite;
  position: relative;
}

@keyframes table-flip {
  0% {  left: 0px; top: 0em; transform: rotate(-180deg);  }
  24% {  top: -2em;  }
  60% { left: 2.5em; top: 0em; transform: rotate(0deg); }
  80% { left: 2.5em; top: 0em; transform: rotate(0deg); }
  100% {  left: 0px; top: 0em; transform: rotate(-180deg);  }
}

@keyframes arm-rotate {
  0% { top: 0.2em; transform: rotate(90deg); }
  20% { top: 0em;transform: rotate(0deg); }
  50% { top: 0em;transform: rotate(0deg); }
  100% { top: 0.2em;transform: rotate(90deg); }
}
</style>