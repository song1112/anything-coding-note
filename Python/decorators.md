#裝飾器(Decorators)

###關於函數
在說明裝飾器之前，需要知道函數的幾個特性

- ####函數是對象

		def out():
   			return 'this is out'

	a = out # 將a變數賦予函數out
	print a() # 使用a變數
	
	當函數out被del時，變數a還是會存在

		del out
		print a() # 可以執行
		print out() # 發生NameError錯誤

- ####函數	中可定義函數(閉包)
在函數中可以定義函數，但是函數內的函數不能被外面給呼叫

		def out():
			print 'this is outside'

    	def in1():
        	return 'this is in1'

    	def in2():
        	return 'this is in2'
    
    	print in1()
    	print in2()
		
		out()
		in1() # 這是不成立的

- ####函數中可以返回函數

    	def out():
        	print 'this is out'
    		def in1():
        		return 'this is in1'
    		return in1()

		print out()
	
- ####函數可以作為參數傳遞

		def out(func):
    		print 'this is out'
    		print func()

		def func1():
    		return 'this is func1'

		out(func1)
	
###裝飾器
首先先看此程式碼

	def out(func):
    	print 'this is out'
    	def in1():
        	print 'this is in1 before func()'
        	func()
        	print 'this is in1 after func()'
    	return in1

	def func1():
    	print 'this is func1'

	f = out(func1)
	f()
	
	""" show
	this is out
	this is in1 before func()
	this is func1
	this is in1 after func()
	"""
這段程式碼代表的意思是呼叫out()並傳入func1()，執行func1()，這就等同於裝飾器的功能。
  
建立一個裝飾器需讓被作為裝飾器的函數傳入其他函數，並且定義函數內部的函數，以及回傳函數內部的函數

	def out(func):
    	print 'this is out'
    	def in1():
        	print 'this is in1 before func()'
        	func()
        	print 'this is in1 after func()'
    	return in1

	@out
	def func1():
    	print 'this is func1'

	func1()
	
	""" show
	this is out
	this is in1 before func()
	this is func1
	this is in1 after func()
	"""

但是當呼叫print func1.\_\_name__時，結果會顯示in1，這是因為func1被in1給替代了，他會重寫函數的名字和註釋文檔，可以使用functools的wraps來解決此一問題

####functools.wraps
使用wraps需在內部函數裡加標籤@wraps(func)

	from functools import wraps
	def out(func):
    	print 'this is out'
    	@wraps(func)
    	def in1():
        	print 'this is in1 before func()'
        	func()
        	print 'this is in1 after func()'
    	return in1

	@out
	def func1():
    	print 'this is func1'

	func1()
	print func1.__name__ # result: func1
	
使用*args和\*\*kwargs

	from functools import wraps

	def out(func):
   		print 'this is out'
    	@wraps(func)
    	def in1(*args):
        	print args
        	func(*args)
        	print 'this is in1'
        	return func(*args)
    	return in1

	@out
	def func1(x):
    	print 'this is func1'

	func1(2)
	""" show
	this is out
	(2,)
	this is func1
	this is in1
	this is func1
	"""

####接受參數的裝飾器
裝飾器需要內包兩個函數，最外層用來傳入裝飾器的值，第二層接受函數並回傳第三層的函數，第三層用來接受func的*args和\*\*kwargs

	from functools import wraps

	def out(x='x'):
    	print 'this is out'
    	def in1(func):
        	print 'in1'
        	@wraps(func)
        	def inin(*args, **kwargs):
            	print args
            	print 'this is inin'
            	print 'x=', x
            	return func(*args, **kwargs)
        	return inin
    	return in1

	@out(x='a')
	def func1():
    	print 'this is func1'

	func1()
	
	""" show
	this is out
	in1
	()
	this is inin
	x= a
	this is func1
	"""