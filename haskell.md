haskell learning notes
======================
##类型
函数类型可以手动签名或者自动推导
```haskell
--myLength :: [t] -> Int --参数为List，返回值Int类型, 可以忽略该类型签名

myLength [] = 0 --空列表 模式匹配
myLength (h:hs) = (myLength hs) + 1 
--编译器推导类型 :type myLength
--myLength :: Num a => [t] -> a
```
##variable

```haskell
{- 多行注释
	::类型 和类型签名
	变量声明时类型签名可以忽略，自动推导类型
-}
--行注释
x = 10::Int -- x is 10
y = 10::Double -- y is 10.0
```
##function
```haskell
{-
	函数定义
	函数名 :: 类型 -> 类型 -> 类型 ...-> 返回类型
	函数名 参数1 参数2 参数3 ... = 函数体
-}
	f x y z = x + y + z
{-
	+ - * / 等都是函数
-}
	x = 10 + 12
	y = (+) 10 12 -- 10和12是参数
```