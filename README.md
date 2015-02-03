#ZHDynamicCollectionView

## Why

Extremely useful for dynamic collectionView cell size, see this project [Dynamic-Collection-View-Cell-With-Auto-Layout-Demo](https://github.com/honghaoz/Dynamic-Collection-View-Cell-With-Auto-Layout-Demo)

For dynamic collection view cell, in sizeForItemAtIndexPath, we need to 
initialize an off screen cell and use it to calculate cell's size.

Normally, we will create an dictionary to maintain off screen cells, like this:

In collectionView controller:

```
var offscreenCells = Dictionary<String, UICollectionViewCell>()
```

In 
`collectionView:layout:sizeForItemAtIndexPath:`

```
var cell: MyCollectionViewCell? = collectionVC.offscreenCells[reuseIdentifier] as? MyCollectionViewCell
if cell == nil {
	cell = NSBundle.mainBundle().loadNibNamed("MyCollectionViewCell", owner: nil, options: nil)[0] as? MyCollectionViewCell
	collectionVC.offscreenCells[reuseIdentifier] = cell
}
```

This kind of code is too trival.

## How to use

Register cells as usual:

```
collectionView.registerClass(MyCollectionViewCell.self, forCellWithReuseIdentifier: kCellResueIdentifier)
```
or

```
var cellNib = UINib(nibName: "MyCollectionViewCell", bundle: nil)
collectionView.registerNib(cellNib, forCellWithReuseIdentifier: kCellResueIdentifier)
```

In `collectionView:layout:sizeForItemAtIndexPath:`

```
var helperCell = collectionView.dequeueReusableOffScreenCellWithReuseIdentifier(kCellResueIdentifier) as MyCollectionViewCell

var size = helperCell.contentView.systemLayoutSizeFittingSize(UILayoutFittingCompressedSize)
return size
```

Only One new API: 

`- dequeueReusableOffScreenCellWithReuseIdentifier(identifier: String) -> UICollectionViewCell?`


# The MIT License (MIT)

The MIT License (MIT)

Copyright (c) 2015 Honghao Zhang 张宏昊

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

