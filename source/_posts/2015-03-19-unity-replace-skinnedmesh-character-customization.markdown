---
layout: post
title: "Unity: 自定义角色中的蒙皮网格替换"
date: 2015-03-19 08:47:55 +0000
comments: true
categories: Unity
---

Unity允许运行时替换SkinnedMeshRenderer的网格来实现自定义角色部件，分两种情况`Optimize Game Objects` 于 `Nonoptimized GameObjects`

`Optimize Game Objects`是模型Rig选项，开启后骨架不会在场景中存在，而是放入了内部存储。这种情况下只需简单替换mesh就行，不需要重新设置骨骼信息

```csharp
    public static void ReplaceSkinnedMeshOptimized(SkinnedMeshRenderer src, SkinnedMeshRenderer dst)
    {
        dst.sharedMaterials = src.sharedMaterials;
        dst.sharedMesh = src.sharedMesh;
        dst.localBounds = src.localBounds;
    }
```

对于没有优化的Rig 需要重新设置骨骼信息


```csharp
    public static void ReplaceSkinnedMeshNonOptimized(SkinnedMeshRenderer src, SkinnedMeshRenderer dst)
    {
        Dictionary<string, Transform> skeleton = new Dictionary<string, Transform>();
        Transform[] allTrans =  dst.transform.GetComponentsInChildren<Transform>(true);
        foreach (var item in allTrans)
        {
            skeleton[item.name] = item;
        }

        dst.sharedMaterials = src.sharedMaterials;
        dst.sharedMesh = src.sharedMesh;
        Transform[] bones = new Transform[src.bones.Length];
        for (int ii = 0; ii < bones.Length; ii++)
        {
            bones[ii] = skeleton[src.bones[ii].name];
        }

        dst.bones = bones;
        dst.rootBone = skeleton[src.rootBone.name];
        dst.localBounds = src.localBounds;
    }
```