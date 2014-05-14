Real World Haskell notes
======================
##命名
* 名字可包含字母，数字，符号`'`和`_`
* 函数名，参数，变量，类型变量必须以小写字母或者`_`开头
* 类型名和构造名必须以大写字母开头

有效名字:
    `x foldl' long_name unzip3 concatMap _yada `
    `Bool Cons TreeNode Int `
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
fn = if expr0
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
haskell函数名必须以小写字母开头
类型名必须以大写字母开头
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
特殊函数

列表构造操作符`:`
`(:) :: a -> [a] -> [a]`
##自定义类型

`data 类型名 = 类型值构造函数 组成值类型列表`
`类型值构造函数` 返回一个对应类型的值

`代数数据类型`
不需要任何语言特性就可以实现c++ struct,union,enum这些功能
```haskell
data JValue = JNull
            |JBool Bool
            |JInt Int
            |JDouble Double
            |JString String
            |JObject [(String, JValue)]
            |JArray [JValue]
            deriving (Show)


-- BookInfo 包含：Id 书名 作者
data BookInfo = Book Int String [String]
                deriving (Show)
```

`类型别名`
`type` 类似c++ `typedef`
```haskell
type BookID = Int
type Writer = [String]
type BookName = String
```

###local variable

#### let in

```haskell
lend amount balance = let reserve    = 100
                          newBalance = balance - amount
                      in if balance < reserve
                         then Nothing
                         else Just newBalance
```
`let`代码块定义表达式名字，对应的`in`代码块里面使用这些表达式

有点类似c++ Local Scope的东东，`let` 定义的变量只能在 对应的`in`代码块里使用

`let` `in`可以嵌套，和c++类似最里层的变量名被优先使用
```haskell
bar = let x = 1
      in ((let x = "foo" in x), x)
--ghci> bar 
--("foo",1)
```

####where 约束

与`let`声明变量在前不同的是 `where`只能放在在函数尾部用于定义变量或者函数模式匹配，与SQL有相似性

```haskell
lend2 amount balance = if amount < reserve * 0.5
                       then Just newBalance
                       else Nothing
    where reserve    = 100
          newBalance = balance - amount

pluralise :: String -> [Int] -> [String]
pluralise word counts = map plural counts
    where plural 0 = "no " ++ word ++ "s"
          plural 1 = "one " ++ word
          plural n = show n ++ " " ++ word ++ "s"
```

####缩进

同一个scope的缩进要保持一致

####case表达式

函数内模式匹配

`case` 变量 `of`

从上到下匹配表达式列表

表达式列表类型需一致

```haskell
fromMaybe defval wrapped =
    case wrapped of
      Nothing     -> defval
      Just value  -> value
```

####条件求值guard `|`

```haskell
nodesAreSame (Node a _ _) (Node b _ _)
    | a == b     = Just a
nodesAreSame _ _ = Nothing --模式匹配

lend3 amount balance
     | amount <= 0            = Nothing
     | amount > reserve * 0.5 = Nothing
     | otherwise              = Just newBalance
    where reserve    = 100
          newBalance = balance - amount

--naive版本
myDrop n xs = if n <=0 || null xs
              then xs
              else myDrop (n-1) (tail xs)
--函数模式匹配版本
myDrop1 0 xs      = xs
myDrop1 n []      = []
myDrop1 n (x:xs)  = myDrop1 (n-1) xs
--使用条件求值guard的版本
niceDrop n xs | n <= 0 = xs
niceDrop _ []          = []
niceDrop n (_:xs)      = niceDrop (n-1) xs
```

####尾递归 `CPS`
`CPS` `Continuation-passing style`

myLengthTail xs = myLengthTailHelp 0 xs

myLengthTailHelp n [] = n
myLengthTailHelp n (x:xs) = myLengthTailHelp (n + 1) xs