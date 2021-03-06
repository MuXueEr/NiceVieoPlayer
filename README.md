# NiceVieoPlayer
### Features

 * 用IjkPlayer/MediaPlayer + TextureView封装，可切换IjkPlayer、MediaPlayer.
 * 支持本地和网络视频播放.
 * 完美切换小窗口、全屏，可在RecyclerView、ListView中无缝全屏.
 * 手势滑动调节播放进度、亮度、声音.
 * 可自定义控制界面

### Usage
下载niceviewoplayer库，在AndroidSutio中作为Mudule添加依赖。

或者在Gradle中添加依赖：

```
allprojects {
    repositories {
    ...
    maven { url 'https://jitpack.io' }
    }
}

dependencies {
    compile 'com.github.xiaoyanger0825:NiceVieoPlayer:v1.6'
}
```

**在Activity中使用时，该Activity需要继承自AppCompatActivity。在Fragment中使用时，外层的Activity也同样需要继承自AppCompatActivity**

1.在Activity中使用：
```
 private void init() {
      mNiceVideoPlayer = (NiceVideoPlayer) findViewById(R.id.nice_video_player);
      mNiceVideoPlayer.setPlayerType(NiceVideoPlayer.PLAYER_TYPE_IJK); // or NiceVideoPlayer.PLAYER_NATIVE
      mNiceVideoPlayer.setUp(mVideoUrl, null);
      // 默认的控制器，如需自定义，可继承NiceVideoPlayer自己实现
      TxVideoPlayerController controller = new TxVideoPlayerController(this);
      controller.setTitle(mTitle);
      controller.setImage(mImageUrl);
      mNiceVideoPlayer.setController(controller);
  }
  
  // 按下Home键暂停播放，回到界面继续播放。
  @Override
  protected void onStop() {
      NiceVideoPlayerManager.instance().pauseNiceVideoPlayer();
      super.onStop();
  }
  
  @Override
  protected void onRestart() {
      NiceVideoPlayerManager.instance().restartNiceVideoPlayer();
      super.onRestart();
  }
  
  // 按返回键
  // 当前是全屏或小窗口，需要先退出全屏或小窗口。
  @Override
  public void onBackPressed() {
      if (NiceVideoPlayerManager.instance().onBackPressd()) {
          return;
      }
      super.onBackPressed();
  }
  
  @Override
  protected void onDestroy() {
      // 很重要，在Activity和Fragment的onStop方法中一定要调用，释放的播放器。
      NiceVideoPlayerManager.instance().releaseNiceVideoPlayer();
      super.onDestroy();
  }
  ```
2.在RecyclerView列表中使用需要监听itemView detach：
```
mRecyclerView.addOnChildAttachStateChangeListener(new RecyclerView.OnChildAttachStateChangeListener() {
    @Override
    public void onChildViewAttachedToWindow(View view) {

    }

    @Override
    public void onChildViewDetachedFromWindow(View view) {
        NiceVideoPlayer niceVideoPlayer = (NiceVideoPlayer) view.findViewById(R.id.nice_video_player);
        if (niceVideoPlayer == NiceVideoPlayerManager.instance().getCurrentNiceVideoPlayer()) {
            NiceVideoPlayerManager.instance().releaseNiceVideoPlayer();
        }
    }
});
```
3.自定义控制界面

```
public class CustomController extends NiceVideoPlayerController {
    // 实现自己的控制界面
    ...
}

... 

public class MainActivity extends AppCompatActivity {

    private NiceVideoPlayer mNiceVideoPlayer;

      @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            // 使用自定义的CustomContorller
            mNiceVideoPlayer = (NiceVideoPlayer) findViewById(R.id.nice_video_player);
            CustomContorller controller = new CostomController(this);
            ...// mNiceVideoPlayerh和controller的其他设置
            mNiceVideoPlayer.setController(controller);
        }
}
  
```
### Proguard
```
-keep class tv.danmaku.ijk.media.player.**{*;}
```
### Demo
![](https://github.com/xiaoyanger0825/NiceVieoPlayer/raw/master/images/aa.jpg)
![](https://github.com/xiaoyanger0825/NiceVieoPlayer/raw/master/images/bb.jpg)
![](https://github.com/xiaoyanger0825/NiceVieoPlayer/raw/master/images/cc.jpg)
![](https://github.com/xiaoyanger0825/NiceVieoPlayer/raw/master/images/dd.jpg)