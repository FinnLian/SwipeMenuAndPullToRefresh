# SwipeMenuAndPullToRefresh

ListView和ExpandableListView支持下拉刷新，上拉加载和侧滑删除三个功能;
Demo:
----
<p>
   <img src="https://github.com/heynchy/SwipeMenuAndPullToRefresh/blob/master/ScreenShot/1.gif" width="350" alt="Screenshot"/>
</p>
<p>
   <img src="https://github.com/heynchy/SwipeMenuAndPullToRefresh/blob/master/ScreenShot/2.gif" width="350" alt="Screenshot"/>
</p>
<p>
   <img src="https://github.com/heynchy/SwipeMenuAndPullToRefresh/blob/master/ScreenShot/3.gif" width="350" alt="Screenshot"/>
</p>        

## Usage
###  Add dependency

```groovy
dependencies {
   compile 'com.github.heynchy:SwipeMenuAndPullToRefresh:0.1.6'
}
```
## ListView全部功能的实现
### Step1. 添加xml
```xml
     <com.chy.srlibrary.PTRLayoutView
        android:id="@+id/refresh_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <include
            layout="@layout/refresh_head"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>
        <com.chy.srlibrary.slistview.SMRListView
            android:id="@+id/lv_swipe_menu"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>
        <include
            layout="@layout/refresh_load"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>/>
    </com.chy.srlibrary.PTRLayoutView>
``` 
### Step2. 添加初始化侧滑删除（如果不设置则不会有侧滑删除，仅有刷新)
```java
 final SwipeMenuCreatorInterfaceUtil creator = new SwipeMenuCreatorInterfaceUtil() {
            @Override
            public void create(SwipeMenu menu) {

                SwipeMenuItem deleteItem = new SwipeMenuItem(mContext);
                deleteItem.setBackground(new ColorDrawable(Color.rgb(0xF9,
                        0x3F, 0x25)));
                deleteItem.setWidth(dp2px(mContext, 100));
                deleteItem.setTitle("删除");
                deleteItem.setTitleColor(Color.rgb(255, 255, 255));
                deleteItem.setTitleSize(15);
                menu.addMenuItem(deleteItem);
            }
        };

        // 设置侧滑删除(如果不设置则不会有侧滑删除，仅有刷新)
         mSMRExpandView.setMenuCreator(creator);
        // 点击侧滑按钮的响应事件（如果涉及自定义的侧滑布局，可参考SwipMenuListView的实现方法） 
         // 侧滑的监听事件
        mSMRExpandView.setOnMenuItemClickListener(new SMExpandableView.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(int i, SwipeMenu swipeMenu, int i1) {
                mDataList.remove(i);
                mAdapter.notifyDataSetChanged();
                Toast.makeText(getApplicationContext(), "删除成功！", Toast.LENGTH_SHORT).show();
                return false;
            }
        });
        });
```
### Step3. 监听下拉刷新和上拉加载的事件
 ```java
  mPTRLayoutView.setOnRefreshListener(new PTRLayoutView.OnRefreshListener() {
            @Override
            public void onRefresh(PTRLayoutView ptrLayoutView) {
                // 处理下拉刷新
                mPTRLayoutView.refreshFinish(PTRLayoutView.SUCCEED); // 此句话必须有（以结束加载）
                Toast.makeText(getApplicationContext(), "刷新成功！", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onLoadMore(PTRLayoutView ptrLayoutView) {
                // 处理上拉加载
                mPTRLayoutView.loadmoreFinish(PTRLayoutView.SUCCEED); // 此句话必须有（以结束加载）
                Toast.makeText(getApplicationContext(), "加载成功！", Toast.LENGTH_SHORT).show();
            }
        });
 ```
 ### 另： 如果单单需要侧滑删除则加入SMListView的xml后，代码中初始化同listview的step2
 
 ```xml
  <com.chy.srlibrary.slistview.SMListView
        android:id="@+id/lv_swipe_menu"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
 ```
## ExpandableListView全部功能的实现
### Step1. 添加xml
```xml
    <com.chy.srlibrary.PTRLayoutView
         android:id="@+id/refresh_view"
         android:layout_width="match_parent"
         android:layout_height="match_parent">

          <include
               layout="@layout/refresh_head"
               android:layout_width="match_parent"
               android:layout_height="match_parent"/>

          <com.chy.srlibrary.expandview.SMRExpandView
               android:id="@+id/elv_swipe_menu"
               android:layout_width="match_parent"
               android:layout_height="match_parent"/>

          <include
               layout="@layout/refresh_load"
               android:layout_width="match_parent"
               android:layout_height="match_parent"/>/>
   
      </com.chy.srlibrary.PTRLayoutView>
    ```
 ### Step2. 添加初始化侧滑删除（如果不设置则不会有侧滑删除，仅有刷新)
```java
      // 设置侧滑的选项
        SwipeMenuCreatorInterfaceUtil creator = new SwipeMenuCreatorInterfaceUtil() {
            @Override
            public void create(SwipeMenu menu) {

                SwipeMenuItem deleteItem = new SwipeMenuItem(getApplicationContext());
                deleteItem.setBackground(new ColorDrawable(Color.rgb(0xF9,
                        0x3F, 0x25)));
                deleteItem.setWidth(dp2px(60));
                deleteItem.setTitle("删除");
                deleteItem.setTitleColor(Color.rgb(255, 255, 255));
                deleteItem.setTitleSize(15);
                menu.addMenuItem(deleteItem);
            }
        };
        mSMRExpandView.setMenuCreator(creator);

        // 侧滑的监听事件
        mSMRExpandView.setOnMenuItemClickListener(new SMExpandableView.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(int i, SwipeMenu swipeMenu, int i1) {
                mDataList.remove(i);
                mAdapter.notifyDataSetChanged();
                Toast.makeText(getApplicationContext(), "删除成功！", Toast.LENGTH_SHORT).show();
                return false;
            }
        });
```
### Step3. 监听下拉刷新和上拉加载的事件
 ```java
        mPTRLayoutView.setOnRefreshListener(new PTRLayoutView.OnRefreshListener() {
            @Override
            public void onRefresh(PTRLayoutView ptrLayoutView) {
                // 处理下拉刷新
                mAdapter.notifyDataSetChanged();
                mPTRLayoutView.refreshFinish(PTRLayoutView.SUCCEED); // 此句话必须有（以结束加载）
                Toast.makeText(getApplicationContext(), "刷新成功！", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onLoadMore(PTRLayoutView ptrLayoutView) {
                // 处理上拉加载
                mAdapter.notifyDataSetChanged();
                mPTRLayoutView.loadmoreFinish(PTRLayoutView.SUCCEED); // 此句话必须有（以结束加载）
                Toast.makeText(getApplicationContext(), "加载成功！", Toast.LENGTH_SHORT).show();
            }
        });
 ```
 ### 另： 如果单单需要侧滑删除则加入SMRExpandView的xml后，代码中初始化同Expandablelistview的step2
 
 ```xml
   <com.chy.srlibrary.expandview.SMRExpandView
        android:id="@+id/elv_swipe_menu"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
 ```
 
 ## 感谢以下两个项目的开发者：
 1. https://github.com/jingchenUSTC/PullToRefreshAndLoad
 2. https://github.com/baoyongzhang/SwipeMenuListView


License
------
    Copyright 2017 heynchy
    
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
    
        http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
