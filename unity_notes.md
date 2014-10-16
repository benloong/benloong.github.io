Unity Notes
================

###AssetBundle 依赖

`->`依赖于

如果`A -> B`

则先加载B,后加载A,

在A使用前不能Unload B, 否则依赖解析出错

```c#
IEnumerator Start() {
    WWW www = new WWW(new System.Uri(Application.dataPath+"/assetbundle/basematerial.u3ys").AbsoluteUri);
    yield return www;
    var dependency = www.assetBundle;
    www = new WWW(new System.Uri(Application.dataPath+"/assetbundle/fc0032.u3ys").AbsoluteUri);
    yield return www;
    dependency.LoadAll();
    Instantiate(www.assetBundle.mainAsset);

    ((AssetBundle)dependency).Unload(false);
}
```


###APK 打包出错

`invocation failed`

`not a file`

错误原因是设置keystore出错，没有找到正确的keypass

http://answers.unity3d.com/questions/621759/building-error-not-a-file-.html