# SHSlideUnlock
高仿iPhone屏幕上的“滑动来解锁”动画效果以及滑动,颜色字体自定义
- (void)viewDidLoad {
    [super viewDidLoad];
    
    [self setScrollView];
    [self animationUnlockLabel];
}

#pragma mark - 设置所有ScrollView方法
#pragma mark -
- (void)setScrollView
{
    [self.view addSubview:self.scrollView];
    [self.scrollView addSubview:self.firstView];
    [self.scrollView addSubview:self.secondView];
    self.scrollView.contentSize = CGSizeMake(SHPageCount * kMainScreenW, 0);
}

#pragma mark - getter
#pragma mark -
- (UIScrollView *)scrollView
{
    if (!_scrollView) {
        
        _scrollView = [[UIScrollView alloc]initWithFrame:self.view.bounds];
        _scrollView.showsHorizontalScrollIndicator = NO;
        _scrollView.showsVerticalScrollIndicator = NO;
        _scrollView.pagingEnabled = YES;//启动分页
        _scrollView.delegate = self;
    }
    return _scrollView;
}

- (UIImageView *)firstView
{
    if (!_firstView) {
        _firstView = [[UIImageView alloc]init];
        _firstView.image = [UIImage imageNamed:@"guide_image1"];
        _firstView.frame = CGRectMake(-kMainScreenW, 0, kMainScreenW, KMainScreenH);
    }
    return _firstView;
}

- (UIImageView *)secondView
{
    if (!_secondView) {
        _secondView = [[UIImageView alloc] init];
        _secondView.image = [UIImage imageNamed:@"guide_image2"];
        _secondView.frame = CGRectMake(0, 0, kMainScreenW, KMainScreenH);
        _secondView.userInteractionEnabled = YES;
        
    }
    
    return _secondView;
}

#pragma mark - 代理- 将要结束滑动
#pragma mark -
- (void)scrollViewWillEndDragging:(UIScrollView *)scrollView withVelocity:(CGPoint)velocity targetContentOffset:(inout CGPoint *)targetContentOffset{
    
    [UIView animateWithDuration:0.3
                     animations:^{
                         _firstView.frame = CGRectMake(0, 0, kMainScreenW, KMainScreenH);
                         
                         _secondView.frame = CGRectMake(kMainScreenW, 0, kMainScreenW, KMainScreenH);
                     }];
    
}




#pragma mark - 设置所有动画解锁方法
#pragma mark -
- (void)animationUnlockLabel
{
    
    // gradientLayer
    CAGradientLayer *gradientLayer = [CAGradientLayer layer];
    gradientLayer.frame = CGRectMake(0, KMainScreenH-100, kMainScreenW, 64);
    gradientLayer.colors = @[(__bridge id)[UIColor redColor].CGColor,
                             (__bridge id)[UIColor yellowColor].CGColor,
                             (__bridge id)[UIColor purpleColor].CGColor];
    gradientLayer.locations = @[@0.25,@0.5,@0.75];
    gradientLayer.startPoint = CGPointMake(0, 0);
    gradientLayer.endPoint = CGPointMake(1, 0);
    [self.secondView.layer addSublayer:gradientLayer];
    
    // 初始化 unlockLabel
    self.unlockLabel = [[UILabel alloc] initWithFrame:gradientLayer.bounds];
    _unlockLabel.text = @"> 滑动来解锁";
    _unlockLabel.textAlignment = NSTextAlignmentCenter;
    _unlockLabel.font = [UIFont boldSystemFontOfSize:24];
    _unlockLabel.textColor = [UIColor whiteColor];
    _unlockLabel.userInteractionEnabled = YES;
    gradientLayer.mask = _unlockLabel.layer;
    
    // 添加点按手势
    UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(slideUnlockClick:)];
    [self.view addGestureRecognizer:tap];
    
    //执行动画 animation
    CABasicAnimation *animation = [CABasicAnimation animationWithKeyPath:@"locations"];
    animation.fromValue = @[@0,@0,@0.25];
    animation.toValue = @[@0.75,@1,@1];
    animation.repeatCount = MAXFLOAT;
    animation.duration = 2.5f;
    [gradientLayer addAnimation:animation forKey:nil];
    
}

- (void)slideUnlockClick:(UITapGestureRecognizer *)tap
{
    SHTestViewController *testVC = [[SHTestViewController alloc]init];
    [self presentViewController:testVC animated:YES completion:nil];
}

@end
