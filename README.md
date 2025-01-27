# tradingview-strategy-crypto
tradingview strategy crypto only - 加密货币量化交易策略 Tradingview pine 脚本策略 比特币 以太坊 加密货币 自动交易 策略 机器人 binance okx 交易所 币安 欧易 Bitget 

----------------------------------------------------------------------------
10.7 更新
实时跟踪测试發現了一些结论，追踪止盈止损非常容易使你的回测结果产生错误的订单。因为放大镜的存在，下面有说。目前的结论是30m以下（对应15s级别的放大镜K线）可以保证一定的准确率，前提是参数幅度和我的差不多，我设在了在0.5%-1%左右，而30m以上可能存在虚假订单产生，后面越往大误差越大，需要你的参数幅度跟随增加，否则回测结果没有多少意义都是假的。因此后面的2h测试结果可以认为有很大水分，现在继续用29m周期测试观察效果
----------------------------------------------------------------------------

先介绍这个策略：
* 策略核心逻辑是：突破进场试错，跟踪止盈止损。
* 核心参数只有5个，分别是信号过滤幅度、入场过滤幅度、止损幅度、跟踪止盈和止损幅度。
* 此策略是Tradingview脚本策略，可以直接在TV平台回测，回测环境：**K线放大镜开启(不开等于自己骗自己，对某些策略来说结果没有任何参考价值)**，手续费0.05%，多空均开，但同时最多只开一个单。
* 回测结论如下：
  + 经过调参测试，发现只适用于加密货币类别，即24小时开市高波动型趋势品种。其他品种不适用。这是由策略本身的逻辑和参数决定的。
  + 太大的周期可能会导致结果失真、交易频率降低，太小的周期可能会导致手续费增加，这里使用的是2h周期，交易频率适中，一个月交易次数在几次到十几次之间，回测了几十个市值、人气排前的币种，BTC,ETH,SOL,BNB,BCH,PEPE,SHIB,FIL,UMA,BSV...不列举了，几十个也足够了。
  + 下图是2020-至今的其中几个币种的2h回测结果：开单额固定便于查看亏损情况
![图片](https://github.com/user-attachments/assets/c320a5d9-8647-4eb3-98b9-700b65cd1a88)
![图片](https://github.com/user-attachments/assets/26123645-79c3-47a2-9608-cf7bb5580b49)

开单额固定百分比，随余额变化：
![图片](https://github.com/user-attachments/assets/fd176ff6-591d-410c-b1ef-fa04d0ae879e)
![图片](https://github.com/user-attachments/assets/86439b8a-7ee5-4241-b3e2-612f97f5a4bb)

介绍K线放大镜：https://www.tradingview.com/blog/cn/accurate-backtesting-with-bar-magnifier-31746/
起初我是根本不相信这样的结果的，但实际了解pine脚本的运行机制和K线放大镜的作用后，我尝试着进行了手动测试比对、前向测试等，由于2h的K线放大镜精度是5m，而我的参数设置是0.5%，因此只要不是在1m精度的k线上产生了离谱的上下来回变化超过0.5%以上都是真实的结果，具体不说明了，简而言之我手动比对了一些数据大部分是真实的可执行的，但少数波动比较离谱，比如上图的一笔赚10%以上的基本都是假的单子，1m内波动1%以上这种就无法确保真实性。

+ 此外，2h周期对应的放大镜是5m，因此仍然有一定误差存在，可以尝试15m、30m（对应的分别是15s和1m），效果都不错，下图是pepe15m，最近半年左右回测15m周期，可以发现是可行的，除非说15s内K线上下动0.5%以上，这种属于极少数了吧。
+ ![图片](https://github.com/user-attachments/assets/7a041433-b585-4527-85ab-3a8425b2b519)

![图片](https://github.com/user-attachments/assets/4f46ec8d-4610-4768-834a-f2b238fdcca8)

OKX可以无缝接入TV的策略，观察实际测试结果。我已经接到平台上自动交易，等一段时间查看实际结果。这一步如果比较符合实际回测结果那么基本上可以认为策略是可行的。
如果需要此策略，欢迎联系我试用 leifetbastidblpfci@gmail.com ，我开放权限你就可以自行查看回测结果、自己手动测试、前向测试、改参数等等，但唯一无法查看源代码，到期后无法使用
（代码中无未来函数、重绘等自欺欺人操作，唯一的失真性源自放大镜精度的不够）
