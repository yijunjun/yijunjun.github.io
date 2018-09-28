# unity一些知识

## 清除启动界面工程

```bash
cd /Users/<yourUserName>/Library/Preferences/

cat com.unity3d.UnityEditor5.x.plist

defaults read com.unity3d.UnityEditor5.x.plist
defaults delete com.unity3d.UnityEditor5.x "RecentlyUsedProjectPaths-0"
```

## 打印调用堆栈

```c#
string trackStr = new System.Diagnostics.StackTrace().ToString();
Debug.Log ("Stack Info:" + trackStr);
```

## 入门积累

1.  Unity是单线程设计的游戏引擎,子线程中无法运行Unity SDK
2.  Unity主循环是单线程,游戏脚本MonoBehavior有着严格的生命周期
3.  倾向使用time slicing（时间分片）的协程（coroutine）去完成异步任务