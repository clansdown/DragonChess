<script lang="ts">
  import { onMount } from 'svelte';

onMount(() => {
    drawBoard();
});

function drawBoard() {
  let canvas = document.getElementById("board");
  let ctx = canvas.getContext("2d");
  ctx.beginPath();
  ctx.rect(0, 0, 600, 600);
  ctx.fill();

  ctx.fillStyle = "#fff";
  ctx.beginPath();
  for(let i = 1;i <= 8; i++){
    ctx.rect(0, 75*i, 600, 1);
  }
  ctx.fill();
  
  ctx.fillStyle = "#fff";
  ctx.beginPath();
  for(let i = 1;i <= 8; i++){
    ctx.rect(75*i, 0, 1, 600);
  }
  ctx.fill();
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
