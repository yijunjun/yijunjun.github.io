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