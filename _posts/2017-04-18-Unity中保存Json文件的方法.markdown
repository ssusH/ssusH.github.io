---
layout: post
title: "Unity中保存Json文件的方法"
excerpt: "朋友，你是否有时候需要保存一些游戏数据。比如你的存档之类的数据。  
或许你会需要用到Json"
categories: [Unity]
comments: true
image:
  feature: http://i4.buimg.com/567571/54648a0b67f73cc6.png
---
朋友，你是否有时候需要保存一些游戏数据。比如你的存档之类的数据。  
或许你会需要用到[Json文件](http://baike.baidu.com/link?url=69w-uP3n9dNdnb-h83KxWHGsP1VRnCMPiWPD2nIrUZM4XcbQYGfXE9vnXeVkqpC0rStVJn_zahGqJ4KpOLM49_)
---
JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式。它基于 ECMAScript 规范的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。  

那么在Unity中如何将需要的数据保存成Json文件呢。 下面写一种由一平同志传授的秘技。  

## 基本思路如下：

1. 写一个类用于保存数据。
2. 用内置的方法将该类转化为Json文件中的字符串
3. 最后用文件写入的方法保存成Json文件。

那么，我在这个虚拟实训项目当中用到这个方法存储自由组装时候零件组装的数据。方便将来读取使用。  

    [System.Serializable]
    public class DATA
    {
        public string name;
        public List<record> records = new List<record>();
    }
首先是保存数据的类，里面包含了本次组装的成品的名字，以及一个列表，用于存储每一个零件的具体数据。  

`record`类内容如下：

    [System.Serializable]
    public class record{
        public string name;
        public string type;
        public Vector3 position;
        public Vector3 rotation;
        public Vector3 scale;
        public Color color;
        public string info;
    }

然后是转换Json字符串的方法`JsonUtility.ToJson()`。

    // 摘要:
    //     ///
    //     Generate a JSON representation of the public fields of an object.
    //     ///
    //
    // 参数:
    //   obj:
    //     The object to convert to JSON form.
    //
    //   prettyPrint:
    //     If true, format the output for readability. If false, format the output for minimum
    //     size. Default is false.
    //
    // 返回结果:
    //     ///
    //     The object's data in JSON format.
    //     ///
    [ThreadAndSerializationSafe]
    public static string ToJson(object obj, bool prettyPrint);

最后是将字符串存为文件的方法`File.WriteAllText()`

    //
    // 摘要:
    //     创建一个新文件，在其中写入指定的字符串，然后关闭文件。如果目标文件已存在，则覆盖该文件。
    //
    // 参数:
    //   path:
    //     要写入的文件。
    //
    //   contents:
    //     要写入文件的字符串。
    //
    // 异常:
    //   T:System.ArgumentException:
    //     path 是一个零长度字符串，仅包含空白或者包含一个或多个由 System.IO.Path.InvalidPathChars 定义的无效字符。
    //
    //   T:System.ArgumentNullException:
    //     path 为 null。
    //
    //   T:System.IO.PathTooLongException:
    //     指定的路径、文件名或者两者都超出了系统定义的最大长度。例如，在基于 Windows 的平台上，路径必须小于 248 个字符，文件名必须小于 260 个字符。
    //
    //   T:System.IO.DirectoryNotFoundException:
    //     指定的路径无效（例如，它位于未映射的驱动器上）。
    //
    //   T:System.IO.IOException:
    //     打开文件时发生 I/O 错误。
    //
    //   T:System.UnauthorizedAccessException:
    //     path 指定了一个只读文件。- 或 -在当前平台上不支持此操作。- 或 -path 指定了一个目录。- 或 -调用方没有所要求的权限。
    //
    //   T:System.IO.FileNotFoundException:
    //     未找到 path 中指定的文件。
    //
    //   T:System.NotSupportedException:
    //     path 的格式无效。
    //
    //   T:System.Security.SecurityException:
    //     调用方没有所要求的权限。
    public static void WriteAllText(string path, string contents);
具体使用如下
>File.WriteAllText(Application.streamingAssetsPath + "\\Data.json",JsonUtility.ToJson(data,true));

第一个参数是存储地址，第二个是字符串。

经过这些操作之后，可以得到一个Json文件如下：

    {
    "name": "parts",
    "records": [
        {
            "name": "PrefabAnsaugroh0 0",
            "type": "PrefabAnsaugroh0",
            "position": {
                "x": 0.19307847321033479,
                "y": 1.2476834058761597,
                "z": 0.8727283477783203
            },
            "rotation": {
                "x": 0.0,
                "y": 0.0,
                "z": 0.0
            },
            "scale": {
                "x": 1.0,
                "y": 1.0,
                "z": 1.0
            },
            "color": {
                "r": 1.0,
                "g": 0.0,
                "b": 0.0,
                "a": 1.0
            },
            "info": ""
        },
        {
            "name": "PrefabAnsaugroh2 0",
            "type": "PrefabAnsaugroh2",
            "position": {
                "x": 0.7253917455673218,
                "y": 0.9404572248458862,
                "z": 0.7312040328979492
            },
            "rotation": {
                "x": 0.0,
                "y": 0.0,
                "z": 0.0
            },
            "scale": {
                "x": 1.0,
                "y": 1.0,
                "z": 1.0
            },
            "color": {
                "r": 1.0,
                "g": 0.0,
                "b": 0.0,
                "a": 1.0
            },
            "info": ""
        },
        {
            "name": "Prefabar-6003 0",
            "type": "Prefabar-6003",
            "position": {
                "x": 1.3006634712219239,
                "y": 0.8795359134674072,
                "z": 0.6112642288208008
            },
            "rotation": {
                "x": 0.0,
                "y": 0.0,
                "z": 0.0
            },
            "scale": {
                "x": 1.0,
                "y": 1.0,
                "z": 1.0
            },
            "color": {
                "r": 0.7529411911964417,
                "g": 0.7529411911964417,
                "b": 0.7529411911964417,
                "a": 1.0
            },
            "info": ""
        },
        {
            "name": "Prefabar-4202 0",
            "type": "Prefabar-4202",
            "position": {
                "x": -0.2058638632297516,
                "y": 0.904215931892395,
                "z": 0.7581205368041992
            },
            "rotation": {
                "x": 0.0,
                "y": 0.0,
                "z": 0.0
            },
            "scale": {
                "x": 1.0,
                "y": 1.0,
                "z": 1.0
            },
            "color": {
                "r": 0.7529411911964417,
                "g": 0.7529411911964417,
                "b": 0.7529411911964417,
                "a": 1.0
            },
            "info": ""
        }
    ]
    }
