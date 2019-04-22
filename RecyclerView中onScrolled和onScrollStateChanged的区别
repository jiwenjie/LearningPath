
出现场景：RecyclerView横向滑动，每一个Item占据满屏。需要在外部实现索引，用户滑动的时候动态更新当前的item位置信息。
    需要在item滑动结束后自动把最后的一个居中显示（可以使用LinearSnapHelper或者PagerSnapHelper）
    区别： PagerSnapHelper 一次只能滑动一页，适合总体Item数量不多的情况
          LinearSnapHelper 的滚动速度较快，适合Item数量较多的情况，但是用户体验不是特别好。推荐使用 PagerSnapHelper

实现方式：
    毫无疑问设置RecyclerView的滚动监听事件，通过其LayoutManager的findFirstVisibleItemPosition来获取当前的
    位置信息并设置到TextView中。因为主管要求用户滑动后让它跟随滑动，所以这里选用 LinearSnapHelper.
    
结果：当使用 PagerSnapHelper 的时候在 onScrolled和onScrollStateChanged 中没有区别,每次滑动后索引的更新没什么问题。
    当使用 LinearSnapHelper 的时候 在 onScrolled 方法中设置索引更新可以在滚动中改变索引的值。在onScrollStateChanged中只有停止滚动的
    时候才能够更新当前的索引值，在用户手指离开界面但是滚动未停止的时候无法更新。
    
    根据日志和查询资料发现，onScrolled 事件会一直触发响应，只要界面一滚动。但是 onScrollStateChanged 是根据状态来判断。它的
    内部分为 /**
              * The RecyclerView is not currently scrolling.（静止没有滚动）
              */
             public static final int SCROLL_STATE_IDLE = 0;

            /**
             * The RecyclerView is currently being dragged by outside input such as user touch input.
             *（正在被外部拖拽,一般为用户正在用手指滚动）
             */
            public static final int SCROLL_STATE_DRAGGING = 1;

            /**
             * The RecyclerView is currently animating to a final position while not under outside control.
             *（自动滚动）
             */
            public static final int SCROLL_STATE_SETTLING = 2;  三种
            
            因为不是一直在响应监听，所以会出现只有停止滚动的时候才会更新值的情况。



