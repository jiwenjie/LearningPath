1.自定义ListView
解决:重写其中的onMeasure()方法

原因:
ScrollView默认把Childview设置为UNSPEFEIED模式,而该模式下的ListView给自己的测量的高度就是第一个item的高度.

原理:
int expandSpec = MeasureSpec.makeMeasureSpec(Integer.MAX_VALUE >> 2, MeasureSpec.AT_MOST);
这个方法的作用是根据大小和模式来生成一个int值,这个int值封装了模式和大小信息.
首先MeasureSpec类是View的一个静态内部类,MeasureSpec类封装了从父布局到子布局传递的布局需求.
每个MeasureSpec对象代表了宽度和高度的要求.
MeasureSpec用int类型表示,前2位代表模式,后30位代表大小.
第一个参数Integer.MAX_VALUE >> 2:
Integer.MAX_VALUE获取到int的最大值,但是表示大小的值size是int数值的底30位,所以把这个值右移两位,留出高两位表示布局模式.
此时这个值仍旧是一个30位所能表示的最大数,用该数作为控件的size,应该足够满足控件大小的需求.
第二个参数MeasureSpec.AT_MOST:
表示这个控件适配父控件的最大空间.

(以下三种仅供参考,不推荐使用)
2.手动设置ListView高度
3.使用单个ListView取代ScrollView中所有内容
4.使用LinearLayout取代ListView

参考资料:
https://juejin.im/entry/5979ab4d5188253e3271c953
https://juejin.im/post/5a322cbf6fb9a045204c3da1
