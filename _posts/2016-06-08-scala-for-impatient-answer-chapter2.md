---
title: 《快学scala》 题解（第2章）
layout: post
tag: 快学scala题解
---

1. 函数定义如下

   ```scala
   def signum( v: Int ) : Int = {
	 if ( v > 0 ) {
	   1
	 } else if ( v < 0 ) {
	   -1
	 } else {
	   0
	 }
   }
   ```

2. `{}` 表示 Unit，类型为 void

3. 当 x 为 `{}` 时表达式 `x = y = 1` 是合法的

4. 代码如下
   
   ```scala
   for ( i <- 10 to (0,-1) ) {
     println(i)
   }
   ```

5. 函数定义如下

   ```scala
   def countdown( n: Int ) : Unit = {
     for ( i <- n to (0,-1) ) {
       println(i)
     }
   }
   ```

6. 代码如下

   ```scala
   var p = 1L
   for( c <- "Hello" ) {
	 p *= c
   }
   println( p )
   ```

7. 代码如下
   
   ```scala
   println( "Hello".map( _.toLong ).product )
   ```

8. 函数定义如下

   ```scala
   def product( s:String ): Long = {
     s.map( _.toLong ).product
   }
   ```

9. 递归函数定义如下（注意退出条件）

   ```scala
   def product_r( s: String ): Long = {
     if ( s.isEmpty ) {
       1
     } else {
       s.head.toLong * product_r( s.tail )
     }
   }
   ```

10. 递归如下

    ```scala
	def pow_x_n( x: Double, n: Int ): Double = {

	  if ( n == 0 ) { 
		1
	  } else if ( n < 0 ) {
		1.0 / pow_x_n( x, -n )
	  } else if ( n % 2 == 0 ) {
		var h = pow_x_n( x, n/2 )
		h * h
	  } else { 
		x * pow_x_n( x, n-1 )
	  }

	}
	```
