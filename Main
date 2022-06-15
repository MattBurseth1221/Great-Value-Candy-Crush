var numRows = 10;
var mult = World.width / numRows;
var score = 0;
var swap = [];
var check = true;
World.frameRate = 1;

var cells = [
  [null, null, null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null, null, null],
  [null, null, null, null, null, null, null, null, null, null]
  ];
var vals = ["red", "blue", "green", "orange", "yellow"];

function setup() {
  initCells();
  checkClears();
  
  console.log("========Begin========");
  score = 0;
  logBonus(0);
}

function draw() {
  background("white");
  drawCells();
  
  if (check) {
    if (!checkClears());
    check = false;
  }
  
  checkMouse();
  
  outlineSelectedCells();
  
  if (swap.length == 2) {
    swapCells();
  }
}

function checkMouse() {
  if (mouseWentDown("leftButton")) {
    swap.push(cells[Math.floor(World.mouseX / mult)][Math.floor(World.mouseY / mult)]);
  }
}

function outlineSelectedCells() {
  for (var i = 0; i < swap.length; i++) {
    strokeWeight(2);
    stroke("white");
    //top line
    line(swap[i].x, swap[i].y, swap[i].x + mult, swap[i].y);
    //right line
    line(swap[i].x + mult, swap[i].y, swap[i].x + mult, swap[i].y + mult);
    //bottom line
    line(swap[i].x, swap[i].y + mult, swap[i].x + mult, swap[i].y + mult);
    //left line
    line(swap[i].x, swap[i].y, swap[i].x, swap[i].y + mult);
  }
}

function swapCells() {
  if (swap[0].isNextTo(swap[1])) {
    var tempVal = swap[0].value;
    swap[0].value = swap[1].value;
    swap[1].value = tempVal;
    
    check = true;
  } else {
    console.log("Not next to eachother");
    check = false;
  }
  
  swap.pop();
  swap.pop();
}

function initCells() {
  for (var i = 0; i < numRows; i++) {
    for (var j = 0; j < numRows; j++) {
      cells[i][j] = new Cell(i * mult, j * mult);
    }
  }
}

function checkClears() {
  World.frameRate = 1;
  
  /*for (var i = 0; i < numRows; i++) {
    for (var j = 0; j < numRows - 2; j++) {    
      if (isVCombo(i, j)) {
        cells[i][j] = new Cell(i * mult, j * mult);
        cells[i][j + 1] = new Cell(i * mult, (j + 1) * mult);
        cells[i][j + 2] = new Cell(i * mult, (j + 2) * mult);
        console.log("Vert " + i + " " + j);
        score += 3;
      } 
    }
  }*/
  var len = 0;
  
  for (var i = 0; i < numRows; i++) {
    for (var j = 0; j < numRows - 2; j++) {    
      if (isVCombo(i, j)) {
        len = sizeOfVCombo(i, j);
        updateCellsVert(i, j, len);
        //console.log("Vert " + i + " " + j + " len " + len);
        checkClears();
      } 
    }
  }
  
  /*for (var i = 0; i < numRows - 2; i++) {
    for (var j = 0; j < numRows; j++) {
      if (isHCombo(i, j)) {
        cells[i][j] = new Cell(i * mult, j * mult);
        cells[i + 1][j] = new Cell((i + 1) * mult, j * mult);
        cells[i + 2][j] = new Cell((i + 2) * mult, j * mult);
        score += 3;
        console.log("Horiz " + i + " " + j);
      }
    }
  }*/
  
  for (var i = 0; i < numRows - 2; i++) {
    for (var j = 0; j < numRows; j++) {    
      if (isHCombo(i, j)) {
        len = sizeOfHCombo(i, j);
        updateCellsHoriz(i, j, len);
        //console.log("Horiz " + i + " " + j + " len " + len);
        checkClears();
      } 
    }
  }
  
  World.frameRate = 30;
  return false;
}

function sizeOfHCombo(i, j) {
  var iIndex = i;
  var len = 0;
  var val = cells[i][j].value;
  //console.log("i " + i + " j " + j);
  
  while (iIndex < numRows) {
    if (cells[iIndex][j].value == val) {
      len++;
      iIndex++;
      //console.log("  iIndex " + iIndex);
    } else {
      break;
    }
  }
  
  return len;
} 

function sizeOfVCombo(i, j) {
  var jIndex = j;
  var len = 0;
  var val = cells[i][j].value;
  
  while (jIndex < numRows) {
    if (cells[i][jIndex].value == val) {
      len++;
      jIndex++;
    } else {
      break;
    }
  }
  
  return len;
}

function isHCombo(i, j) {
  var val = cells[i][j].value;
  
  if (cells[i + 1][j].value == val && cells[i + 2][j].value == val) {
    return true; 
  }
  
  return false;
}

function isVCombo(i, j) {
  var val = cells[i][j].value;
  
  if (cells[i][j + 1].value == val && cells[i][j + 2].value == val) {
    return true;
  }
  
  return false;
}

function updateCellsVert(i, j, len) {
  for (var k = j + (len - 1); k > (len - 1); k--) {
    cells[i][k].value = cells[i][k - len].value;
  }
  
  for (var replace = 0; replace < len; replace++) {
    cells[i][replace].changeVal();
  }
  
  logBonus(len);
}

function updateCellsHoriz(i, j, len) {
  for (var y = j; y > 0; y--) {
    for (var x = i; x < i + len; x++) {
      cells[x][y].value = cells[x][y - 1].value;
    }
  }
  
  for (var replace = i; replace < i + len; replace++) {
    cells[replace][0].changeVal();
  }
  
  logBonus(len);
}

function logBonus(length) {
  score += scoreConvert(length);
  console.log("Score: " + score);
}

function scoreConvert(length) {
  switch (length) {
    case 0:
      return 0;
    case 3: 
      return 10;
    case 4: 
      return 25;
    case 5:
      return 50;
    default:
      console.log("how did this happen?");
    break;
  }
}
  
function Cell(x, y) {
  this.x = x;
  this.y = y;
  this.w = World.width / numRows;
  this.value = vals[getRandVal()];
    
  this.show = function() {
    fill(this.value);
    rect(this.x, this.y, this.w, this.w);
  };
  
  this.changeVal = function() {
    this.value = vals[getRandVal()];
  };
  
  this.changeXToI = function() {
    return [Math.floor(this.x / mult), Math.floor(this.y / mult)];
  };
  
  this.isNextTo = function(tempCell) {
    var tcs = tempCell.changeXToI();
    console.log(tcs[0], tcs[1]);
    var hcs = this.changeXToI();
    console.log(hcs[0], hcs[1]);
    
    if (tcs[0] == hcs[0] + 1 && tcs[1] == hcs[1]) {
      return true;
    } else if (tcs[0] == hcs[0] - 1 && tcs[1] == hcs[1]) {
      return true;
    } else if (tcs[0] == hcs[0] && tcs[1] == hcs[1] + 1) {
      return true;
    } else if (tcs[0] == hcs[0] && tcs[1] == hcs[1] - 1) {
      return true;
    }
    
    return false;
  };
}
  
function getRandVal() {
  return Math.floor(Math.random() * vals.length);
}

function drawCells() {
  for (var i = 0; i < numRows; i++) {
    for (var j = 0; j < numRows; j++) {
      cells[i][j].show();
    }
  }
}
