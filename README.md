# 高频跨期套利策略

## 策略说明

交易标的：比特币（BTC）
价差数据：BTC 永续 - BTC 季度（省略协整性检验）
交易周期：1 分钟
头寸匹配：1:1
交易类型：同品种跨期

做多价差开仓条件：如果当前账户没有持仓，并且价差 < （长期价差水平 - 阈值），就做多价差。即：买开 BTC 永续，卖开 BTC 季度。

做空价差开仓条件：如果当前账户没有持仓，并且价差 > （长期价差水平 + 阈值），就做空价差。即：卖开 BTC 永续，买开 BTC 季度。

做多价差平仓条件：如果当前账户持有 BTC 永续多单，并且持有 BTC 季度空单，并且价差 > 长期价差水平，就平多价差。即：卖平 BTC 永续，买平 BTC 季度。

做空价差平仓条件：如果当前账户持有 BTC 永续空单，并且持有 BTC 季度多单，并且价差 < 长期价差水平，就平空价差。即：买平 BTC 永续，卖平 BTC 季度

**举个例子**，假设 BTC 永续 和 BTC 当季的价差长期维持在 35 左右。如果某一天价差达到 50 ，我们预计价差会在未来某段时间回归到 35 及以下。那么就可以卖出 BTC 永续，同时买入 BTC 当季，来做空这个价差。反之亦然，注意 BTC 永续 和 BTC 当季 的价差总会回归到0附近（到期交割），所以价差为正的时候，优先做空价差，价差为负的时候，优先做多价差

**请注意，该策略仅是一个简单的交易策略，请不要在实际环境直接使用，否则，你必然会亏钱！**

**不过，如果你想在实际环境中利用策略获得盈利，我们希望你能够在sandbox环境配合其他参数或是策略进行测试调整，以使你能够获得盈利。当然，如果这个过程中，你遇到任何问题需要帮助亦或是有好的想法想要分享，请在ISSUE中反映，我们会努力及时响应。**

## 如何使用

* 克隆该策略项目至本地后，安装依赖：

  ```shell script
  pip install python-kumex
  ```

* 复制config.json.example，并重命名为config.json，然后完善相关的配置信息

  ```json
  {  
    "api_key": "api key",
    "api_secret": "api secret",
    "api_passphrase": "api pass phrase",
    // 是否是沙盒环境
    "is_sandbox": true,
    // 合约名称，比如：XBTUSDM
    "symbol_a": "contract name",
    // 合约名称，比如：XBTMM20
    "symbol_b": "contract name",
    // 长期价差水平，可取前三天的价差均值  
    "spread_mean": "average closed price for 3 days",
    // 价差阈值，可取前三天的价差2倍标准差左右
    "num_param": "2 * Standard deviation of spread_mean",
    // 杠杆倍数，比如：5
    "leverage": "Leverage of the order",
    // 开仓数量，比如：1
    "size": "Order size. Must be a positive number"
  }
  ```

  

* 让你的策略运行起来：

  ```shell
  ./high_frequency.py
  ```

  