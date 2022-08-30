# CollectionView: 为什么numberOfItemsInSection执行了并返回count>0,cellForItemAtIndexPath缺不执行count次

直接原因: collectionView的在执行上述代码的时候,frame不够展示count个item

悬疑现象: 
1. 在使用masonry布局的时候. cell还是zeroFrame, cell里包着collectionView
2. 此时collectionView还是zeroFrame, 0Width或者是0Height
3. 然后去做reloadData, 就会出现上述现象

解决:
1. collectionView的width或者height, 不跟cell走. 直接写死约束, width = ScreenWidth
