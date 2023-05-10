<script lang="ts">
  import { onMount } from 'svelte';

onMount(() => {
    drawBoard();
});

function drawBoard() {
  let canvas = document.getElementById("board");
  let ctx = canvas.getContext("2d");
  ctx.beginPath();
  ctx.rect(20, 20, 150, 100);
  ctx.fill();

}

</script>

<main>
  <h1>DragonChess!</h1>

  <canvas id="board" style="widht: 600px; height: 600px;"></canvas>




</main>

<style>
  
</style>
