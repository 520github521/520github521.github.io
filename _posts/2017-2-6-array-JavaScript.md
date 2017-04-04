---
category: JavaScript
path: '/JavaScript'
title: 'JavaScript数组'
type: 'array'

layout: nil
---

### 1.数组的创建：

```var arrayObj = new Array();　//创建一个数组 ```  
```var arrayObj = new Array([size]);　//创建一个数组并指定长度，注意不是上限，是长度 ```  
```var arrayObj = new Array([element0[, element1[, ...[, elementN]]]]);　//创建一个数组并赋值```
要说明的是，虽然第二种方法创建数组指定了长度，但实际上所有情况下数组都是变长的，也就是说即使指定了长度为5，仍然可以将元素存储在规定长度以外的，注意：这时长度会随之改变。  

### 2、数组的元素的访问
```var testGetArrValue=arrayObj[1]; //获取数组的元素值 ```
```object.attribute;//获取数组的元素值```
```arrayObj[1]= "这是新值"; //给数组元素赋予新的值 ```

### 3、数组元素的添加  
``` arrayObj. push([item1 [item2 [. . . [itemN ]]]]);// 将一个或多个新元素添加到数组结尾，并返回数组新长度。  ```
  
```arrayObj.unshift([item1 [item2 [. . . [itemN ]]]]);// 将一个或多个新元素添加到数组开始，数组中的元素自动后移，返回数组新长度。  ```
  
```arrayObj.splice(insertPos,0,[item1[, item2[, . . . [,itemN]]]]);//将一个或多个新元素插入到数组的指定位置，插入位置的元素自动后移，返回""。  ```

```for(var i=0;i<cab_id.length;i++){                
               info['cabpos']=cabpos.split(',')[i];
               info['cabid']=cabid.split(',')[i];
               info['gunid']=gunid.split(',')[i];
               info['guntypeid']=guntypeid.split(',')[i];
               data.push(info);
	       data.push({'cabpos':cabpos.split(',')[i],'cabid':cabid.split(',')[i],'gunid':gunid.split(',')[i],'guntypeid':guntypeid.split(',')[i]})//转换成json数组
   }```
