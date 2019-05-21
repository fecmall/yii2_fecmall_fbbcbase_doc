FecMall用户取消订单
==============

> 订单创建后，用户进行订单取消的操作



### 订单取消操作


用户可以在`账户中心`,
订单管理功能页面，点击`取消`按钮

![xx](images/order-4.png)

1.如果订单可以`直接取消`，那么不需要经销商审核，也就是上面
`订单取消`类型的第一种,
提交后，`订单直接取消`，如果订单支付为在线支付，并且支付成功，那么会发生
订单`退款`，需要在后台进行`订单取消`退款（目前退款为`线下退款`）


2.如果订单取消发起后，需要经销商审核

2.1`订单取消`请求发起后，如果用户想撤回该`请求`，用户可以点击订单`撤销取消`请求，
来撤回该请求

![xx](images/order-5.png)

2.2订单取消请求发起后，经销商在后台可以看到该请求

![xx](images/order-6.png)

经销商勾选处理的`订单取消`请求，选择`订单取消通过`还是`订单取消拒绝`

如果点击的是`订单取消通过`，那么订单将会被`取消`，
如果订单支付为在线支付，那么会发生
`订单退款`


如果点击的是`订单取消拒绝`，那么`订单取消`请求将会被驳回,
订单将继续按照后续的处理流程继续处理。

3.订单发货后，用户将不能发起`订单取消请求`

`订单取消请求`还没有审核的时候，订单无法进行其他处理（
也就是无法进行后续的订单发货操作），只能等
`订单取消审核`处理完成。

4.订单取消后
产品将会返还`库存`，
订单取消之后，代表订单终结，`订单取消`操作成功后，订单不可以进行其他的操作。

5.订单取消后，如果订单是在线支付（收款方为平台），
那么平台需要进行订单退款操作。

![xx](images/order-7.png)

线下退款完成后，平台商在后台更改退款状态。


### 订单取消类型

`订单取消`，指的是用户下单后，进行订单取消的操作
，下面是用户（customer）在哪些订单状态下，可以进行订单取消的操作：

```
Yii::$service->order->payment_status_pending,    // 订单创建状态（未支付）
Yii::$service->order->payment_status_processing,  //  订单支付中状态
Yii::$service->order->payment_status_canceled,    // 订单支付取消状态
Yii::$service->order->payment_no_need_status_confirmed,  // 订单-货到付款支付方式，确认
Yii::$service->order->payment_status_confirmed,  // 订单支付完成
Yii::$service->order->status_audit_fail,   // 订单内容审核失败
Yii::$service->order->status_processing   // 订单内容审核通过，备货中状态
```

根据订单的状态，有的状态的订单可以直接取消，不需要经销商审核，有的需要经销商审核。

1.用户直接发起订单取消请求后，不需要经销商确认，
`直接订单取消`成功的情况

当订单创建，支付，以及审核失败等状态，详细参看代码：

```
Yii::$service->order->info->orderStatusRedirectCancelArr = [
    Yii::$service->order->payment_status_pending,    // 订单创建状态（未支付）
    Yii::$service->order->payment_status_processing,  //  订单支付中状态
    Yii::$service->order->payment_status_canceled,    // 订单支付取消状态
    Yii::$service->order->payment_no_need_status_confirmed,  // 订单-货到付款支付方式，确认
    Yii::$service->order->payment_status_confirmed,  // 订单支付完成
    Yii::$service->order->status_audit_fail,   // 订单内容审核失败
]
```

当订单状态在上面的状态范围中，那么，当用户发起订单取消操作后，订单会被直接取消，
进行产品库存的返还，如果订单已经被在线支付，那么会进行退款处理（平台进行收款，因此由平台进行退款）。

详细代码参看：

```
Yii::$service->order->process->redirectCancel($orderModel)
```


2.订单取消发起后，需要`经销商`确认

用户发起`订单取消`请求，需要
经销商审核的情况

```
$this->orderStatusRequestCancelArr = [
    Yii::$service->order->status_processing,   // 订单审核通过，备货中
];
```   

当订单审核通过后，订单进入备货状态中，如果在该状态，用户发起`订单取消`操作，
那么需要经销商后台审核，如果进行`订单取消通过`操作，那么订单将会被取消，
如果进行`订单取消拒绝`操作（譬如已经发货了，无法取消），那么`订单取消请求`将会被拒绝，经销商可以对订单进行正常
流程的发货操作


### 脚本取消订单

对于用户发起的订单，在一段时间内没有支付，系统脚本将会将这部分订单取消掉














