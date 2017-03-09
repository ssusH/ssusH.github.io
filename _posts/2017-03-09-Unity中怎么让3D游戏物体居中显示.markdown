---
layout: post
title: "Unity中怎么让3D游戏物体居中显示"
excerpt: "不知道你们有没有遇到过这种需求，就是需要讲一个游戏物体正好居中在屏幕中央。  
并且要一个合适的尺寸，比如占屏幕中间一半或者三分之一的大小。  
但是你们的游戏物体又不是一个规则物体，他的碰撞体组件不是规则的，而是网格碰撞体。  
并且它还又可能是由很多子物体组成的。 "
categories: [Unity]
comments: true

---  

不知道你们有没有遇到过这种需求，就是需要讲一个游戏物体正好居中在屏幕中央。  
并且要一个合适的尺寸，比如占屏幕中间一半或者三分之一的大小。  
但是你们的游戏物体又不是一个规则物体，他的碰撞体组件不是规则的，而是网格碰撞体。  
并且它还又可能是由很多子物体组成的。

就像这样:  
![](http://p1.bpimg.com/567571/95b258ada8ee35f9.png)

我是这么做的。思路具体是。  
1. 先通过遍历游戏物体的子物体，来获取游戏物体包围盒的尺寸
2. 然后我们需要知道关于这个包围盒的一些数据，比如说中心点，在X,Y,Z轴上的长度各是多少。  
3. 得到这些数据之后我们声明一个RECT变量，存下他的中心点和尺寸信息。  
4. 最后通过这个Rect来调整摄像机位置。  

直接上代码：  

    public void calculateObjectsize(GameObject obj)
    {
    Bounds temp = new Bounds();
    float minX, minY, minZ;
    float maxX, maxY, maxZ;
    minX = minY = minZ = 10000f;
    maxX = maxY = maxZ = -minX;
    foreach (Transform t in obj.transform.GetComponentsInChildren<Transform>())
    {
        if(t.name != mainObjName)
            temp = t.GetComponent<Renderer>().bounds;

        if (temp.max.x > maxX)
            maxX = temp.max.x;
        if (temp.max.y > maxY)
            maxY = temp.max.y;
        if (temp.max.z > maxZ)
            maxZ = temp.max.z;

        if (temp.min.x < minX)
            minX = temp.min.x;
        if (temp.min.y < minY)
            minY = temp.min.y;
        if (temp.min.z < minZ)
            minZ = temp.min.z;
    }
    bound = new Bounds(new Vector3((maxX + minX) / 2, (maxY + minY) / 2, (maxZ + minZ) / 2), new Vector3(maxX - minX, maxY - minY, maxZ - minZ));
    Debug.Log((maxX + minX) / 2 + " " + (maxY + minY) / 2 + " " + (maxZ + minZ) / 2);
    Debug.Log((maxX - minX) + " " + (maxY - minY) + " " + (maxZ - minZ));
    }

    //这一句是按照我的情况用来调节摄像机位置的语句。
    Camera.main.transform.position = bound.center + new Vector3(0, 0, -(bound.size.x + bound.size.z + bound.size.y) / 3 * 2);

这里调节摄像机位置，是从这个Rect的中心点出发移动它包围盒XYZ尺寸和除以3，也就是一个平均大小的值的两倍。你们也可以按照自己的需求调节这个数值。  
