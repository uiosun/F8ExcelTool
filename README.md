# F8ExcelTool

[![license](http://img.shields.io/badge/license-MIT-green.svg)](https://opensource.org/licenses/MIT) 
[![Unity Version](https://img.shields.io/badge/unity-2021.3.15f1-blue)](https://unity.com) 
[![Platform](https://img.shields.io/badge/platform-Win%20%7C%20Android%20%7C%20iOS%20%7C%20Mac%20%7C%20Linux-orange)]() 

## 简介（希望自己点击F8，就能开始制作游戏，不想多余的事）
Unity读取Excel工具，点击F8自动生成C#索引字段，能读取Excel导出的二进制文件，运行时读取Excel文件，支持Win/Android/iOS/Mac/Linux  

## 导入插件（需要首先导入核心）
注意！此包依赖->F8Framework核心：https://github.com/TippingGame/F8Framework.git  
方式一：直接下载文件，放入Unity  
方式二：Unity->点击菜单栏->Window->Package Manager->点击+号->Add Package from git URL->输入：https://github.com/TippingGame/F8ExcelTool.git  
URL->输入：https://github.com/TippingGame/F8Framework.git  

## 使用流程
1、在目录StreamingAssets/config下创建Excel  
2、点击菜单栏->开发工具->导入配置表_F8（快捷键）  
3、成功后，根目录下会生成"F8ExcelTool"目录  
4、游戏运行时读取Excel数据，点击菜单栏->开发工具->运行时读取Excel_F7（快捷键）  

### 读取二进制文件
```C#
        F8DataManager.Instance.LoadAll();//加载所有配置
```
### 运行时读取Excel文件
```C#
        ReadExcel.Instance.LoadAllExcelData();//运行时加载所有配置
```
### 打印数据
```C#
        Debug.Log(F8DataManager.Instance.GetTableByID(115).name);//注意，Table替换为你的配置表名，name为表的一个属性
        foreach (var VARIABLE in F8DataManager.Instance.GetTableByID(113).llliststr)
        {
            foreach (var VARIABLE2 in VARIABLE)
            {
                Debug.Log(VARIABLE2);
            }
        }
```

## Excel格式
基础类型
|int| long | float | double | str |
| - | - | - | - | - |
| id | name1 | name2 | name3 | name4 |
|1|9935434343|2.725412|1.346655321|读取Excel工具|
|2|9935434343|2.725412|1.346655321|读取Excel工具|

拓展类型
| int[] | float[] | str[] | obj[] | int[][] | float[][] | str[][] |
| - | - | - | - | - | - | - |
| name1 | name2 | name3 | name4 | name5 | name6 | name7 |
|[1,5]|[1.5,5.8]|[文件,支持]|["生成",656,1.235999]|{[1,6],[2,8]}|{[6.215,6.12],[2.5,14.556]}|{[自动,格式],[tipping,excel]}|
|[1,5]|[1.5,5.8]|[文件,支持]|["生成",656,1.235999]|{[1,6],[2,8]}|{[6.215,6.12],[2.5,14.556]}|{[自动,格式],[tipping,excel]}|

## 目录结构
---F8ExcelTool  
----Editor（编辑器代码）  
----Plugins（库文件）  
----Runtime（运行时代码）  
----Tests（测试场景Demo）  

## 使用到的库
Excel.dll  
I18N.CJK.dll  
I18N.dll  
I18N.MidEast.dll  
I18N.Other.dll  
I18N.Rare.dll  
I18N.West.dll  
ICSharpCode.SharpZipLib.dll  

## 注意  
#### 由于Android资源都在包内，在Android上使用，需要先复制到可读写文件夹中再进行读取
```C#
    IEnumerator Start()
    {
        //由于安卓资源都在包内，需要先复制到可读写文件夹1
        string assetPath = URLSetting.STREAMINGASSETS_URL + "config";
        string[] paths = null;
        WWW www = new WWW(assetPath + "/fileindex.txt");
        yield return www;
        if (www.error != null)
        {
            Debug.Log(www.error);
            yield return null;
        }
        else
        {
            string ss = www.text;
            paths = ss.Split("\n", StringSplitOptions.RemoveEmptyEntries);
        }
    
        for (int i = 0; i < paths.Length; i++)
        {
            yield return CopyAssets(paths[i].Replace("\r", ""));
        }
        //读取Excel文件
        ReadExcel.Instance.LoadAllExcelData();
        text.text += F8DataManager.Instance.GetfasdffByID(115).name;
        Debug.Log(F8DataManager.Instance.GetfasdffByID(115).name);
        foreach (var VARIABLE in F8DataManager.Instance.GetfasdffByID(113).llliststr)
        {
            foreach (var VARIABLE2 in VARIABLE)
            {
                text.text += VARIABLE2;
                Debug.Log(VARIABLE2);
            }
        }
    }
    //由于安卓资源都在包内，需要先复制到可读写文件夹2
    IEnumerator CopyAssets(string paths)
    {
        string assetPath = URLSetting.STREAMINGASSETS_URL + "config";
        string sdCardPath = Application.persistentDataPath + "/config";
        WWW www = new WWW(assetPath + "/" + paths);
        yield return www;
        if(www.error != null)
        {
            Debug.Log(www.error);
            yield return null;
        }
        else
        {
            FileTools.SafeWriteAllBytes(sdCardPath + "/" + paths, www.bytes);
        }
    }
```
