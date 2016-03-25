title: Unity5.3新版场景管理
date: 2016-03-25 04:57:17
tags: Unity
categories: Unity
---

本文介绍如何使用Unity5.3引入的新API `UnityEngine.SceneManagement`来进行场景和关卡的管理。

## 介绍
经过5个版本的发展, Unity5.3放出了统一合理的场景管理API。 在之前的版本中场景即代码中*关卡*(`Level`)。 但是使用Unity过程中很快便发现用*场景*来定义不同的*关卡*不是必须的，所以就引入了 `Scene` API.
之前版本中的*关卡*在逻辑角度上指明了此关卡的构建索引号(即*Build Settins*中此关卡的序号), *场景*则指明他们的包含该场景的资源名。 然而这两个概念都不是识别一个*真正场景*的可靠唯一ID。*Build Settings*中移动场景会改变他们的索引。同样的，不同文件夹下可能会有两个相同名字的场景。Unity5.3尝试结束这些混乱的逻辑。

## 5.3版本中新的场景API
Unity5.3之前使用*构建索引*和*场景资源名*都能指向一个场景。Unity5.3引入了一个新的`Scene`结构体来指明一个场景。它封装了一些场景常用方法和变量如`buildIndex`, `name`和`path`。

## 访问`SceneManager`类
要想使用新的场景API，C#代码需要引用`SceneManagement`命名空间
```csharp
using UnityEngine.SceneManagement;
```
这样就可以使用`SceneManager`类来替换`Application`中废弃的相关场景函数。

## 获取当前场景
Unity改用“场景”而不是“关卡”的一个主要原因是你可以同时加载多个场景。这样就破坏了“当前关卡”这个概念，它现在被替换成了“激活场景”。多数情况下，激活场景为最新加载的场景。我们可以任意时刻使用`GetActiveScene`获取激活场景。
```csharp
// 5.3
Scene scene = SceneManager.GetActiveScene();
Debug.Log(scene.name);
Debug.Log(scene.buildIndex);
 
// 5.2
Debug.Log(Application.loadedLevelName);
Debug.Log(Application.loadedLevel);
```
检测激活场景是不是某个指定场景名：
```csharp
if (SceneManager.GetActiveScene().name == "sceneName")
{
    // ...
}
```
也可以使用重载的`==`操作符来检测：
```csharp
if (SceneManager.GetActiveScene() == scene)
{
    // ...
}
```

## 加载场景

和之前的版本一样，我们可以通过构建索引和场景名来加载场景，两个场景名一样但路径不同的话推荐使用路径来加载指定场景，否则Unity会加载第一个匹配名字的场景。

### 加载单个场景

默认是加载单个场景，加载新场景会卸载所有已加载的旧场景，销毁所有旧场景的`GameObjects`。
```csharp
// 5.3
SceneManager.LoadScene(4);
SceneManager.LoadScene("sceneName"); //场景名方式，忽略大小写
SceneManager.LoadScene("path/to/scene/file/sceneName");//场景资源路径方式，忽略大小写

// 5.2
Application.LoadLevel(4);
Application.LoadLevel("sceneName");
```
异步版本：
```csharp
// 5.3
SceneManager.LoadSceneAsync(4);
SceneManager.LoadSceneAsync("sceneName");
SceneManager.LoadSceneAsync("path/to/scene/file/sceneName");

// 5.2
Application.LoadLevelAsync(4);
Application.LoadLevelAsync("sceneName");
```

### 添加方式加载场景

我们也可以通过指定`SceneManagement.LoadSceneMode.Additive`以添加新场景的方式来加载新场景：
```csharp
// 5.3
SceneManager.LoadScene(4, SceneManagement.LoadSceneMode.Additive);
SceneManager.LoadScene("sceneName", SceneManagement.LoadSceneMode.Additive);
SceneManager.LoadScene("path/to/scene/file/sceneName", SceneManagement.LoadSceneMode.Additive);

// 5.2
Application.LoadLevelAdditive(4);
Application.LoadLevelAdditive("sceneName");
```
异步版本：
```csharp
// 5.3
SceneManager.LoadSceneAsync(4, SceneManagement.LoadSceneMode.Additive);
SceneManager.LoadSceneAsync("sceneName", SceneManagement.LoadSceneMode.Additive);
SceneManager.LoadSceneAsync("path/to/scene/file/sceneName", SceneManagement.LoadSceneMode.Additive);

// 5.2
Application.LoadLevelAdditiveAsync(4);
Application.LoadLevelAdditiveAsync("sceneName");
```

## 卸载场景

Unity5.3同时也提供`UnloadScene`方法来卸载指定场景:
```csharp
// 5.3
SceneManager.UnloadScene(4); // 通过构建索引卸载
SceneManager.UnloadScene("sceneName"); // 通过场景名或场景路径卸载
SceneManager.UnloadScene(sceneStruct); // 通过Scene结构体卸载（加载场景却没有提供通过Scene结构体加载的方式)
```
该函数返回是否成功卸载场景。

## 总结

上面简单介绍了5.3版本中的场景管理API, 我们看到Unity中新的场景管理方式提供了一种更清晰可靠的动态加载管理的流程。如果你在用5.3版本还是尽快更新代码使用新API来管理场景吧。

参考 [Scene Management in Unity 5][1]


  [1]: http://www.alanzucconi.com/2016/03/23/scene-management-unity-5/