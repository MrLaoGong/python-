#正则表达式与Cookie的使用
###正则表达式基础知识
1. 原子  
原子是正则表达式中最基本的组成单位，每个正则表达式中至少要包含一个原子，常见的原子有以下几类：  
	* 普通字符作为原子     
	* 非打印字符作为原子  
	* 通用字符作为原子  
	* 院子表  
    1. 普通字符作原子的情况。普通字符比如数字、大小写字母、下划线等都可以作为原子使用。比如如下程序中，我们就使用了“yue"作为原子使用，分别是'y''u''e'。
```
import re
pattern="yue"
string="http://yum.iqianyue.com"  
result1=re.search(pattern,string)
print(result1)
```  
  2. 非打印字符作为原子  
  	 所谓的非打印字符，指的是一些在字符串中用于格式控制的符号，比如换行符等。
```
	import re
	pattern="\n"
    string="http://yum.iqianyue.com
http://baidu.com"
result1=re.search(pattern.string)
print(result1)
```
	3. 通用字符作为原子   
所谓通用字符，即一个原子可以匹配一类字符，我们在实际项目中经常会用到这一类原子。    
| 符号 | 含义|
| \w | 匹配任意一个字母、数字或下划线 |  
| \W | 匹配除了字母、数字和下划线以外的任意字符|  
| \d | 匹配任意一个十进制数|  
| \D | 匹配除了十进制数以外的任意一个其他字符 |  
| \s | 匹配任意一个空白字符  |  
| \S | 匹配除空白字符以外的任意一个其他字符 |    	 
```
import re
pattern="\w\dpython\w"
string="abcdfphp345python_py"
result1=re.search(pattern,string)
print(result1)
```
	4. 原子表   
定义一组地位平等的原子，然后匹配的时候会取该原子表中的任意一个原子匹配，在Python中，原子表有[]表示比如[xyz]就是一个原子表，这个原子表中定义了3个原子，这3个原子的地位平等，如，我们定义的正则表达式为“[xyz]py”,对应的原字符串是"xpython",如果此时使用re.search()函数进行匹配，就可以匹配出结果"xpy",因为此时只要py前一位是x y z字母中的其中一个字母，就可以匹配成功。  
类似的，[^]代表的是除了中括号里面的原子均可以匹配，比如"[^xyz]py"能匹配"apy",但是却不能匹配"xpy"等。  
```
import re 
pattern1="\w\dpython[xyz]\w"
pattern2="\w\dpython[^xyz]\w"
pattern3="\w\dpython[xyz]\W"
string="abcdfphp345pythony_py"
result1=re.search(pattern1,string)
result2=re.search(pattern2,string)
result3=re.search(pattern3,string)
print(result1)
print(result2)
print(result3)
```  
2. 元字符  
所谓元字符，就是正则表达式中具有一些特殊含义的字符，比如重复N次前面的字符等。 
| 符号 | 含义 |  
| . | 匹配除换行符以外的任意字符 |  
| ^ | 匹配字符串的开始位置 |  
| $ | 匹配字符串的结束位置 |  
| * | 匹配0次、1次或多次前面的原子 |  
| ? | 匹配0次或1次前面的院子 |  
| + | 匹配1次或多次前面的原子 |  
| {n} | 前面的院子恰好出现n次 |  
| {n,} | 前面的原子至少出现n次 |  
| {n,m} | 前面的原子至少出现n次，至多出现m次 |  
| '|' | 模式选择符 |  
| () | 模式单元符 |  
具体来说元字符可以分为：任意匹配元字符、边界限制元字符、限定符、模式选择符、模式单元等。 
