#關於List的變動

這篇要講的是關於List的變動(Mutation)  
首先從最簡單的範例來看：

	>>> a = [1,2,3,4]
	>>> b = a
	>>> b.append(5)
	>>> a
	[1, 2, 3, 4, 5]

這時候會發現a被改變了，這是因為b=a代表b和a指向了同個記憶體空間，那麼要如何拷貝list呢?可以透過遞增1的方式一個一個指派給b：

	>>> a
	[1, 2, 3, 4, 5]
	>>> b = a[::1]
	>>> b.append(6)
	>>> a
	[1, 2, 3, 4, 5]
	>>> b
	[1, 2, 3, 4, 5, 6]


接著還有個範例是關於函數的變動：

	>>> def add_one(result=[]):
	...     result.append(1)
	...     return result
	... 
	>>> add_one()
	[1]
	>>> add_one()
	[1, 1]

會有上面的結果是因為當函數被調用時，默認參數會運算一次，而不是重新運算，解決方法如下：

	>>> def add_one(result=[]):
	...     if result==[]:
	...         result=[]
	...     result.append(1)
	...     return result
	... 
	>>> add_one()
	[1]
	>>> add_one()
	[1]
	
