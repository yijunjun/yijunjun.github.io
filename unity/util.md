# unity一些知识

## 清除启动界面工程

```bash
cd /Users/<yourUserName>/Library/Preferences/

cat com.unity3d.UnityEditor5.x.plist

defaults read com.unity3d.UnityEditor5.x.plist
defaults delete com.unity3d.UnityEditor5.x "RecentlyUsedProjectPaths-0"
```