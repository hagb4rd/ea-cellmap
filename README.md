# ea-cellmap

##### Contact 
* mail - earendel@wolke7.net
* chat - http://bit.ly/irc-javascript


### Install

` npm install ea-cellmap --save `

### Usage

```javascript

//require module
var CellMap = require('ea-cellmap');

//init 10x10 cells
var cell = CellMap.create(10,10);

//set some property on any cell within range
cell(0,1).alive = true;

//get neighboured cells 
cell(0,1).cells.all()

//get neighboured cell to the right
cell(0,1).cells.right;

```

### Source Code

```javascript

/* git@github.com:hagb4rd/ea-cellmap.git */ 
var util = require('util');

function CellMap(cols, rows) {
	this.cols = cols;
	this.rows = rows;
	this.items = new Array(rows * cols).fill(0).map((elem, index) => this.createCell(index));
};

CellMap.prototype.createCell = function(i) {
	var self = this;
	i = i || 0;
	var cell = {
		x: (i + self.cols) % (self.cols),
		y: Math.floor(i / self.cols),
		index: i
	};

	cell.cells = {
		all: () => {
			return [[-1, -1],[0, -1],[1, -1],[-1, 0],[1, 0],[-1, 1],[0, 1],[1, 1]].map(z => self.field(cell.x + z[0], cell.y + z[1]))
		},
		get topLeft() {
			return this.all()[0]
		},
		get top() {
			return this.all()[1]
		},
		get topRight() {
			return this.all()[2]
		},
		get left() {
			return this.all()[3]
		},
		get right() {
			return this.all()[4]
		},
		get bottomLeft() {
			return this.all()[5]
		},
		get bottom() {
			return this.all()[6]
		},
		get bottomRight() {
			return this.all()[7]
		}
	};
	return cell;
};

CellMap.prototype.field = function(x, y) {
	if (!arguments.length) return this.items;
	x = Math.abs(Math.floor(parseInt(cols, 10)));
	y = Math.abs(Math.floor(parseInt(rows, 10)));
	if ((x < 0) || (x >= this.colt)) return null;
	if ((y < 0) || (y >= this.rows)) return null;
	return this.items[y * this.cols + x]
};

CellMap.create = function(cols, rows) {
	var cellMap = new CellMap(cols, rows);
	var f = cellMap.field.bind(cellMap);
	f.cellMap = cellMap;
	f.toJSON = function() {
		return JSON.stringify(f.cellMap)
	};
	f.inspect = () => util.inspect(f.cellMap);
	return f;
};

CellMap.help = " https://github.com/hagb4rd/ea-cellmap ";

module.exports = CellMap;

```