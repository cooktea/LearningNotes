# HTML拖放

>[HTML 拖放 API](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API)

## 事件
事件名|事件处理函数|介绍
:--:|:--:|:--:
drag|ondrag|拖动元素或选中文本时触发
dragend|ondragend|拖动操作结束时触发
dragenter|ondragenter|拖动元素或选中的文本到一个可释放目标时触发
dragexit|ondragexit|元素变得不再是拖动操作的选中目标时触发
dragleave|ondragleave|拖动院所或选中的文本离开一个可释放目标是触发
dragover|ondragover|当元素或选中的文本被拖到一个可释放目标上时触发（每100ms触发一次）
dragstart|ondragstart|用户开始拖动一个元素或选中的文本时触发
drop|ondrop|元素或选中的文本在可释放目标上被释放时触发

*tip：从操作系统向浏览器中拖放文件时，不会触发dragstart和dragend事件*

## 接口
接口名|作用|详细信息
:--:|:--:|:--:
DrahEvent|DragEvent 接口有一个**构造函数和一个dataTransfer属性**，dataTransfer属性是一个DataTransfer对象。 DataTransfer 对象包含了**拖拽事件的状态，例如拖动事件的类型（如拷贝或者移动），拖动的数据（一个或者多个项）和每个拖动项的类型（MIME类型）。** DataTransfer 对象也有一些方法，**可以向拖动数据中添加项或者删除项。** DragEvent  和 DataTransfer  接口应该仅有的接口来给应用程序添加html拖放功能。|[详细信息](https://developer.mozilla.org/zh-CN/docs/Web/API/DragEvent)
DataTransfer|每一个DataTransfer对象代表一个单独的拖动项，每一项有个**kind属性，代表数据的kind（string或者file），也有个type属性，代表数据项的type（例如MIME类型）**|[详细信息](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer)
DataTransferItem|DataTransferItem 描述了一个拖拽项。在一个拖拽操作中，每一个 drag event 都有一个dataTransfer 属性，它包含一个存有拖拽数据的 list ，其中每一项都是一个 DataTransferItem。|[详细信息](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransferItem)
DataTransferItemList|DataTransferItemList对象**是DataTransferItem对象的列表**。这个列表对象包含以下方法：向列表中添加拖动项，从列表中移除拖动项和清空列表中所有的拖拽项。|[详细信息(英文版)](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransferItemList)

*tips: DataTransfer和DataTransferItem接口的一个主要的不同在于，前者使用**同步**的getData() 方法去得到一个拖拽项的数据，然后后者使用**异步**的getAsString() 方法得到一个拖拽项的数据。*

## 基础
* 确定什么是可以拖动的  
    可以被拖动的元素需要添加draggable属性，再加上全局事件处理函数ondragstart。如下所示
    ```
        function dragstart_handler(ev) {  
            console.log("dragStart");  
            // Add the target element's id to the data transfer object  
            ev.dataTransfer.setData("text/plain", ev.target.id);  
        }  
        <body>  
            <p id = "p1" draggable = "true" ondragstart = "dragstart_handler(event);">  
                This element is draggable.  
            </p>  
        </body>
    ```

* 定义拖动数据  
    应用程序可以在拖动操作中包含任意数量的数据项。每个数据项都是一个string 类型，典型的MIME类型，如：text/html。
    [详细信息](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API#%E5%AE%9A%E4%B9%89%E6%8B%96%E5%8A%A8%E6%95%B0%E6%8D%AE)

* 定义拖动图像   
    拖动过程中，浏览器会在鼠标旁显示一张默认图片。当然，应用程序也可以通过setDragImage()方法自定义一张图片，如下面的例子所示。
    [详细信息](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API#%E5%AE%9A%E4%B9%89%E6%8B%96%E5%8A%A8%E5%9B%BE%E5%83%8F)

* 定义拖动效果  
    **dropEffect**属性用来控制拖放操作中用户给予的反馈。它会影响到拖动过程中浏览器显示的鼠标样式。比如，当用户悬停在目标元素上的时候，浏览器鼠标也许要反映拖放操作的类型。
    [详细信息](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API#%E5%AE%9A%E4%B9%89%E6%8B%96%E5%8A%A8%E6%95%88%E6%9E%9C)

* 定义放置区  
    当拖动一个项目到HTML元素中时，浏览器默认不会有任何响应。想要让一个元素变成可释放区域，该元素必须设置**ondragover**和**ondrop**事件处理程序属性。
    [详细信息](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API#%E5%AE%9A%E4%B9%89%E4%B8%80%E4%B8%AA%E6%94%BE%E7%BD%AE%E5%8C%BA)

* 处理放置效果  
    drop事件的处理程序是以程序指定的方法处理拖动数据。一般，程序调用getData()方法取出拖动项目并按一定方式处理。程序意义根据dropEffect 的值与/或可变更关键字的状态而不同。
    [详细信息](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API#%E5%A4%84%E7%90%86%E6%94%BE%E7%BD%AE%E6%95%88%E6%9E%9C)

* 拖动结束
    拖动操作结束时，在源元素（开始拖动时的目标元素）上触发dragend事件。不管拖动是完成还是被取消这个事件都会被触发。dragend事件处理程序可以检查dropEffect属性的值来确认拖动成功与否。

## 拖放演示
* [使用DataTransfer接口拷贝和移动元素](https://mdn.github.io/dom-examples/drag-and-drop/copy-move-DataTransfer.html)
* [使用DataTransferListItem接口拷贝和移动元素](http://mdn.github.io/dom-examples/drag-and-drop/copy-move-DataTransferItemList.html)
* [拖放文件](https://jsbin.com/hiqasek/edit?html,js,output)


## 参考文献
[1] [HTML 拖放 API](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API)  
[2] [Drag​Event](https://developer.mozilla.org/zh-CN/docs/Web/API/DragEvent)  
[3] [Data​Transfer](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer)  
[4] [Data​Transfer​Item](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransferItem)  
[5] [Data​Transfer​Item​List](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransferItemList)