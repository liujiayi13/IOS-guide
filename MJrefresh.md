# MJrefresh

刷新控件用的比较多，多了解了一下用法
![image](https://upload-images.jianshu.io/upload_images/1805486-6e3ad96c401ef562.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

```
1.MJrefresh的继承关系
在开发中，我们用到的就是后面红色标记的类，如果项目有特殊需求，可以继承前面的类，来进行个性化的开发
MJRefreshNormalHeader （默认的下拉刷新控件）
MJRefreshBackNormalFooter （默认的上拉加载控件，提示一直贴在tableView的尾部）
MJRefreshAutoNormalFooter （默认的上拉加载控件，提示的语句跟着cell走）
2.基本使用方法
//直接调用默认的刷新控件
//单独写回调方法
self.tableView.mj_header = [MJRefreshNormalHeader headerWithRefreshingTarget:self refreshingAction:nil];
//刷新事件直接写在block中
self.tableView.mj_header = [MJRefreshNormalHeader headerWithRefreshingBlock:^{
              
    }];

①刷新的触发事件：cancelClick
MJRefreshNormalHeader *header = [MJRefreshNormalHeader headerWithRefreshingTarget:self refreshingAction:nil];

MJRefreshAutoNormalFooter *footder = [MJRefreshAutoNormalFooter footerWithRefreshingTarget:self refreshingAction:@selector(cancelClick)];

MJRefreshBackNormalFooter *footder1 = [MJRefreshBackNormalFooter footerWithRefreshingTarget:self refreshingAction:@selector(cancelClick)];

创建好视图之后，可以做一些小小的修改
1.休息时候的状态
 [header setTitle:@"蓄势待发" forState:MJRefreshStateIdle];
 2.已经开始刷新的状态
[header setTitle:@"我已经飙到了极限" forState:MJRefreshStateRefreshing];   
3.即将开始刷新的状态
[header setTitle:@"come on baby" forState:MJRefreshStatePulling];

//把创建好的视图赋值给MJrefresh
self.tableView.mj_footer = footder;

②block
self.tableView.mj_header = [MJRefreshNormalHeader headerWithRefreshingBlock:^{
NSLog(@"下拉=========");
}];

self.tableView.mj_footer = [MJRefreshBackNormalFooter footerWithRefreshingBlock:^{
NSLog(@"上拉=========");
}];
3.GIF动图效果
需要添加图片的数组
调用的方法没有区别，就是换一下调用的类，同样是后面默认的几个红色标记的带GIF的类

//样式一：设置一张图片（无动画效果）
//    NSArray *idleImages = [NSArray arrayWithObject:[UIImage imageNamed:@"xiala_icon.png"]];
    //样式二：设置多张图片（有动画效果）
    NSArray *idleImages = [NSArray arrayWithObjects:
                           [UIImage imageNamed:@"dropdown_loading_01.png"],
                           [UIImage imageNamed:@"dropdown_loading_02.png"],
                           [UIImage imageNamed:@"dropdown_loading_03.png"],nil];

    NSArray *pullingImages = [NSArray arrayWithObject:[UIImage imageNamed:@"shifang_icon.png"]];
    NSArray *refreshingImages = [NSArray arrayWithObjects:
                                 [UIImage imageNamed:@"load_view_01.png"],
                                 [UIImage imageNamed:@"load_view_02.png"],
                                 [UIImage imageNamed:@"load_view_03.png"],
                                 [UIImage imageNamed:@"load_view_04.png"],
                                 [UIImage imageNamed:@"load_view_05.png"],
                                 [UIImage imageNamed:@"load_view_06.png"],
                                 [UIImage imageNamed:@"load_view_07.png"],
                                 [UIImage imageNamed:@"load_view_08.png"],
                                 [UIImage imageNamed:@"load_view_09.png"],
                                 [UIImage imageNamed:@"load_view_010.png"], nil];

//    MJRefreshGifHeader *header = [MJRefreshGifHeader headerWithRefreshingTarget:self refreshingAction:@selector(animationRefresh)];

    //-------以下是使用block方法【不包含animationRefresh方法】,动画设置在上面的部分代码---------

    __weak typeof(self) weakSelf = self;
    MJRefreshGifHeader *header = [MJRefreshGifHeader headerWithRefreshingBlock:^{
        [weakSelf getNetworkData:YES];
    }];

    //-------以上是使用block方法【不包含animationRefresh方法】,动画设置在上面的部分代码---------

    //1.设置普通状态的动画图片
    [header setImages:idleImages forState:MJRefreshStateIdle];
    //2.设置即将刷新状态的动画图片（一松开就会刷新的状态）
    [header setImages:pullingImages forState:MJRefreshStatePulling];
    //3.设置正在刷新状态的动画图片
    [header setImages:refreshingImages forState:MJRefreshStateRefreshing];

    self.tableView.mj_header = header;

//透明度自适应，随着下拉的力度，自动隐藏和显示
header.automaticallyChangeAlpha = YES;
//进入刷新状态
[self.tableView.mj_header beginRefreshing];
//结束控件的刷新
[self.tableView.mj_footer endRefreshing];
//显示加载完全部数据，上拉不会在刷新
[self.tableView.mj_footer endRefreshingWithNoMoreData];
//刷新完毕之后，可以继续上啦刷新
[self.tableView.mj_footer resetNoMoreData];


```

MJRefreshBackNormalFooter 一直在屏幕最下方显示刷新的过程，刷新完回弹
MJRefreshAutoNormalFooter  向下拖动的过程中就会在当前最后一个cell下面显示刷新的过程

