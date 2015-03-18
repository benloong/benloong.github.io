---
layout: post
title: "Unity Load Level From AssetBundle"
date: 2015-03-18 05:55:37 +0000
comments: true
categories: Unity
---

`由于Unity5有一套新的AssetBundle机制，所以这里讲的仅适用于Unity4.x`

## 打包场景

```csharp
    [MenuItem("Tools/BuildStreamingScene")]
    public static void BuildStreamingScene()
    {
        var path = SelectSaveFolderPath();
        if (string.IsNullOrEmpty(path))
        {
            return;
        }
        string[] levels = new string[] { "Assets/Game/Scenes/Level1.unity" };
        BuildPipeline.BuildStreamedSceneAssetBundle(levels, path + "Scenes.ab", BuildTarget.Android, BuildOptions.BuildAdditionalStreamedScenes);
    }
```

## 加载场景

由于Application.LoadLevel实际上会延迟加载场景对象，如果提前调用ab.Unload(false)会造成场景对象被清空，所以ab.Unload(false)应在场景加载完成后调用 在OnLevelWasLoaded事件中Unload.

```csharp
    AssetBundle ab { set; get; }
    IEnumerator Start()
    {
        yield return new WaitForSeconds(1.0f);
     
        WWW www = new WWW(new System.Uri(Application.streamingAssetsPath + "/scenes/Scenes.ab").AbsoluteUri);
        yield return www;
        ab = www.assetBundle;

        if (Application.CanStreamedLevelBeLoaded("Level1"))
        {
            Application.LoadLevel("Level1");
        }
        //ab.Unload(false); // 此处会施放AssetBundle会造成空场景
        yield return null;
    }

    void OnLevelWasLoaded(int levelId)
    {
        if (ab != null)
        {
            ab.Unload(false);
        }
        Debug.Log("On Level Was Loaded .. " + Application.loadedLevelName);
    }
```

`完`