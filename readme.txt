index.html多尺寸图片左右切换

1.一次点击按钮，向左或向右移动一个图片
2.切换到尽头时不显示按钮
3.页面有三个尺寸


根据图，向右切换的时候，margin需要向左移动的距离为（li的width+2*border+margin-right）
width+2*border在jquery中可用用outerWidth()获取
margin-right的值在jquery中用css("marginRight")获取


思路：总共就一个响应事件，就是点击按钮，以next按钮为例。点按钮发生了哪些事件。点击按钮—》ul产生位移—》更新按钮状态。或者点击按钮—》更新按钮状态—》ul产生位移。顺序可以改变


第一步：分析ul产生位移的过程。
抽象2个变量

now：表示当前页面第一个展示位展示的是第几个li，默认为1，向右切换now++，向左切换now--
offset：表示i的width+2*border+margin-right
这样点击按钮时的响应就变成了设置margin-left为-(now-1)*offset
因为页面有三个尺寸，所以点击按钮时ul产生位移时需要根据尺寸计算出offset的值


第二步，分析更新按钮状态的过程。
分析一下这句话“切换到尽头时不显示按钮”，页面默认只显示向右的按钮，切换到到最左边只显示向左的按钮，中间过程两个按钮都要显示。

抽象出几个变量：
linum:总共有几个li，一次切换一个。
shownum:页面需要展示几个li。
now：当前页面第一个展示位展示的是第几个li，默认为1，向右切换now++，向左切换now--。

分析几种情况：

如果li的总数小于要展示的数，即linum<shownum，pre和next按钮都不显示。
初始化，判断now为1，只显示next按钮。
展示到最后shownum个li的时候，即判断now==linum-shownum+1时，只显示next按钮。
否则pre和next按钮都显示。
因为页面有三个尺寸，所以点击按钮更新按钮状态时需要根据尺寸计算出shownum的值。


第三步：提取方法
把第一步和第二步中涉及通过尺寸获取信息的部分封装成一个函数getInfo
第二步更新按钮状态的封装成一个函数btnShow
第一步中给按钮绑定事件的过程封装成一个函数bindBtn

同时要注意，页面缩放的时候，需要重置一些状态。


相关
动画中

wrap.stop(true).animate（）
把当前元素涉及的所有动画都停止了，让后再触发。否则当页面快速resize，快速点击按钮多次，会产生“动画积累”，触发了下一个动画，之前的动画还没完，就会有一个延迟，影响用户体验。

stop()参数说明：

$(selector).stop(stopAll,goToEnd)
stopAll，可选，规定是否停止被选元素的所有加入队列的动画。
goToEnd，可选，规定是否允许完成当前的动画。该参数只能在设置了stopAll参数时使用。
具体用法：

stop(true)等价于stop(true,false): 停止被选元素的所有加入队列的动画。
stop(true,true):停止被选元素的所有加入队列的动画，但允许完成当前动画。
stop()等价于stop(false,false):停止被选元素当前的动画，但允许完成以后队列的所有动画。
stop(false,true):立即结束当前的动画到最终效果，然后完成以后队列的所有动画。