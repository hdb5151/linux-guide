# 搜索
	1、in:name 关键词		#按仓库名称查找
	2、in:description 关键词	#按内容的关键词查找	
	3、in:readme 关键词		#在项目README中增加检索条件
	4、stars: > 数字  关键字	#按标星数 和关键字查找
	5、stars: 10..20 关键词		#按标星数区间 和关键字查找
	6、size:>=5000 关键词		#按工程大小和关键字查找   单位是K
	7、pushed:>2019-01-03 关键词	#按项目上传的时间（前或后）和关键词查找
	8、created:>2019-01-03 关键词	#按仓库创建的时间（前或后）和关键词查找
	9、language:java 关键词		#按语言类型和关键词查找
	10、user:joshlong		#按用户查找


# 创建仓库，提交代码
>* 配置库存的用户名
>>* git config --global user.name "hdb5151"
>* 配置仓库的邮箱
>>* git config --global user.email "hdb5151@163.com"
>* 初始化git, 
>>* git init
>>>* 初始化后，会在当前目录生成.git 隐藏文件
>* 将目录下的文件添加到git版本控制系统中
>>* git add 文件名
>>>* 将“文件名” 文件添加到git控制系统中，
>> git add .
>>>* 将当前目录里的所有文件都添加到git版本控制系统中。
>* 提交
>>* git commit
>>>* 会弹出 vim编辑框，可以写代码的版本信息或修改说明之类的。
>>* git commit -m "提交说明"
>>>* 使用-m 参数后，就不会弹出vim编辑框，可以直接添加修改说明
>* 查看修改的信息
>>* git log
>* 回退操作
>>* git reset --hard "commit id"
>>>* **commit id** 可以通过 **git log** 查询， 回退到以前的版本后，后面的版本会被删除。
>* 创建分支，并行开发
>>* git branch "verName"
>* 多分支之间切换
>>* git checkout "verName"
0.4