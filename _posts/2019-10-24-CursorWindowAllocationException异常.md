### 异常信息如下
```
io.reactivex.exceptions.UndeliverableException: android.database.CursorWindowAllocationException: Cursor window allocation of 2048 kb failed. # Open Cursors=1 (# cursors opened by this proc=1)
	at io.reactivex.plugins.RxJavaPlugins.onError(RxJavaPlugins.java:367)
	at io.reactivex.internal.schedulers.ScheduledRunnable.run(ScheduledRunnable.java:69)
	at io.reactivex.internal.schedulers.ScheduledRunnable.call(ScheduledRunnable.java:57)
	at java.util.concurrent.FutureTask.run(FutureTask.java:237)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$201(ScheduledThreadPoolExecutor.java:152)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:265)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1112)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:587)
	at java.lang.Thread.run(Thread.java:818)
Caused by: android.database.CursorWindowAllocationException: Cursor window allocation of 2048 kb failed. # Open Cursors=1 (# cursors opened by this proc=1)
	at android.database.CursorWindow.<init>(CursorWindow.java:108)
	at android.database.AbstractWindowedCursor.clearOrCreateWindow(AbstractWindowedCursor.java:198)
	at android.database.sqlite.SQLiteCursor.fillWindow(SQLiteCursor.java:139)
	at android.database.sqlite.SQLiteCursor.getCount(SQLiteCursor.java:133)
	at org.greenrobot.greendao.AbstractDao.loadAllFromCursor(AbstractDao.java:453)
	at org.greenrobot.greendao.AbstractDao.loadAllAndCloseCursor(AbstractDao.java:203)
	at org.greenrobot.greendao.InternalQueryDaoAccess.loadAllAndCloseCursor(InternalQueryDaoAccess.java:37)
	at org.greenrobot.greendao.query.Query.list(Query.java:89)
	at org.greenrobot.greendao.query.QueryBuilder.list(QueryBuilder.java:427)
	at com.dinghao.uboard.newversion.module.function.OfflineCardFragment.submitCardInfo(OfflineCardFragment.java:119)
	at com.dinghao.uboard.newversion.module.function.OfflineCardFragment.access$200(OfflineCardFragment.java:55)
	at com.dinghao.uboard.newversion.module.function.OfflineCardFragment$2.onNext(OfflineCardFragment.java:186)
	at com.dinghao.uboard.newversion.module.function.OfflineCardFragment$2.onNext(OfflineCardFragment.java:178)
	at io.reactivex.internal.operators.observable.ObservableObserveOn$ObserveOnObserver.drainNormal(ObservableObserveOn.java:200)
	at io.reactivex.internal.operators.observable.ObservableObserveOn$ObserveOnObserver.run(ObservableObserveOn.java:252)
	at io.reactivex.internal.schedulers.ScheduledRunnable.run(ScheduledRunnable.java:66)
```
### 原因
使用rxjava定时器定时处理数据库，线程模式是Schedulers.io()，因为这个模式没有线程限制，导致线程太多，引起此异常
### 解决
改用固定线程数，如Schedulers.single()，单个线程
