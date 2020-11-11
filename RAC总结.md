# RACCommand

头文件中，RACCommand 对外暴露了5个属性，作用分别是：

executionSignals：高阶信号，当执行 -execute: 方法的时候，会创建新的信号，这个信号通过 executionSignals 发送出去
executing：发送的是布尔值，标志着 RACCommand 是否正在执行中，信号值是布尔值
enabled：标志 RACCommand 是否可以执行 -execute:
errors：执行 -execute: 方法，过程中出现的 error 都由此发送
allowsConcurrentExecution：是否允许 RACCommand 并发执行，atomic 修饰，getter/setter方法是原子性的

两种初始化方法
```
- (instancetype)initWithSignalBlock:(RACSignal<id> * (^)(id input))signalBlock
- (instancetype)initWithEnabled:(RACSignal *)enabledSignal signalBlock:(RACSignal<id> * (^)(id input))signalBlock
```

示例
```
RACCommand *command = [[RACCommand alloc] initWithSignalBlock:^RACSignal * _Nonnull(NSNumber * _Nullable input) {
    return [RACSignal createSignal:^RACDisposable * _Nullable(id<RACSubscriber>  _Nonnull subscriber) {
        NSInteger integer = [input integerValue];
        for (NSInteger i = 0; i < integer; i++) {
            [subscriber sendNext:@(i)];
        }
        [subscriber sendCompleted];
        return nil;
    }];
}];
[[command.executionSignals switchToLatest] subscribeNext:^(id  _Nullable x) {
    NSLog(@"%@", x);
}];

[command execute:@1];
[RACScheduler.mainThreadScheduler afterDelay:0.1
                                    schedule:^{
                                        [command execute:@2];
                                    }];
[RACScheduler.mainThreadScheduler afterDelay:0.2
                                    schedule:^{
                                        [command execute:@3];
                                    }];
```

个人理解：
RACCommand需要执行，然后才能创建信号，不执行信号也不会执行。


# RACSignal
