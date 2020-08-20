## 获取说明和帮助
python获得对象的所有属性和方法  
dir(Obkect)  

要判断对象是否有某个属性，可以用：  
hasattr(Object, "name")  

查看帮助文档：  
help(Object)  

查看属性：  
type(Object)  

查看信息：  
print(func_name.__doc__)  

模块帮助查询：  
help(module_name)  
dir(module_name)  

id()函数可返回对象的内存地址：  
id(Object)

## 深拷贝和浅拷贝
* 直接赋值：其实就是对象的引用（别名）。  
* 浅拷贝（copy）：拷贝父对象，不会拷贝对象的内部的子对象。  
* 深拷贝（deepcopy）：copy模块的deepcopy方法，完全拷贝了父对象及其子对象。  
https://www.runoob.com/w3cnote/python-understanding-dict-copy-shallow-or-deep.html  
