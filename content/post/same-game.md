+++
date = "2016-12-21T12:00:21+08:00"
title = "消除游戏"

+++

## 用 phaser 制作消除游戏

最近我使用 phaser 游戏框架做了一个简单的消除游戏。

#### 基本设置

我使用了 electron 而不是 http 服务器来做本地调试，  
因为 electron 自带 require，可以使我的代码更模块化，  
而且我还可以使用 ES6。  

所以我的游戏入口设置是这样的：  

```JavaScript
const game = new Phaser.Game(options.width, options.height);

game.state.add('InGame', InGame);
game.state.start('InGame');
```

我的游戏只有一个 in game 游戏状态，这里会有所有的游戏逻辑。

#### 游戏逻辑

首先是构造器，这里面会有我们要用到的状态，比较重要的是以下几个：

```JavaScript
class InGame {
	constructor() {
		// 是否允许用户点击
		// 如果有消除或填充的动画，则不允许用户点击
		this.canPick = true;
		// 二维数组，保存画面上所有可以消除的方块
		this.tilesArray = [];
		// 方块回收池
		// 当用户消除了一些方块时，那些方块不会被删除，而是暂时回到回收池
		this.tilePool = [];
	}
}
```

有了这些状态之后，我们就可以预加载图片并且在画面上显示所有的方块了。  
预加载的代码我先省略掉。  

主要看 InGame 游戏状态的create方法

```JavaScript
class InGame {
	preload() {
		// 预加载代码
		// 例如：this.game.load.image('tile', './tile.png')
	}
	create() {
		// 这里一般会有缩放代码来适配各种屏幕
		this.createLv();
	}
	createLv() {
		for (let i = 0; i < opts.rows; i++) {
			this.tilesArray[i] = [];
			for (let j = 0; j < opts.cols; j++) {
				this.addTile(i, j);
			}
		}		
	}
	addTile(row, col) {
		const tile = this.makeTile(row, col);

		this.tilesArray[row][col] = {
			tileSprite: tile,
			isEmpty: false,
			coordinate: new Point(row, col),
			tint: tile.tint
		};
	}

	makeTile(row, col) {
		const left = (col + 0.5) * opts.tileSize;
		const top = (row + 0.5) * opts.tileSize;

		const theTile = this.game.add.sprite(left, top, 'tiles');

		theTile.anchor.set(0.5);
		theTile.width = opts.tileSize;
		theTile.height = opts.tileSize;

		const colorIndex = this.game.rnd.integerInRange(0, opts.colors.length - 1);

		theTile.tint = opts.colors[colorIndex];

		return theTile;
	}
}
```

这个 opts 定量里面有设定游戏有多少行和列，  
所以 createLv 方法先遍历行，再遍历列，然后对对应的行和列添加方块  
makeTile 方法会生成对应的方块并添加到游戏当中  
注意：
* 方块的中心在方块的中间，所以计算位置时会有 + 0.5
* 每个方块有一个随机的颜色，用的是 Phaser 的 rnd.integerInRange 方法
最后在 addTile 方法里面，我把生成的 sprite ，相应的位置，还有颜色添加到了 tilesArray 里面

这时打开游戏，已经能看到所有的方块已经显示出来了。

但是，现在点击这些方块还没有任何效果。  

所以，我们需要对用户的点击操作进行处理：

```JavaScript
class InGame {
	createLv() {
		// 遍历添加方块的代码省略
		this.game.input.onDown.add(this.pickTile, this);
	}

	pickTile(e) {
		if (this.canPick) {
			const posX = e.x;
			const posY = e.y;

			// 这里的 e 是事件对象
			// 事件触发的 x 决定触发在哪个列
			// 事件触发的 y 决定触发在哪个行
			// 一定要小心并弄清它们的关系
			const pickedRow = Math.floor(posY / opts.tileSize);
			const pickedCol = Math.floor(posX / opts.tileSize);

			// isValidTile 函数检查用户的点击是否在合法范围内，略。
			if (this.isValidTile(pickedRow, pickedCol)) {
				const pickedTile = this.tilesArray[pickedRow][pickedCol];
				// 真正的消除逻辑
				this.tryPick(pickedTile);
			}
		}
	}

	tryPick(theTile) {
		const fill = [];
		this.floodFill(theTile.coordinate, theTile.tint, fill);

		if (fill.length > 2) {
			this.destroyTiles(fill);
		}
	}
}
```

当用户点击鼠标左键时，会触发回调 pickTile 函数，具体逻辑在注释中有。  

如果用户的点击合法，就调用 tryPick 函数。  
这个函数初始化了一个 fill 变量，保存这次点击会影响到的方块。  
之后有一个 floodFill 的方法，它会递归查找受影响的方块。  

最后检查受影响的方块的数量，如果是3个或更多，就可以消除它们，即三消。  

那就来看看 floodFill 是如何递归的：

```JavaScript
class InGame {
	floodFill(point, color, fillArr) {
		const {x, y} = point;
		if (!this.isValidTile(x, y)) {
			return;
		}
		if (this.tilesArray[x][y].isEmpty) {
			return;
		}
		if (this.pointInArr(fillArr, point)) {
			return;
		}
		if (this.tilesArray[x][y].tint === color) {
			fillArr.push(point);
			this.floodFill(new Point(x + 1, y), color, fillArr);
			this.floodFill(new Point(x - 1, y), color, fillArr);
			this.floodFill(new Point(x, y + 1), color, fillArr);
			this.floodFill(new Point(x, y - 1), color, fillArr);
		}
	}

	pointInArr(arr, point) {
		return arr.some(myPoint => myPoint.equals(point));
	}
}
```

这个函数有3个参数，第一个是初始点，第二个是初始颜色，第三个是受影响的点的数组。  

之后会做一些条件判定，包括：
- 递归的点是否合法，比如当前的点是(0, 0)， 之后会向(-1, 0) 等非法点递归
- 检查当前的格子是否为空
- 检查当前的点是否已经被加入到了数组中，这里用的是数组的 Array.some 方法
- 当前颜色是否和初始点的颜色一样
如果符合条件，会把当前的点加入到数组中，并向4个方向递归，这也是 windows 中“画图”应用中的“填充”功能的递归方法。  

当我们知道所有同样颜色的方块之后，就可以消除它们了。  

在 Phaser 官方的教程中，如果你碰到了一个星星，你就使用 kill 方法移除那个星星的 sprite  
但那不一定是最好的选择，来看我是如果移除方块的：

```JavaScript
class InGame {
	destroyTiles(tileList) {
		// 禁止用户在动画中点击方块
		this.canPick = false;

		tileList.forEach(point => {
			// 按位置查找需要消除的方块
			const tile = this.tilesArray[point.x][point.y];
			// 不需要真的移除这些方块，而是让他们的透明度变为0
			// 这里是用动画让透明度逐渐变化
			const tween = this.game.add.tween(tile.tileSprite).to({
				alpha: 0
			}, 300, Phaser.Easing.Linear.None, true);
			// 被“移除”的方块回到池中
			this.tilePool.push(tile.tileSprite);
			tween.onComplete.add(this.fillHoles, this);
			// 当前的方块被标记为空
			tile.isEmpty = true;
		});
	}
}
```

当动画完成后，执行 fillHoles 回调函数，并用重复利用池中的方块。  

代码我就不在这里展示了，因为都是一些面向过程的代码，而且比较长。

项目在这里： [same](https://github.com/steambap/same)
