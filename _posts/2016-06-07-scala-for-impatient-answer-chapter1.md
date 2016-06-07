---
title: 《快学scala》 题解（第1章）
layout: post
categories: scala
tag: 快学scala题解
---


1. 键入 `3.`，按 tab 出现

   ```
   !=   >=             floatValue      isValidInt     synchronized     toString     
   ##   >>             floor           isValidLong    to               unary_+      
   %    >>>            formatted       isValidShort   toBinaryString   unary_-      
   &    ^              getClass        isWhole        toByte           unary_~      
   *    abs            hashCode        longValue      toChar           underlying   
   +    asInstanceOf   intValue        max            toDegrees        until        
   -    byteValue      isInfinite      min            toDouble         wait         
   ->   ceil           isInfinity      ne             toFloat          |            
   /    compare        isInstanceOf    notify         toHexString      →            
   <    compareTo      isNaN           notifyAll      toInt                         
   <<   doubleValue    isNegInfinity   round          toLong                        
   <=   ensuring       isPosInfinity   self           toOctalString                 
   ==   eq             isValidByte     shortValue     toRadians                     
   >    equals         isValidChar     signum         toShort      
   ```
   
2. 交互过程如下

   ```scala
   import scala.math._
   sqrt(3)
   res0*res0 - 3
   // => res1: Double = -4.440892098500626E-16
   ```
   
3. res0 是 val，不可修改
4. 返回 "crazycrazycrazy"，`StringOps.*(n:Int): String` 返回n个字符串拼接结果
5. `max` 在 `RichInt` 中定义，相当于 `10.max(2)`
6. `BigInt(2) pow 1000`，其中 `pow` 是 BigInt 对象的方法
7. 需要引入 BigInt 单例对象和 Random 类

   ```scala
   import scala.math.BigInt._
   import scala.util.Random
   ```
   
8. 需要用到 `BigInt.toString( radix: Int )` 方法，如下

   ```scala
   BigInt(scala.util.Random.nextInt).toString(36)
   ```
   
9. 使用 `StringOps.head` 和 `StringOps.last`，如下

   ```scala
   val a = "Hello, World"
   a.head
   a.last
   ```
   
10. 分别用于取前 N 位、去掉前 N 位、取后 N 位、去掉后 N 位。  
`substring`需要用绝对偏移量取子字符串，对于变长组合字符串计算可能略复杂；特别在末尾进行操作时更需要与 `length` 配合使用

