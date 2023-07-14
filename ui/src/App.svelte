<script lang="ts">
  import { afterUpdate, onMount } from 'svelte';


class Sprite {
  x: number;
  y: number;
  width: number;
  height: number;
}

let spritesheet = new Map<string,Sprite>([
  ["white_square", { x: 96, y: 0, width: 16, height: 16}],
  ["black_square", { x: 96, y: 16, width: 16, height: 16}],  
  ["white_pawn", { x: 0, y: 0, width: 16, height: 16}],
  ["black_pawn", { x: 0, y: 16, width: 16, height: 16}],
  ["white_knight", { x: 16, y: 0, width: 16, height: 16}],
  ["black_knight", { x: 16, y: 16, width: 16, height: 16}],
  ["white_bishop", { x: 32, y: 0, width: 16, height: 16}],
  ["black_bishop", { x: 32, y: 16, width: 16, height: 16}],
  ["white_rook", { x: 48, y: 0, width: 16, height: 16}],
  ["br", { x: 48, y: 16, width: 16, height: 16}],
  ["wq", { x: 64, y: 0, width: 16, height: 16}],
  ["bq", { x: 64, y: 16, width: 16, height: 16}],
  ["wk", { x: 80, y: 0, width: 16, height: 16}],
  ["bk", { x: 80, y: 16, width: 16, height: 16}],
  ["cursor_white", { x: 0, y: 32, width: 16, height: 16}],
  ["cursor_black", { x: 16, y: 32, width: 16, height: 16}],
]);

function drawBoard() {
  let canvas = document.getElementById("board");
  let ctx = canvas.getContext("2d");
  ctx.imageSmoothingEnabled = false;
  let sprites = document.getElementById("spritesheet");
  ctx.fillStyle = "#3f3053";
  ctx.beginPath();
  ctx.rect(0, 0, 600, 600);
  ctx.fill();

 ctx.fillStyle = "#edfeff";
  ctx.beginPath();
  for(let j = 0; j < 8; j++){
    for(let i = 0; i < 8; i ++){
      let black_begins = (j%2);
      let sprite;
      if(i%2==black_begins){
          // white tiles
        sprite = spritesheet.get("white_square");
      } else {
          // black tiles
        sprite = spritesheet.get("black_square");
      }
      ctx.drawImage(sprites, sprite.x, sprite.y, sprite.width, sprite.height, 75*i, 75*j, 75, 75);
    }
  }
  ctx.fill();
}



</script>

<main>
  <h1>DragonChess!</h1>

  <img id="spritesheet" src="dragonchess.png" class="spritesheet" on:load={drawBoard} />
<center>
  <canvas id="board" width="600" height="600" ></canvas>
</center>



</main>

<style>
canvas#board {
  
  width: 90%;
  aspect-ratio: 1 / 1;
  /*
  width: 900px;
  height: 900px;
  */
  border: 1px solid #3f3053;
  margin: auto;
  image-rendering: optimizeSpeed; 
  /* Older versions of FF*/ 
  image-rendering: -moz-crisp-edges; 
  /* FF 6.0+ */ 
  image-rendering: -webkit-optimize-contrast; 
  /* Webkit // (Safari now, Chrome soon) */ 
  image-rendering: -o-crisp-edges; 
  /* OS X & Windows Opera (12.02+) */
  display: optimize-contrast; 
  /* Possible future browsers. */ 
  -ms-interpolation-mode: nearest-neighbor;
}
img.spritesheet {
  display: none;

}
</style>
