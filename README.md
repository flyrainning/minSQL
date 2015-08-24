# minSQL
小型mysql单表管理页面，提供单表基本的数据操作

## 安装
下载`minsql.php`，在页面上需要的位置使用`include`包含即可。
*注意*

1. 本页面并独立页面，没有<html><body></body></html>结构，所以可直接包含到您现有系统的页面结构中。若要单独使用，也请建立相应html结构后包含本文件。
2. 页面需使用utf-8编码。
3. 需预先包含`jquery`和`jquery-ui`。需要jquery-ui的dialog支持。
4. 需要[SQLiPlus]()支持，请先包含SQLiPlus数据库操作类。


## 可用设置
打开页面，可通过以下变量进行基本设置。

```php

//设置功能可用性
$add_btn=true;//添加
$edit_btn=true;//编辑
$del_btn=true;//删除


$page_title='分组管理';//设置标题，如果不设置，则自动取当前页面菜单名称
$sql_table='group';//设置数据库要处理的数据表，若不设置，则自动通过当前文件名取同名数据表



$headers=array(//表头重命名，'数据库字段'=>'显示名称',不设置则显示数据库中该字段的备注，没有备注则显示字段名
	//'code'=>'编码',
	
	
);
$_defval=array(//设置字段的默认值，在添加新纪录时会自动代入到表单中。
	//'code'=>'00001',
	//'date'=>date('Y-m-d'),
	'status'=>'1',


);
$_field_type=array(//字段类型设置，根据不同类型会有不同的输入框，如果没有会自动判断数据库字段类型。。如果字段有外键，会自动启用外键值，此处设置将无效。
			//类型包括  vchar(int) text int(int) date time datetime [array(...)]  其中 array() 会生成一个下拉框 可处理如状态字段 示例如下：
			
	/*
	'id'=>'int(11)',
	'name'=>'vchar(50)',
	'remark'=>'text',
	'date'=>'datetime',
	'select'=>array(
		'key1'=>'value 1',
		'key2'=>'value 2',
	),
	status'=>array(
		'1'=>'启用',
		'2'=>'禁用',
	),
	*/
	'status'=>array(
		'1'=>'正常',
		'2'=>'停用',
	),
	
);
$_hide_field=array(//要隐藏的字段，注意，隐藏的字段必须在数据库中设置默认值，否则添加新纪录时会产生错误。主键不能隐藏，隐藏后结果界面，添加，编辑界面均不显示。若要显示但不能修改，使用 $_disabled 进行配置
	//'field_name1','field_name2',
	//'__act__', //表示 操作 列
	//'remark',
	

);
$_pretreatment=array(//结果预处理功能，'数据库字段'=>预处理函数体,输入当前字段值，需要函数return处理后的结果，显示在页面中。。预处理不影响添加和编辑界面的显示
	'status'=>function($v){
		return ($v=='1')?'正常':'停用';
	},
	'role'=>function($v){
		global $tableheader;
	
		return $tableheader['role']['fg'][$v];
	},
	
	

);
$_field_note=array(//字段输入提示，在添加和编辑页面显示，提示用户输入内容
	//'status'=>'1 表示启用，2 表示禁用',
	'name'=>'分组名称，必填',

);

$_necessarily=array(//规定必填项  外键无效
	//'id','name',
	'name'
	
);

$_disabled=array(//规定禁填项，禁填项用户无法输入，可以有默认值  外键无效 select无效
	//'id','name',
	'gid'
	

);

$__FG=array(//外键设置，如果字段已经在数据库中设置外键，这里再次设置，会覆盖该字段数据库中的外键设置。每个设置一个array，说明如下
	
	array(
		'COLUMN_NAME'=>'role',//本表字段名，在这个字段使用外键功能
		'REFERENCED_TABLE_NAME'=>'role',//外键所连接的表名
		'REFERENCED_COLUMN_NAME'=>'rid',//外键“值”对应的字段，如id。这个值将作为 字段值 进行数据库操作
		'REFERENCED_COLUMN_TITLE'=>'name',//外键值对应的显示名称，显示在下拉框中的名称，如name。可以留空，留空select中将显示 “值” 的值。
		
	),
	

);



```

