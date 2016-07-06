#迭代器與生成器
此處主要講的有四大項：

- 可迭代對象(Iterable)
- 迭代器(Iterator)
- 迭代(Iteration)
- 生成器(Generator)

###可迭代對象(Iterable)
只要是定義了\_\_iter__方法返回一個迭代器就可稱為可迭代對象，也就是說只要能提供迭代器的對象就可稱為可迭代對象。

###迭代器(Iterator)
只要定義了next()或者\_\_next__方法，就是一個迭代器

###迭代(Iteration)
從一串列表(或其他地方)取出一個元素的過程，就稱為迭代，例如說使用for迴圈去遍歷某個列表，這個過程就稱為迭代。

###生成器(Generator)
是迭代的一種，但是只能迭代一次，在同個時間，生成器本身只會存在一個值，通常透過yield來創建

	In [1]: def generator_func():
   		...:     for i in range(10):
   		...:         yield i
   		...:         

	In [2]: gen = generator_func()

	In [3]: gen
	Out[3]: <generator object generator_func at 0x10e015f00>

	In [4]: gen.next()
	Out[4]: 0

	In [5]: gen.next()
	Out[5]: 1

	In [6]: next(gen)
	Out[6]: 2

	In [7]: next(gen)
	Out[7]: 3

如果迭代最後一個元素了話就會引發StopIteration錯誤

	next(gen)
	---------------------------------------------------------------------------
	StopIteration                             Traceback (most recent call last)
	<ipython-input-17-8a6233884a6c> in <module>()
	----> 1 next(gen)
	
	StopIteration: 

可使用iter()來返回一個迭代器

	In [1]: str = 'abcdefg'

	In [2]: it = iter(str)

	In [3]: type(it)
	Out[3]: iterator