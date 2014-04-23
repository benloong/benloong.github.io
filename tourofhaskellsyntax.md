Haskell语法笔记
===============

##函数
_变量_和_函数_实际上都是一个表达式的名字

名字定义遵循下面的格式

**name pattern1 pattern2 ... patternN = expression (N>=0)**

```haskell
pi = 3.1415926 --N = 0
nextChar c = chr (ord c + 1) --N = 1
sum x y z = x+y+z --N = 3
```
##模式绑定
**pattern = expression**
```haskell
(xs, ys) = unzip listOfPairs
(x:y:z:rest) = [1..] -- x 1, y 2, z 3,
```

##类型签名
最好定义前加类型签名

格式：**nameOfFunction :: _type1_ -> _type2_ ... -> _typeN_  (N > 1)**

泛型
**name :: _type_**

##Guards
条件匹配

##模式匹配

##表达式

##列表推导

##Lambda表达式
**\pattern1 pattern2 ... patternN -> expression (N>=1)**

##名字

名字可包含字母，数字，符号`'`和`_`

函数名，参数，变量，类型变量必须以小写字母或者`_`开头

类型名和构造名必须以大写字母开头

有效名字:
    `x foldl' long_name unzip3 concatMap _yada `
    `Bool Cons TreeNode Int `

##注释

```haskell
{-多行注释
-}

pi = 3.1415926--单行注释
```
##引号
| 引号        | 描述    |
|-------------|---------|
| 'a'           | Char类型的值       |
| "hello"       | String 类型的值    |
| \`mod\`       | 函数当做操作符  12 \`mod\` 5 |
| foldl'        | 函数名的一部分       |