<script lang="ts">
  import { afterUpdate, onMount } from 'svelte';
    import { assign } from 'svelte/internal';


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
  ["black_rook", { x: 48, y: 16, width: 16, height: 16}],
  ["white_queen", { x: 64, y: 0, width: 16, height: 16}],
  ["black_queen", { x: 64, y: 16, width: 16, height: 16}],
  ["white_king", { x: 80, y: 0, width: 16, height: 16}],
  ["black_king", { x: 80, y: 16, width: 16, height: 16}],
  ["cursor_white", { x: 0, y: 32, width: 16, height: 16}],
  ["cursor_black", { x: 16, y: 32, width: 16, height: 16}],
]);


class Square {
  piece: string;
  highlighted: boolean;
  selected: boolean; 
}

let board : Square[] = Array(64);
init_board(board);

function square_at(column:number, row:number) : Square {
  return board[8*row + column];
}

function init_board(b:Square[]) {
  for(let i = 0; i < 64; i++){
    b[i] = {piece: undefined, highlighted: false, selected: false};
  }
  square_at(0, 0).piece = "white_rook";
  square_at(1, 0).piece = "white_knight";
  square_at(2, 0).piece = "white_bishop";
  square_at(3, 0).piece = "white_queen";
  square_at(4, 0).piece = "white_king";
  square_at(5, 0).piece = "white_bishop";
  square_at(6, 0).piece = "white_knight";
  square_at(7, 0).piece = "white_rook";
  for(let p = 0; p < 8; p++){
    square_at(p, 1).piece = "white_pawn"; 
  }
  square_at(0, 7).piece = "black_rook";
  square_at(1, 7).piece = "black_knight";
  square_at(2, 7).piece = "black_bishop";
  square_at(3, 7).piece = "black_queen";
  square_at(4, 7).piece = "black_king";
  square_at(5, 7).piece = "black_bishop";
  square_at(6, 7).piece = "black_knight";
  square_at(7, 7).piece = "black_rook";
  for(let p = 0; p < 8; p++){
    square_at(p, 6).piece = "black_pawn"; 
  }
 
}



function draw_board() {
  let canvas = document.getElementById("board");
  let ctx = canvas.getContext("2d");
  ctx.imageSmoothingEnabled = false;
  let sprites = document.getElementById("spritesheet");
  ctx.fillStyle = "#3f3053";
  ctx.beginPath();
  ctx.rect(0, 0, 512, 512);
  ctx.fill();

 ctx.fillStyle = "#edfeff";
  ctx.beginPath();
  for(let j = 0; j < 8; j++){
    for(let i = 0; i < 8; i ++){
      let black_begins = (j%2);
      let sprite:Sprite;
      if(i%2==black_begins){
        sprite = spritesheet.get("white_square");
      } else {
        sprite = spritesheet.get("black_square");
      }
      ctx.drawImage(sprites, 
                    sprite.x, sprite.y, sprite.width, sprite.height, 
                    64*i, 64*j, 64, 64);
    }
  }
  ctx.fill();
  draw_pieces(ctx, sprites);
  draw_highlights(ctx, sprites);


}

function draw_pieces(ctx, pieces) {
  for(let row = 0; row < 8; row++){
    for(let column = 0; column < 8; column++){
      let sprite = spritesheet.get(square_at(column, row).piece);
      if(sprite){
        ctx.drawImage(pieces, 
                      sprite.x, sprite.y, sprite.width, sprite.height,
                      column*64, (7-row)*64, 64, 64);
      }
    }
  }
}

function draw_highlights(ctx, sprites) {
  for(let row = 0; row < 8; row++){
    for(let column = 0; column < 8; column++){
      if(square_at(column, row).highlighted) {
        let sprite = spritesheet.get("cursor_white");
        ctx.drawImage(sprites,
          sprite.x, sprite.y, sprite.width, sprite.height,
          column*64, (7-row)*64, 64, 64);
      }
    }
  }
}


function mouseMoved(event){
  let canvas = document.getElementById("board");
  let rect = canvas.getBoundingClientRect();
  let x = 512*(event.clientX - rect.left)/rect.width;
  let y = 512*(event.clientY - rect.y)/rect.height;
  let column = Math.floor(x/64);
  let row = 7 - Math.floor(y/64);
  // console.log("col == " + column + ", row == " + row);
  for(let sclear = 0; sclear <64; sclear++) {
    board[sclear].highlighted = false;
  }
  square_at(column, row).highlighted = true;
  draw_board();
};

onMount(()=>{
  let can = document.getElementById("board");
  can.addEventListener("mousemove", mouseMoved);
});

</script>

<main>
  <h1>DragonChess!</h1>

  <img id="spritesheet" src="dragonchess.png" class="spritesheet" on:load={draw_board} />
<center>
  <canvas id="board" width="512" height="512" ></canvas>
</center>



</main>

<style>
main {
     height: 100%;
}
canvas#board {
  
  width: 90%;
  aspect-ratio: 1 / 1;

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
@media (min-aspect-ratio: 1/1) {
       canvas#board {
                    width: auto;
                    height: 90%;
           
           aspect-ratio: 1/ 1;
        }
}
img.spritesheet {
  display: none;

}
</style>
