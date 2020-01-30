# MediaLocalProxyLib
> * 封装了一个播放器功能库
> * 实现视频边下边播的功能

本项目的架构如下：
![](./files/LocalProxy.png)
从上面的架构可以看出来，本项目的重点在本地代理层，这是实现边下边播的核心逻辑；

### 接入库须知
#### 1.在Application->onCreate(...) 中初始化
```
LocalProxyCacheManager.getInstance().initProxyCache(this, 3888);
```
#### 2.打开本地代理开关
```
PlayerAttributes attributes = new PlayerAttributes();
attributes.setUseLocalProxy(mUseLocalProxy);
```
#### 3.设置本地代理模块监听
```
    mPlayer.setOnLocalProxyCacheListener(mOnLocalProxyCacheListener);
    mPlayer.startLocalProxy(mUrl, null);



    private IPlayer.OnLocalProxyCacheListener mOnLocalProxyCacheListener = new IPlayer.OnLocalProxyCacheListener() {
        @Override
        public void onCacheReady(IPlayer mp, String proxyUrl) {
            LogUtils.w("onCacheReady proxyUrl = " + proxyUrl);
            Uri uri = Uri.parse(proxyUrl);
            try {
                mPlayer.setDataSource(PlayerActivity.this, uri);
            } catch (IOException e) {
                e.printStackTrace();
                return;
            }
            mPlayer.setSurface(mSurface);
            mPlayer.setOnPreparedListener(mPreparedListener);
            mPlayer.setOnVideoSizeChangedListener(mVideoSizeChangeListener);
            mPlayer.prepareAsync();
        }

        @Override
        public void onCacheProgressChanged(IPlayer mp, int percent, long cachedSize) {
            LogUtils.w("onCacheProgressChanged percent = " + percent);
            mPercent = percent;
        }

        @Override
        public void onCacheForbidden(IPlayer mp, String url) {
            LogUtils.w("onCacheForbidden url = " + url);
        }
    };
```
#### 4.本地代理的生命周期跟着播放器的生命周期一起


### 功能概要
#### 1.封装了一个player sdk层
> * 1.1 接入Android 原生的 MediaPlayer 播放器
> * 1.2 接入google的EXO player 播放器
> * 1.3 接入开源的 ijk player 播放器
#### 2.实现整视频的边下边播
> * 2.1 实现整视频的分片下载
> * 2.2 实现整视频的断点下载
#### 3.实现HLS分片视频的边下边播
> * 3.1 实现HLS视频源的解析工作
> * 3.2 实现HLS的边下边播
> * 3.3 实现HLS的断点下载功能
#### 4.线程池控制下载功能
#### 5.提供视频下载的额外功能
> * 5.1 可以提供播放视频或者下载视频的实时网速
> * 5.2 可以提供已缓存视频的大小

如果你觉得这个库有用,请鼓励一下吧;
![](./files/Jeffmony.jpg)
