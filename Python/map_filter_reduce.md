#Map、Filter、Reduce

###Map
map(func, list)：將func作用到list的所有元素上

	In [1]: list = [1,2,3,4,5]

	In [2]: list2 = map(lambda x: x*x, list)

	In [3]: list2
	Out[3]: [1, 4, 9, 16, 25]

###Filter
filter(func, list)：使用func過濾list的元素

	In [4]: list3 = filter(lambda x: x>3, list)

	In [5]: list3
	Out[5]: [4, 5]
###Reduce
對串列中的元素作連續運算

	In [1]: from functools import reduce

	In [2]: list = [1,2,3,4,5]

	In [3]: list4 = reduce((lambda x,y:x+y), list)

	In [4]: list4
	Out[4]: 15