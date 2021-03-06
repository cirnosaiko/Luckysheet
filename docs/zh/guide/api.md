# API

Luckysheet针对常用的数据操作需求，开放了主要功能的API，开发者可以根据需要进行任意对接开发。

使用注意：
1. 所有方法均挂载到window.luckysheet对象下面，可以在浏览器控制台打印看到
2. `success`回调函数第一个参数为API方法的返回值
3. 需要新的API请到github [Issues](https://github.com/mengshukeji/Luckysheet/issues/new/choose)中提交，根据点赞数决定是否开放新API
3. ⚠️新的API正在整理，旧API可能随时变动，请谨慎使用并随时关注官网更新！

## 单元格操作

### getCellValue(row, column [,setting])

- **参数**：
	
	- {Number} [row]: 单元格所在行数；从0开始的整数，0表示第一行
	- {Number} [column]: 单元格所在列数；从0开始的整数，0表示第一列
	- {PlainObject} [setting]: 可选参数
		+ {String} [type]: 单元格的值，可以设置为"v"：原始值 或者"m"：显示值；默认值为'v',表示获取单元格的实际值
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	获取单元格的值。

	特殊情况，单元格格式为`yyyy-MM-dd`，`type`为`'v'`时会强制取`'m'`显示值

- **示例**:

	- 返回当前工作表第1行第1列单元格的数据的v值
		
		`luckysheet.getCellValue(0, 0)`

	- 返回指定data数据的第2行第2列单元格的原始值。
		
		`luckysheet.getCellValue(1, 1, {type:"m"})`

------------

### setCellValue(row, column, value [,setting])

- **参数**：
    - {Number} [row]: 单元格所在行数；从0开始的整数，0表示第一行
    - {Number} [column]: 单元格所在列数；从0开始的整数，0表示第一列
    - {Object | String | Number} [value]: 要设置的值；可以为字符串或数字，或为符合Luckysheet单元格格式的对象，参考 [单元格属性表](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/format.html#%E5%8D%95%E5%85%83%E6%A0%BC%E5%B1%9E%E6%80%A7%E8%A1%A8)
    - {PlainObject} [setting]: 可选参数
        + {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：

    设置某个单元格的值，也可以设置整个单元格对象，用于同时设置多个单元格属性

- **示例**:

    - 设置当前工作表"A1"单元格的值为"abc"
    `luckysheet.setCellValue(0, 0, 'abc');`

------------

### setCellFormat(row, column, attr, value [,setting])

- **参数**：
	- {Number} [row]: 单元格所在行数；从0开始的整数，0表示第一行
	- {Number} [column]: 单元格所在列数；从0开始的整数，0表示第一列
    - {String} [attr]: 属性类型，参考 [单元格属性表](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/format.html#%E5%8D%95%E5%85%83%E6%A0%BC%E5%B1%9E%E6%80%A7%E8%A1%A8)的属性值
	- {String | Number | Object} [value]: 具体的设置值，一个属性会对应多个值，参考 [单元格属性表](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/format.html#%E5%8D%95%E5%85%83%E6%A0%BC%E5%B1%9E%E6%80%A7%E8%A1%A8)的值示例，特殊情况：如果属性类型`attr`是单元格格式`ct`，则设置值`value`应提供`ct.fa`，比如设置A1单元格的格式为百分比格式：
	  
  	  `luckysheet.setCellFormat(0, 0, "ct", "0.00%")`

	- {PlainObject} [setting]: 可选参数
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	设置某个单元格的属性，如果要设置单元格的值或者同时设置多个单元格属性，推荐使用`setCellValue`
	
	特殊的设置
    
	- 边框设置时，attr为`"bd"`，value为一个key/value对象，需要同时设置边框类型:`borderType`/边框粗细:`style`/边框颜色:`color`/，比如设置A1单元格的边框为所有/红色/细：
	  
  	  `luckysheet.setCellAttr(0, 0, "bd", {borderType: "border-right",style: "1", color: "#ff0000"})`
		
	  完整可选的设置参数如下：

      + 边框类型 `borderType："border-left" | "border-right" | "border-top" | "border-bottom" | "border-all" | "border-outside" | "border-inside" | "border-horizontal" | "border-vertical" | "border-none"`，
      + 边框粗细 `style:  1 Thin | 2 Hair | 3 Dotted | 4 Dashed | 5 DashDot | 6 DashDotDot | 7 Double | 8 Medium | 9 MediumDashed | 10 MediumDashDot | 11 MediumDashDotDot | 12 SlantedDashDot | 13 Thick`
      + 边框颜色 `color: 16进制颜色值`

- **示例**:

   - 设置当前工作表A1单元格文本加粗
   		`luckysheet.setCellFormat(0, 0, "bl", 1)`
   - 设置第二个工作表的B2单元格背景为红色
   		`luckysheet.setCellFormat(1, 1, "bg", "#ff0000", {order:1})`
   - 设置当前工作表"A1"单元格的值为"abc"
   		`luckysheet.setCellFormat(0, 0, 'v', 'abc');`

------------

### find(content [,setting])

- **参数**：
	
	- {String} [content]: 要查找的内容
	- {PlainObject} [setting]: 可选参数
		+ {Boolean} [isRegularExpression]: 是否正则表达式匹配；默认为 `false`
		+ {Boolean} [isWholeWord]: 是否整词匹配；默认为 `false`
		+ {Boolean} [isCaseSensitive]: 是否区分大小写匹配；默认为 `false`
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	查找一个工作表中的指定内容，返回查找到的内容组成的单元格一位数组，数据格式同`celldata`。

------------

### replace(content, replaceContent [,setting])

- **参数**：
	
	- {String} [content]: 要查找的内容
	- {String} [replaceContent]: 要替换的内容
	- {PlainObject} [setting]: 可选参数
		+ {Boolean} [isRegularExpression]: 是否正则表达式匹配；默认为 `false`
		+ {Boolean} [isWholeWord]: 是否整词匹配；默认为 `false`
		+ {Boolean} [isCaseSensitive]: 是否区分大小写匹配；默认为 `false`
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	查找一个工作表中的指定内容并替换成新的内容，返回替换后的内容组成的单元格一位数组，数据格式同`celldata`。

------------

## 行和列操作

### setHorizontalFrozen(isRange [,setting])

- **参数**：
	- {Boolean} [isRange]: 是否冻结行到选区
		`isRange`可能的值有：
		
		+ `"false"`: 冻结首行
		+ `"true"`: 冻结行到选区
	- {PlainObject} [setting]: 可选参数
    	+ {Array | Object | String} [range]: `isRange`为`true`的时候设置，开启冻结的选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	冻结行操作

	特别注意，只有在`isRange`设置为`true`的时候，才需要设置`setting`中的`range`。

------------

### setVerticalFrozen(isRange [,setting])

- **参数**：
	- {Boolean} [isRange]: 是否冻结列到选区
		`isRange`可能的值有：
		
		+ `"false"`: 冻结首列
		+ `"true"`: 冻结列到选区
	- {PlainObject} [setting]: 可选参数
    	+ {Array | Object | String} [range]: `isRange`为`true`的时候设置，开启冻结的选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	冻结列操作

	特别注意，只有在`isRange`设置为`true`的时候，才需要设置`setting`中的`range`。

------------

### setBothFrozen(isRange [,setting])

- **参数**：
	- {Boolean} [isRange]: 是否冻结行列到选区
		`isRange`可能的值有：
		
		+ `"false"`: 冻结行列
		+ `"true"`: 冻结行列到选区
	- {PlainObject} [setting]: 可选参数
    	+ {Array | Object | String} [range]: `isRange`为`true`的时候设置，开启冻结的选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	冻结行列操作

	特别注意，只有在`isRange`设置为`true`的时候，才需要设置`setting`中的`range`。

------------

### cancelFrozen([setting])

- **参数**：

	- {PlainObject} [setting]: 可选参数
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	取消冻结操作

------------

### insertRow( row [,setting])

- **参数**：

	- {Number} [row]: 在第几行插入空白行

	- {PlainObject} [setting]: 可选参数
		+ {Number} [number]: 插入的空白行数；默认为 1
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	在第`row`行的位置，插入`number`行空白行

------------

### insertColumn( column [,setting])

- **参数**：

	- {Number} [column]: 在第几列插入空白列

	- {PlainObject} [setting]: 可选参数
		+ {Number} [number]: 插入的空白列数；默认为 1
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	在第`column`列的位置，插入`number`列空白列

------------

### deleteRow(rowStart, rowEnd [,setting])

- **参数**：
	
	- {Number} [rowStart]: 要删除的起始行
	- {Number} [rowEnd]: 要删除的结束行
	
	- {PlainObject} [setting]: 可选参数
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	删除指定的行

	特别提醒，删除行之后，行的序号并不会变化，注意观察数据是否被正确删除即可。

------------

### deleteColumn(columnStart, columnEnd [,setting])

- **参数**：
	
	- {Number} [columnStart]: 要删除的起始列
	- {Number} [columnEnd]: 要删除的结束列
	
	- {PlainObject} [setting]: 可选参数
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	删除指定的列

	特别提醒，删除列之后，列的序号并不会变化，注意观察数据是否被正确删除即可。

------------

### hideRow(rowStart, rowEnd [,setting])

- **参数**：
	
	- {Number} [rowStart]: 要隐藏的起始行
	- {Number} [rowEnd]: 要隐藏的结束行
	
	- {PlainObject} [setting]: 可选参数
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	隐藏指定的行

	特别提醒，隐藏行之后，行的序号会变化。

------------

### hideColumn(columnStart, columnEnd [,setting])(TODO)

- **参数**：
	
	- {Number} [columnStart]: 要隐藏的起始列
	- {Number} [columnEnd]: 要隐藏的结束列
	
	- {PlainObject} [setting]: 可选参数
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	隐藏指定的列

	特别提醒，隐藏列之后，列的序号会变化。

------------

### showRow(rowStart, rowEnd [,setting])

- **参数**：
	
	- {Number} [rowStart]: 要显示的起始行
	- {Number} [rowEnd]: 要显示的结束行
	
	- {PlainObject} [setting]: 可选参数
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	显示指定的行

------------

### showColumn(columnStart, columnEnd [,setting])(TODO)

- **参数**：
	
	- {Number} [columnStart]: 要显示的起始列
	- {Number} [columnEnd]: 要显示的结束列
	
	- {PlainObject} [setting]: 可选参数
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	显示指定的列

------------

## 选区操作

### getRangeValue([setting])

- **参数**：

	- {PlainObject} [setting]: 可选参数
		+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	返回指定工作表指定范围的单元格二维数组数据，每个单元格为一个对象。

	[单元格对象格式参考](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/format.html#%E5%8D%95%E5%85%83%E6%A0%BC%E5%B1%9E%E6%80%A7%E8%A1%A8)

------------

### getRangeHtml([setting])

- **参数**：

	- {PlainObject} [setting]: 可选参数
    	+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，允许多个选区组成的数组；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	复制指定工作表指定单元格区域的数据，返回包含`<table>`html格式的数据，可用于粘贴到excel中保持单元格样式。
	
	特别注意，如果复制多个选区，这几个选区必须有相同的行或者相同的列才能复制，复制出的结果也会自动合并成衔接的数组，比如，多选`"C18:C20"` / `"E18:E20"` / `"G18:H20"`是允许的，但是多选`"C18:C20"` / `"E18:E21"`是不允许的

------------

### getRangeJson(title [,setting])

- **参数**：

    - {Boolean} [title]: 是否首行为标题

		`title`可能的值有：
		
		+ `"true"`: 首行为标题
		+ `"false"`: 首行不为标题
	- {PlainObject} [setting]: 可选参数
		+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	复制指定工作表指定单元格区域的数据，返回`json`格式的数据

------------

### getRangeArray(dimensional [,setting])

- **参数**：

    - {String} [dimensional]: 数组维度
		
		`dimensional`可能的值有：
		
		+ `"oneDimensional"`: 一维数组
		+ `"twoDimensional"`: 二维数组
		+ `"custom"`: 自定义维数组
	- {PlainObject} [setting]: 可选参数
		+ {Number} [row]: `dimensional`为`custom`的时候设置，多维数组的行数
		+ {Number} [column]: `dimensional`为`custom`的时候设置，多维数组的列数
		+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	复制指定工作表指定单元格区域的数据，返回一维、二维或者自定义维数组格式的数据。

	特别注意，只有在`dimensional`设置为`custom`的时候，才需要设置`setting`中的`row`和`column`

------------

### getRangeDiagonal(type [,setting])

- **参数**：

	- {String} [type]: 对角线还是对角线偏移
	
		`type`可能的值有：
		
		+ `"normal"`: 对角线
		+ `"offset"`: 对角线偏移
	- {PlainObject} [setting]: 可选参数
		- {Number} [column]: `type`为`offset`的时候设置，对角偏移的列数
		+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	复制指定工作表指定单元格区域的数据，返回对角线或者对角线偏移`column`列后的数据。

	特别注意，只有在`type`设置为`offset`的时候，才需要设置`setting`中的`column`。

------------

### getRangeBoolean([setting])

- **参数**：

	- {PlainObject} [setting]: 可选参数
		+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	复制指定工作表指定单元格区域的数据，返回布尔值的数据

------------

### setRangeShow(range [,setting])

- **参数**：

	- {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，允许多个选区组成的数组；默认为当前选区
	- {PlainObject} [setting]: 可选参数
    	+ {Boolean} [show]: 是否显示高亮选中效果；默认值为 `true`
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	指定工作表选中一个或多个选区为选中状态并选择是否高亮，支持多种格式设置。

	特别提醒，Luckysheet中涉及到的选区范围设置都可以参考这个设置

- **示例**:

     + 设定当前工作表选区范围`A1:B2`: 
      
		`luckysheet.setRangeShow("A1:B2")`
     + 设定选区范围`A1:B2`: 
  		
		`luckysheet.setRangeShow(["A1:B2"])`
     + 设定选区范围`A1:B2`: 
  
  		`luckysheet.setRangeShow({row:[0,1],column:[0,1]})`
     + 设定选区范围`A1:B2`: 
  
  		`luckysheet.setRangeShow([{row:[0,1],column:[0,1]}])`
     + 设定选区范围`A1:B2`和`C3:D4`:  
  
		`luckysheet.setRangeShow(["A1:B2","C3:D4"])`
     + 设定选区范围`A1:B2`和`D3`: 
  
  		`luckysheet.setRangeShow([{row:[0,1],column:[0,1]},{row:[2,2],column:[3,3]}])`

------------

### setRangeValue(data [,setting])

- **参数**：

	- {Array} [data]: 要赋值的单元格二维数组数据，每个单元格的值，可以为字符串或数字，或为符合Luckysheet格式的对象，参考 [单元格属性表](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/format.html#%E5%8D%95%E5%85%83%E6%A0%BC%E5%B1%9E%E6%80%A7%E8%A1%A8)
	- {PlainObject} [setting]: 可选参数
		+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	将一个单元格数组数据赋值到指定的区域，数据格式同`getRangeValue`方法取到的数据。

------------

### setRangeFormat(attr, value [,setting])

- **参数**：

    - {String} [attr]: 属性类型，
  	参考 [单元格属性表](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/format.html#%E5%8D%95%E5%85%83%E6%A0%BC%E5%B1%9E%E6%80%A7%E8%A1%A8)的属性值
	- {String | Number | Object} [value]: 具体的设置值，一个属性会对应多个值，参考 [单元格属性表](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/format.html#%E5%8D%95%E5%85%83%E6%A0%BC%E5%B1%9E%E6%80%A7%E8%A1%A8)的值示例，特殊情况：如果属性类型`attr`是单元格格式`ct`，则设置值`value`应提供`ct.fa`，比如设置`"A1:B2"`单元格的格式为百分比格式：
	  
  	  `luckysheet.setRangeFormat("ct", "0.00%", {range:"A1:B2"})`

    - {PlainObject} [setting]: 可选参数
    	+ {Object | String} [range]: 设置参数的目标选区范围，支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，允许多个选区组成的数组；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	  
	设置指定范围的单元格格式，一般用作处理格式，赋值操作推荐使用`setRangeValue`方法
    
	- 边框设置时，attr为`"bd"`，value为一个key/value对象，需要同时设置边框类型:`borderType`/边框粗细:`style`/边框颜色:`color`/，比如设置`"A1:B2"`单元格的边框为所有/红色/细：
	  
  	  `luckysheet.setRangeFormat("bd", {borderType: "border-right",style: "1", color: "#ff0000"}, {range:["A1:B2"]})`
		
	  完整可选的设置参数如下：

      + 边框类型 `borderType："border-left" | "border-right" | "border-top" | "border-bottom" | "border-all" | "border-outside" | "border-inside" | "border-horizontal" | "border-vertical" | "border-none"`，
      + 边框粗细 `style:  1 Thin | 2 Hair | 3 Dotted | 4 Dashed | 5 DashDot | 6 DashDotDot | 7 Double | 8 Medium | 9 MediumDashed | 10 MediumDashDot | 11 MediumDashDotDot | 12 SlantedDashDot | 13 Thick`
      + 边框颜色 `color: 16进制颜色值`

- **示例**:

   - 设置当前工作表`"A1:B2"`范围的单元格文本加粗
   `luckysheet.setRangeFormat("bl", 1, {range:"A1:B2"})`
   - 设置第二个工作表的`"B2"`和`"C4:D5"`范围的单元格背景为红色
   `luckysheet.setRangeFormat("bg", "#ff0000", {range:["B2","C4:D5"], order:1})`

------------

### setRangeFilter(type [,setting])

- **参数**：
	- {String} [type]: 打开还是关闭筛选功能
	
		`type`可能的值有：
		
		+ `"open"`: 打开筛选功能，返回当前筛选的范围对象
		+ `"close"`: 关闭筛选功能，返回关闭前筛选的范围对象
	- {PlainObject} [setting]: 可选参数
    	+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	为指定索引的工作表，选定的范围开启或关闭筛选功能

- **示例**:

	- 打开第二个工作表"A1:B2"范围的筛选功能
	`luckysheet.setRangeFilter("open",{range:"A1:B2",order:1})`

------------

### setRangeMerge(type [,setting])

- **参数**：
	- {String} [type]: 合并单元格类型
	
		`type`可能的值有：
		
		+ `"all"`: 全部合并，区域内所有单元格合并成一个大的单元格
		+ `"horizontal"`: 水平合并，区域内在同一行的单元格合并成一个单元格
		+ `"vertical"`: 垂直合并，区域内在同一列的单元格合并成一个单元格
	- {PlainObject} [setting]: 可选参数
    	+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，允许多个选区组成的数组；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	为指定索引的工作表，选定的范围设定合并单元格

------------

### cancelRangeMerge( [setting])

- **参数**：
	
	- {PlainObject} [setting]: 可选参数
    	+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，允许多个选区组成的数组；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	为指定索引的工作表，选定的范围取消合并单元格

------------

### setRangeSort(type [,setting])

- **参数**：

	- {String} [type]: 排序类型
	
		`type`可能的值有：
		
		+ `"asc"`: 升序
		+ `"des"`: 降序
		
	- {PlainObject} [setting]: 可选参数
		
    	+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	为指定索引的工作表，选定的范围开启排序功能，返回选定范围排序后的数据。

- **示例**:

   - 设置当前工作表当前选区为升序
   `luckysheet.setRangeSort("asc")`

------------

### setRangeSortMulti(title, sort [,setting])

- **参数**：

	- {Boolean} [title]: 数据是否具有标题行
	- {Array} [sort]: 列设置，设置需要排序的列索引和排序方式，格式如：`[{ i:0,sort:'asc' },{ i:1,sort:'des' }]`
	- {PlainObject} [setting]: 可选参数
		
    	+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	为指定索引的工作表，选定的范围开启多列自定义排序功能，返回选定范围排序后的数据。

- **示例**:

   - 设置当前工作表当前选区为自定义排序，数据具有标题行，且按第一列升序第二列降序的规则进行排序
   `luckysheet.setRangeSortMulti(true,{sort:[{ i:0,sort:'asc' },{ i:1,sort:'des' }]})`

------------

### setRangeConditionalFormatDefault(conditionName, conditionValue [,setting])

- **参数**：

	- {String} [conditionName]: 条件格式规则类型
	
		`conditionName`可能的值有：
		
		+ `"greaterThan"`: 大于
		+ `"lessThan"`: 小于
		+ `"betweenness"`: 介于
		+ `"equal"`: 等于
		+ `"textContains"`: 文本包含
		+ `"occurrenceDate"`: 发生日期
		+ `"duplicateValue"`: 重复值
		+ `"top"`: 前 N 项（可以在conditionValue中设置其他值）
		+ `"topPercent"`: 前 N%（可以在conditionValue中设置其他值）
		+ `"last"`: 后 N 项（可以在conditionValue中设置其他值）
		+ `"lastPercent"`: 后 N%（可以在conditionValue中设置其他值）
		+ `"AboveAverage"`: 高于平均值
		+ `"SubAverage"`: 低于平均值
		 
	- {Object} [conditionValue]: 可以设置条件单元格或者条件值
		取值规则
		```js
		{
			type: 'value',
			content: [2]
		}
		```
		或者
		```js
		{
			type: 'range',
			content: ['A1']
		}
		```
	
	- {PlainObject} [setting]: 可选参数
		
      	+ {Object | Array} [format]: 颜色设置
      	  
    		* `type`为`default`时，应设置文本颜色和单元格颜色；默认值为` {
				"textColor": "#000000",
				"cellColor": "#ff0000"
			}`
		+ {Boolean} [isEdit]: 是否在修改条件格式规则；默认为 `false`
    	+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，允许多个选区组成的数组；默认为当前选区
    	
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	为指定索引的工作表，选定的范围开启条件格式，根据设置的条件格式规则突出显示部分单元格，返回开启条件格式后的数据。

- **示例**:

    - 突出显示内容大于数字2的单元格
      `luckysheet.setRangeConditionalFormatDefault("greaterThan",{ type: 'value', content: [2] })`
    
	- 突出显示内容小于单元格A1内容的单元格
	  `luckysheet.setRangeConditionalFormatDefault("lessThan",{ type: 'range', content: ['A1'] })`

	- 突出显示内容介于2和10之间的单元格
	  `luckysheet.setRangeConditionalFormatDefault("betweenness",{ type: 'value', content: [2,10] })`
	
	- 突出显示内容等于单元格A1内容的单元格
	  `luckysheet.setRangeConditionalFormatDefault("equal",{ type: 'range', content: ['A1'] })`
	
	- 突出显示内容包含单元格A1内容的单元格
	  `luckysheet.setRangeConditionalFormatDefault("textContains",{ type: 'range', content: ['A1'] })`
	
	- 突出显示日期在 `2020/09/24 - 2020/10/15` 之间的单元格
      `luckysheet.setRangeConditionalFormatDefault("occurrenceDate",{ type: 'value', content: ['2020/09/24 - 2020/10/15'] })`

	- 突出显示重复值的单元格，content为0
      `luckysheet.setRangeConditionalFormatDefault("duplicateValue",{ type: 'value', content: [0] })`

	- 突出显示唯一值的单元格，content为1
      `luckysheet.setRangeConditionalFormatDefault("duplicateValue",{ type: 'value', content: [1] })`
	
	- 突出显示排名前20名的单元格
      `luckysheet.setRangeConditionalFormatDefault("top",{ type: 'value', content: [20] })`
	
	- 突出显示排名前30%的单元格
      `luckysheet.setRangeConditionalFormatDefault("topPercent",{ type: 'value', content: [30] })`
	
	- 突出显示排名后15名的单元格
      `luckysheet.setRangeConditionalFormatDefault("last",{ type: 'value', content: [15] })`
	
	- 突出显示排名后15%的单元格
      `luckysheet.setRangeConditionalFormatDefault("lastPercent",{ type: 'value', content: [15] })`
	
	- 突出显示高于平均值的单元格
      luckysheet.setRangeConditionalFormatDefault("AboveAverage",{ type: 'value', content: ['AboveAverage'] })`
	
	- 突出显示低于平均值的单元格
	  luckysheet.setRangeConditionalFormatDefault("SubAverage",{ type: 'value', content: ['SubAverage'] })`

------------

### setRangeConditionalFormat(type [,setting])

- **参数**：

	- {String} [type]: 条件格式规则类型
	
		`type`可能的值有：
		
		+ `"dataBar"`: 数据条
		+ `"icons"`: 图标集
		+ `"colorGradation"`: 色阶
		 
	- {PlainObject} [setting]: 可选参数
		
      	+ {Object | Array} [format]: 颜色设置
    	 
		 	* `type`为`dataBar`时，应设置渐变色；默认值为蓝-白渐变` ["#638ec6", "#ffffff"]`

				推荐的快捷取值：
				```js
				["#638ec6", "#ffffff"],  //蓝-白渐变 数据条
				["#63c384", "#ffffff"],  //绿-白渐变 数据条
				["#ff555a", "#ffffff"],  //红-白渐变 数据条
				["#ffb628", "#ffffff"],  //橙-白渐变 数据条
				["#008aef", "#ffffff"],  //浅蓝-白渐变 数据条
				["#d6007b", "#ffffff"],  //紫-白渐变 数据条
				["#638ec6"],  //蓝色 数据条
				["#63c384"],  //绿色 数据条
				["#ff555a"],  //红色 数据条
				["#ffb628"],  //橙色 数据条
				["#008aef"],  //浅蓝色 数据条
				["#d6007b"]   //紫色 数据条
				```
			
			* `type`为`icons`时，应设置图标类型；默认值为"threeWayArrowColor"：三向箭头彩色，

				可取值为：
				
				`threeWayArrowMultiColor`：三向箭头（彩色），
				
				`threeTriangles`：3个三角形，

				`fourWayArrowMultiColor`：四向箭头（彩色），

				`fiveWayArrowMultiColor`：五向箭头（彩色），

				`threeWayArrowGrayColor`：三向箭头（灰色），

				`fourWayArrowGrayColor`：四向箭头（灰色），

				`fiveWayArrowGrayColor`：五向箭头（灰色），

				`threeColorTrafficLightRimless`：三色交通灯（无边框），

				`threeSigns`：三标志，

				`greenRedBlackGradient`：绿-红-黑渐变，

				`threeColorTrafficLightBordered`：三色交通灯（有边框），

				`fourColorTrafficLight`：四色交通灯，

				`threeSymbolsCircled`：三个符号（有圆圈），

				`tricolorFlag`：三色旗，

				`threeSymbolsnoCircle`：三个符号（无圆圈），

				`threeStars`：3个星形，

				`fiveQuadrantDiagram`：五象限图，

				`fiveBoxes`：5个框，

				`grade4`：四等级，

				`grade5`：五等级，

			* `type`为`colorGradation`时，应设置色阶颜色值；默认值为绿-黄-红色阶` ["rgb(99, 190, 123)", "rgb(255, 235, 132)", "rgb(248, 105, 107)"]`

				推荐的快捷取值：
				```js
				["rgb(99, 190, 123)", "rgb(255, 235, 132)", "rgb(248, 105, 107)"],  //绿-黄-红色阶
				["rgb(248, 105, 107)", "rgb(255, 235, 132)", "rgb(99, 190, 123)"],  //红-黄-绿色阶

				["rgb(99, 190, 123)", "rgb(252, 252, 255)", "rgb(248, 105, 107)"],  //绿-白-红色阶
				["rgb(248, 105, 107)", "rgb(252, 252, 255)", "rgb(99, 190, 123)"],  //红-白-绿色阶
				
				["rgb(90, 138, 198)", "rgb(252, 252, 255)", "rgb(248, 105, 107)"],  //蓝-白-红色阶
				["rgb(248, 105, 107)", "rgb(252, 252, 255)", "rgb(90, 138, 198)"],  //红-白-蓝色阶
				
				["rgb(252, 252, 255)", "rgb(248, 105, 107)"],  //白-红色阶
				["rgb(248, 105, 107)", "rgb(252, 252, 255)"],  //红-白色阶

				["rgb(99, 190, 123)", "rgb(252, 252, 255)"],  //绿-白色阶
				["rgb(252, 252, 255)", "rgb(99, 190, 123)"],  //白-绿色阶

				["rgb(99, 190, 123)", "rgb(255, 235, 132)"],  //绿-黄色阶
				["rgb(255, 235, 132)", "rgb(99, 190, 123)"]   //黄-绿色阶
				```
			
    	+ {Boolean} [isEdit]: 是否在修改条件格式规则；默认为 `false`
		+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，允许多个选区组成的数组；默认为当前选区
    	
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	为指定索引的工作表，选定的范围开启条件格式，返回开启条件格式后的数据。

------------

### deleteRangeConditionalFormat(itemIndex [,setting])

- **参数**：

	- {Number} [itemIndex]: 条件格式规则索引
		 
	- {PlainObject} [setting]: 可选参数
		  	
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	为指定索引的工作表，删除条件格式规则，返回被删除的条件格式规则。

- **示例**:

    - 删除第三个条件格式规则
      `luckysheet.editRangeConditionalFormat(2)`
    
------------

### clearRange([setting])

- **参数**：

	- {PlainObject} [setting]: 可选参数
		+ {Array | Object | String} [range]: 要清除的选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，允许多个选区组成的数组；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	清除指定工作表指定单元格区域的内容，返回清除掉的数据，不同于删除选区的功能，不需要设定单元格移动情况

------------

### deleteRange(move [,setting])

- **参数**：
	- {String} [move]: 删除后，右侧还是下方的单元格移动
	
		`move`可能的值有：
		
		+ `"left"`: 右侧单元格左移
		+ `"up"`: 下方单元格上移
		
	- {PlainObject} [setting]: 可选参数
		+ {Array | Object | String} [range]: 要删除的选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，允许多个选区组成的数组；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	删除指定工作表指定单元格区域，返回删除掉的数据，同时，指定是右侧单元格左移还是下方单元格上移

------------

### insertRange(move [,setting])

- **参数**：
	
	- {String} [move]: 活动单元格右移或者下移
	
		`move`可能的值有：
		
		+ `"right"`: 活动单元格右移
		+ `"bottom"`: 活动单元格下移
		
	- {PlainObject} [setting]: 可选参数
		+ {Array} [data]: 赋值到range区域的单元格二维数组数据，[单元格对象格式参考](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/format.html#%E5%8D%95%E5%85%83%E6%A0%BC%E5%B1%9E%E6%80%A7%E8%A1%A8)；默认值为空数组，即插入空白的区域
		+ {Array | Object | String} [range]: 要插入的位置，选区范围，支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，默认为当前选区
    	
			当未设置data数据时，允许多个选区组成的数组，插入的空白区域即为这些选区的区域，
			
			当设置了data数据，只能为单个选区，并且会把data数据插入到当前选区的第一个单元格位置
			
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	在指定工作表指定单元格区域，赋值单元格数据，或者新建一块空白区域，返回data数据，同时，指定活动单元格右移或者下移

------------

### matrixOperation(type [,setting])

- **参数**：
	
	- {String} [type]: 矩阵操作的类型
	
		`type`可能的值有：
		
		+ `"flipUpDown"`: 上下翻转
		+ `"flipLeftRight"`: 左右翻转
		+ `"flipClockwise"`: 顺时针旋转
		+ `"flipCounterClockwise"`: 逆时针旋转
		+ `"Transpose"`: 转置
		+ `"DeleteZeroByRow"`: 按行删除两端0值
		+ `"DeleteZeroByColumn"`: 按列删除两端0值
		+ `"removeDuplicateByRow"`: 按行删除重复值
		+ `"removeDuplicateByColumn"`: 按列删除重复值
		+ `"newMatrix"`: 生产新矩阵
	- {PlainObject} [setting]: 可选参数
    	+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	指定工作表指定单元格区域的数据进行矩阵操作，返回操作成功后的结果数据

------------

### matrixCalculation(type, number [,setting])

- **参数**：
	- {String} [type]: 计算方式
	
		`type`可能的值有：
		
		+ `"plus"`: 加
		+ `"minus"`: 减
		+ `"multiply"`: 乘
		+ `"divided"`: 除
		+ `"power"`: 次方
		+ `"root"`: 次方根
		+ `"log"`: log
		+ `"removeDuplicateByRow"`: 按行删除重复值
		+ `"removeDuplicateByColumn"`: 按列删除重复值
		+ `"newMatrix"`: 生产新矩阵
	- {Number} [number]: 计算数值，如: 2
	- {PlainObject} [setting]: 可选参数
    	+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	指定工作表指定单元格区域的数据进行矩阵计算，返回计算成功后的结果数据

------------

## 工作表操作

### getAllSheets([setting])

- **参数**：

    - {PlainObject} [setting]: 可选参数
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	返回所有工作表配置，格式同[工作表配置](/zh/guide/sheet.html)，得到的结果可用于表格初始化时作为`options.data`使用。

	所以此API适用于，手动操作配置完一个表格后，将所有工作表信息取出来自行保存，再用于其他地方的表格创建。

- **示例**:

	- 取得第一个工作表的所有基本信息
	`luckysheet.getAllSheets()[0]`
	
------------

### getluckysheetfile()

- **参数**：

    - {PlainObject} [setting]: 可选参数
		+ {Function} [success]: 操作结束的回调函数

- **说明**：

	返回所有表格数据结构的一维数组`luckysheetfile`，不同于`getAllSheets`方法，此方法得到的工作表参数会包含很多内部使用变量，最明显的区别是表格数据操作会维护`luckysheetfile[i].data`，而初始化数据采用的是`options.data[i].celldata`，所以`luckysheetfile`可用于调试使用，但是不适用初始化表格。

- **示例**:

	- 取得第一个工作表的所有调试信息
	`luckysheet.getluckysheetfile()[0]`
	
------------

### setSheetAdd([setting])

- **参数**：

    - {PlainObject} [setting]: 可选参数
    	+ {Object} [sheetObject]: 新增的工作表的数据；默认值为空对象
    	+ {Number} [order]: 新增的工作表索引；默认值为最后一个索引位置
    	+ {Function} [success]: 操作结束的回调函数
	
- **说明**：

	新增一个sheet，返回新增的工作表对象，`setting`中可选设置数据为 `sheetObject`，不传`sheetObject`则会新增一个空白的工作表

- **示例**:

	- 在最后一个工作表索引位置新增一个空白的工作表
	`luckysheet.setSheetAdd()`
	
------------

### setSheetDelete([setting])

- **参数**：

    - {PlainObject} [setting]: 可选参数
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
    	+ {Function} [success]: 操作结束的回调函数
	
- **说明**：

	删除指定索引的工作表，返回已删除的工作表对象

- **示例**:

	- 删除当前工作表
	`luckysheet.setSheetDelete()`
			
------------

### setSheetCopy([setting])

- **参数**：

    - {PlainObject} [setting]: 可选参数
		+ {Number} [targetOrder]: 新复制的工作表目标索引位置；默认值为当前工作表索引的下一个索引位置（递增）
    	+ {Number} [order]: 被复制的工作表索引；默认值为当前工作表索引
    	+ {Function} [success]: 操作结束的回调函数
	
- **说明**：

	复制指定索引的工作表到指定索引位置，在`setting`中可选设置指定索引位置`targetOrder`，返回新复制的工作表对象

- **示例**:

	- 复制当前工作表到下一个索引位置
	`luckysheet.setSheetCopy()`

------------

### setSheetHide(hide [,setting])

- **参数**：

	- {Boolean} [hide]: 是否隐藏工作表
		
		`hide`可能的值有的：
		+ `ture`: 隐藏指定索引的工作表，返回被隐藏的工作表对象
		+ `false`: 取消隐藏指定索引的工作表，返回被显示的工作表对象
    - {PlainObject} [setting]: 可选参数
    	+ {Number} [order]: 被隐藏的工作表索引；默认值为当前工作表索引
    	+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	隐藏或取消隐藏指定索引的工作表，返回被隐藏或取消隐藏的工作表对象

- **示例**:

	- 隐藏当前工作表
	`luckysheet.setSheetHide(true)`
	- 取消隐藏第三个工作表
	`luckysheet.setSheetHide(false,{order:2})`

------------

### setSheetActive(order [,setting])

- **参数**：

	- {Number} [order]: 要激活的工作表索引
	- {PlainObject} [setting]: 可选参数
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	设置指定索引的工作表为当前工作表（激活态），即切换到指定的工作表，返回被激活的工作表对象

- **示例**:

	- 切换到第二个工作表
	`luckysheet.setSheetActive(1)`

------------

### setSheetName(name [,setting])

- **参数**：

    - {String} [name]: 新的工作表名称
	- {PlainObject} [setting]: 可选参数
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数
	
- **说明**：
	
	修改工作表名称

- **示例**:

	- 修改当前工作表名称为"CellSheet"
	`luckysheet.setSheetName("CellSheet")`

------------

### setSheetColor(color [,setting])

- **参数**：
	- {String} [color]: 工作表颜色
	- {PlainObject} [setting]: 可选参数
        + {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	设置工作表名称处的颜色

- **示例**:

	- 修改当前工作表名称处的颜色为红色
	`luckysheet.setSheetColor("#ff0000")`

------------

### setSheetMove(type [,setting])

- **参数**：

    - {String | Number} [type]: 工作表移动方向或者移动的目标索引，
		
		`type`可能的值有：
		
		+ `"left"`: 向左
		+ `"right"`: 向右
		+ `1`/`2`/`3`/...: 指定索引
	- {PlainObject} [setting]: 可选参数
    	+ {Number} [order]: 工作表索引；默认值为当前工作表索引
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	指定工作表向左边或右边移动一个位置，或者指定索引，返回指定的工作表对象

- **示例**:

	- 当前工作表向左移动一个位置
	`luckysheet.setSheetMove("left")`
	- 第二个工作表移动到第四个工作表的索引位置
	`luckysheet.setSheetMove(3,{order:1})`

------------

## 工作簿操作

### getScreenshot([setting])

- **参数**：

    - {PlainObject} [setting]: 可选参数
		+ {Array | Object | String} [range]: 选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	返回指定选区截图后生成的base64格式的图片

------------

### setWorkbookName(name [,setting])

- **参数**：

    - {Number} [name]: 工作簿名称
    - {PlainObject} [setting]: 可选参数
    	+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	设置工作簿名称

------------

### undo([setting])

- **参数**：

	- {PlainObject} [setting]: 可选参数
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	撤销当前操作，返回刚刚撤销的操作对象

------------

### redo([setting])

- **参数**：

	- {PlainObject} [setting]: 可选参数
        + {Function} [success]: 操作结束的回调函数

- **说明**：
	
	重做当前操作，返回刚刚重做的操作对象

------------

## 图表

### insertChart([setting])

- **参数**：

    - {PlainObject} [setting]: 可选参数
		+ {Array | Object | String} [range]: 图表数据的选区范围,支持选区的格式为`"A1:B2"`、`"sheetName!A1:B2"`或者`{row:[0,1],column:[0,1]}`，只能为单个选区；默认为当前选区
		+ {Number} [order]: 工作表索引；默认值为当前工作表索引
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	指定工作表指定选区范围生成一个图表，返回图表参数对象，包含图表唯一标识符chart id

------------

### setChart(chartId, attr, value [,setting])

- **参数**：
	
	- {String} [chartId]: 指定要修改的图表id
	- {String} [attr]: 属性类型
		`attr`可能的值有：
		
		+ `"left"`: 左边到工作表边缘的距离
		+ `"top"`: 上边到工作表边缘的距离
		+ `"width"`: 图表外框的宽度
		+ `"height"`: 图表外框的高度
		+ `"chartOptions"`: 图表的详细设置项
    
	- {Number | Object}} [value]: 属性值，当`attr`为`chartOptions`时，直接设置整个chart的配置对象
	
	- {PlainObject} [setting]: 可选参数
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	修改指定id图表的参数，返回修改后的整个图表参数

------------

### getChart(chartId [,setting])

- **参数**：
	
	- {String} [chartId]: 指定要获取的图表id
	
	- {PlainObject} [setting]: 可选参数
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	获取指定id图表的参数

------------

### deleteChart(chartId [,setting])

- **参数**：
	
	- {String} [chartId]: 要删除的图表id
	
	- {PlainObject} [setting]: 可选参数
		+ {Function} [success]: 操作结束的回调函数

- **说明**：
	
	删除指定id图表，返回被删除的图表的参数

------------

## luckysheet.create(options)
- **参数**：
	- {Object} [options]:表格的所有配置信息
- **说明**：
	
	初始化一个luckysheet，可包含多个工作表，参考 [配置列表](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/config.html#container)

------------

## luckysheet.getconfig()
- **说明**：

	快捷返回当前表格config配置，每个工作表的config信息仍然包含在luckysheetfile。

------------
## luckysheet.getluckysheet_select_save()
- **说明**：

	返回当前选区对象的数组，可能存在多个选区。

------------
## luckysheet.getdatabyselection([range] [,sheetOrder])
- **参数**：
	- {Object} [range]：选区对象，`object: { row: [r1, r2], column: [c1, c2] }`；默认为当前第一个选区。
	- {Number} [sheetOrder]：表格下标，从0开始的整数，0表示第一个表格；默认为当前表格下标。
- **说明**：

	返回某个表格第一个选区的数据。
	- `luckysheet.getdatabyselection()`: 返回当前工作表当前选区的数据
	- `luckysheet.getdatabyselection(null,1)`: 返回第2个工作表的当前选区的数据

------------
## luckysheet.luckysheetrefreshgrid(scrollWidth, scrollHeight)
- **参数**：
	- {Number} [scrollWidth]：横向滚动值。默认为当前横向滚动位置。
	- {Number} [scrollHeight]：纵向滚动值。默认为当前纵向滚动位置。
- **说明**：

	按照scrollWidth, scrollHeight刷新canvas展示数据。

------------

## luckysheet.jfrefreshgrid()
- **说明**：

	刷新canvas

------------
## luckysheet.setluckysheet_select_save(v)
- **参数**：
	- {Array} [v]：要设置的选区值(数组)。符合选区格式规则，如`[{ row: [r1, r2], column: [c1, c2] }]`。
- **说明**：
	
	设置当前表格选区的值。配合`luckysheet.selectHightlightShow()`可在界面查看选区改变。
	```js
	luckysheet.setluckysheet_select_save([{ row: [0, 1], column: [0, 1] }]);
	luckysheet.selectHightlightShow();
	```

------------
## luckysheet.selectHightlightShow()
- **说明**：

	高亮当前选区

------------
## luckysheet.setSheetHide(order)
- **参数**：
	- {Number} [order]：表格索引；从0开始的整数，0表示第一个表格；默认为当前表格索引。
- **说明**：

	隐藏某个表格。

------------
## luckysheet.setSheetShow(order)
- **参数**：
	- {Number} [order]：表格索引；从0开始的整数，0表示第一个表格；默认为当前表格索引。
- **说明**：

	显示某个表格。

------------
## luckysheet.flowdata()
- **说明**：
	
	快捷获取当前表格的数据

------------
## luckysheet.buildGridData(file)
- **参数**：
	- {Object} [file]：[luckysheetfile](https://mengshukeji.github.io/LuckysheetDocs/zh/guide/data.html#%E8%8E%B7%E5%8F%96%E8%A1%A8%E6%A0%BC%E6%95%B0%E6%8D%AE)
- **说明**：
	
	生成表格可以识别的二维数组

------------
## luckysheet.getGridData(data)
- **参数**：
	- {Array} [data]：工作表的二维数组数据
- **说明**：
	
	二维数组数据转化成 `{r, c, v}` 格式 一维数组

------------
## luckysheet.destroy()
- **说明**：
	
	删除并释放表格