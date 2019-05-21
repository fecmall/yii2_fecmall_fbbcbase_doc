订单取消脚本
=====


当用户下订单，会扣除产品库存，如果用户在一段时间内未支付，将会占用产品库存，因此，
系统通过脚本，将一段时间内未支付的订单自动取消


脚本：`@fecmall/shell/order/returnPendingProductQtyStock.sh`

对应的函数为：

```
Yii::$service->order->returnPendingStock()
```


您可以通过设置 `Yii::$service->order->minuteBeforeThatReturnPendingStock`(在配置文件中设置)，
来决定多长分钟未支付的订单，进行订单取消操作，释放库存

您需要通过cron，定时跑这个脚本

关于console执行，可以参看文档：[Fecshop console 介绍和配置](http://www.fecshop.com/doc/fecshop-guide/develop/cn-1.0/guide-fecshop-console-about.html)


