Real World Haskell notes
======================
##类型
基本内置类型
```haskell
count = 10::Int -- Int
height = 10::Double -- double 
isStatic = True::Bool -- Bool
bigInt = 100000000000000000::Integer --Unbound integer
letterX = 'X'::Char -- Char 
```
strong static type, 类型是编译时确定的（手动签名或者自动推导）
```haskell
--myLength :: [t] -> Int --参数为List，返回值Int类型, 可以忽略该类型签名

myLength [] = 0 --空列表 模式匹配
myLength (h:hs) = (myLength hs) + 1 
--编译器推导类型 :type myLength
--myLength :: Num a => [t] -> a
```
##变量
变量是一个表达式的名字，同一个表达式可以有多个名字，但是一个名字不能对应多个表达式，也就是说haskell变量是immutable的，是不可变的。试图给已有的变量重新赋值会有编译错误
```haskell
x = 10
x = 11 --compile error
```
```haskell
{- 多行注释
	::类型 和类型签名
	变量声明时类型签名可以忽略，自动推导
-}
--注释
x = 10::Int -- x is 10
y = 10::Double -- y is 10.0
```
惰性求值，真正用到表达式的值时，表达式才会求值

条件求值
```haskell
if expr0
then expr1 --缩进要和if一致
else expr2 --缩进要和if一致
```
很多时候条件求值可以使用模式匹配替代
```haskell
-- file: ch02/myDrop.hs
myDrop n xs = if n <= 0 || null xs
              then xs --haskell的缩进和python一样是语法的一部分
              else myDrop (n - 1) (tail xs)
--version 2
myDrop1 0 xs = xs
myDrop1 n [] = []
myDrop1 n xs = myDrop1 (n-1) (tail xs)
```
##函数
数学意义上讲haskell只存在一个参数的函数，
N个参数函数实际上是复合函数 g(x,y) = (f(x))(y)

`->` 是将一个值映射到另一个值的过程

`->` 是一个右结合的

```haskell
{-
	函数定义
	函数名 :: 类型 -> 类型 -> 类型 ...-> 返回类型
	函数名 参数1 参数2 参数3 ... = 函数体
-}
myTake :: Int -> ([a] -> [a])
myTake 0 (x:xs) = []
myTake n [] = []
myTake n (x:xs) = x:(myTake (n-1) xs)

t = myTake 1 [1,3,4]
-- t [1]
{-
分解后的伪代码:
myTake 1 生成临时函数
g :: [a] -> [a]
t = g([1,3,4])
-}
f x y z = x + y + z
{-
	+ - * / 等都是函数
-}
x = 10 + 12
y = (+) 10 12 -- 10和12是参数
```

##自定义类型
`data 类型名 = 类型值构造函数 组成值类型列表`
`类型值构造函数` 返回一个对应类型的值
```haskell
-- BookInfo 包含：Id 书名 作者
data BookInfo = Book Int String [String]
                deriving (Show)
```