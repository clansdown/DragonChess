<script lang="ts">
  import { onMount } from 'svelte';

onMount(() => {
    drawBoard();
});

function drawBoard() {
  let canvas = document.getElementById("board");
  let ctx = canvas.getContext("2d");
  ctx.fillStyle = "#3f3053";
  ctx.beginPath();
  ctx.rect(0, 0, 600, 600);
  ctx.fill();

 ctx.fillStyle = "#edfeff";
  ctx.beginPath();
  for(let j = 0; j < 8; j++){
    for(let i = 0; i <= 4; i ++){
      let offset = 0;
      if(j%2==1){
        offset = 75;
      }
      ctx.rect(150*i+offset, 75*j, 75, 75);
    }
  }
  ctx.fill();

if(0) {
  ctx.fill();
  ctx.fillStyle = "#f00";
  ctx.beginPath();
  for(let i = 1;i <= 8; i++){
    ctx.rect(0, 75*i, 600, 1);
  }
  ctx.fill();
  
  ctx.fillStyle = "#0ff";
  ctx.beginPath();
  for(let i = 1;i <= 8; i++){
    ctx.rect(75*i, 0, 1, 600);
  }
  ctx.fill();
}
 
}

</script>

<main>
  <h1>DragonChess!</h1>

  <canvas id="board" width="600" height="600" style="width: 600px; height: 600px;"></canvas>




</main>

<style>

canvas#board {
  border: 1px solid #f00;


}

</style>
