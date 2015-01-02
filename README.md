#ZHDynamicCollectionView

## Why
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