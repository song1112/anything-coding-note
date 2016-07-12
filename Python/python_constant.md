#Python 常量

在Python中沒有常量，因此需要使用到常量的情形時，就須自行定義常量模組
filename: const.py

	# coding: utf-8

	""" This is constant module """
	class Const(object):
    	class ConstError(TypeError):  
        	pass

    	def __setattr__(self, key, value):
        	if self.__dict__.has_key(key):
            	raise self.ConstError, "cannot change const.%s" % key
        	self.__dict__[key] = value

    	def __getattr__(self, key):
        	if self.__dict__.has_key(key):
            	return self.key
        	return None

	import sys
	sys.modules[__name__] = Const()
####說明
- 定義常量例外ConstError繼承TypeError
- 定義\_\_setattr__方法，當對象被賦值時會被調用，因此我們要在這邊進行是否已有key的判斷，若在有key的情況下被調用代表使用者嘗試變更值，因此引發ConstError例外
- 定義\_\_getattr__，當對象被訪問時會被調用，這邊可以省略
- 使用sys.modules[\_\_name__]，可以獲取模組的對象，並可通過該對象獲取模組的屬性，這個作用是為了向系統字典注入const對象來實現調用import const時獲得Const實例

關於sys.modules官方註釋如下：

> sys.modules  
This is a dictionary that maps module names to modules which have already been loaded. This can be manipulated to force reloading of modules and other tricks. Note that removing a module from this dictionary is not the same as calling reload() on the corresponding module object.

####使用

	>>> import const
	>>> const.NAME = 'Song'
	>>> const.EMAIL = 'email@mail.com'
	>>> const.NAME = 'Yee'
	Traceback (most recent call last):
  		File "<stdin>", line 1, in <module>
  		File "const.py", line 10, in __setattr__
    		raise self.ConstError, "cannot change const.%s" % key
	const.ConstError: cannot change const.NAME

##Reference

[Python中实现常量（Const）功能](http://www.jb51.net/article/60485.htm)
[python中定义常量](http://michaelyou.github.io/2015/06/13/python中定义常量/)
