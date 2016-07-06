#三元運算符

###Case1:[is\_true] if [condition] else [is_false]:

	In [1]: print 'yes' if 5>2 else 'no'
	yes

###Case2:

	In [2]: value = ['a', 'b'][1>2]

	In [3]: print value
	a
	
當中因為1>2為False，返回的是0，故顯示出串列第0個元素
