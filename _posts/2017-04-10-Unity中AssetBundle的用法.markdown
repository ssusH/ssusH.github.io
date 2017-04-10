---
layout: post
title: "Unity中AssetBundle的用法"
excerpt: "当你的包里面有多个物体的时候，怎么加载他们呢。"
categories: [Unity]
comments: true
---
## 加载AssetBundle

当你的包里面有多个物体的时候，怎么加载他们呢。
关键方法是`LoadAllAssets()`  
而且，还有一点，就是你加载出来的东西可能包括很多奇怪的东西。 比如你加载一个模型，那么就还会有他的材质球等等你不需要用到的物体。这时候你需要判断一下你读到的物体是不是你需要的。就需要用到`GetType()`这个方法或者`LoadAsset()` 这两个方法，一个是判断资源的类型，一个是更具名字加载包里面的资源。
以下贴一个我的源代码。


    IEnumerator loadingBundle()
    {
        url = "file://" + Application.streamingAssetsPath + "\\model.assetbundle";
        WWW www = new WWW(url);
        //定义www为WWW类型并且等于所下载下来的WWW中内容。
        yield return www;
        Debug.Log("www加载完成");
        //取得AssetBundle
        AssetBundle ab = www.assetBundle;
        //异步加载对象
        Object[] objs = ab.LoadAllAssets();  //指定了类型为GameObject
        //等待异步完成
        yield return objs;
        Debug.Log("异步加载完成");
        //获得加载对象的引用,这个时候是在内存中
        for(int i = 0;i<objs.Length;i++)
        {
            if (objs[i].GetType().ToString() == "UnityEngine.GameObject")
            {
                prefab.Add(objs[i] as GameObject);
            }

        }
        //克隆一份对象显示在场景中
        //从内存卸载AssetBundle
        ab.Unload(false);
        creatPart();
    }


卸载AssetBundle
AssetBundle.unload(bool unloadAllLoadedObjects) 接口用来卸载AssetBundle文件。参数为false时，调用该接口后，只会卸载AssetBundle对象自身，并不会影响AssetBundl中加载的Assets。
参数为true时，除了AssetBundle对象自身，所有从当前AssetBundle中加载的Assets也会被同时卸载，不管这个Assets是否还在使用中。
官方推荐参数一般设置为false，只有当很明确知道从AssetsBundle中加载的Assets不会被任何对象引用时，才将参数设置成true。
