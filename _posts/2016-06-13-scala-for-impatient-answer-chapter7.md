---
title: 《快学scala》 题解（第7章）
layout: post
categories: scala
tag: 快学scala题解
---

1. 两者的不同之处在于可见性的不同，例如第一种导入方式下 `Constant` 不可见

   ```scala
   package com {
	 package horstmann {
	   object Constant {
		 val pi = 3.1415926
	   }
	 }
   }


   package com.horstmann.impatient {
	 package test {
	   object Test extends App {
		 println( Constant.pi )
	   }
	 }
   }
   ```

   而第二种方式则是可见的

   ```scala
   package com {
	 package horstmann {
	   object Constant {
		 val pi = 3.1415926
	   }
	 }
   }


   package com {
	 package horstmann {
	   package impatient {
		 package test {
		   object Test extends App {
			 println( Constant.pi )
		   }
		 }
	   }
	 }
   }
   ```


2. 例如在代码中写入下面的代码，可能导致 hadoop 文件系统不可用

   ```scala
   package com {
	 package apache.hadoop.fs {
	   class FileSystem {
	   }
	 }
   }
   ```

3. 包 random 代码如下

   ```scala
   package object random {

	 private val a = 1664525
	 private val b = 1013904223
	 private val n = 32
	 private var seed: Int = 0

	 private def next() {
	   seed = ((1L * seed * a + b) % ( 1L << n )).toInt
	 }

	 def nextInt(): Int = {
	   next()
	   seed
	 }

	 def nextDouble(): Double = {
	   next()
	   1.0 * seed / (1L<<n)
	 }

	 def setSeed( seed: Int ): Unit = {
	   this.seed = seed
	 }
   }
   ```

4. 提供 `package object` 语法可能是为了约束函数和变量定义，防止在顶级定义裸变量

5. `private[com] def giveRaise(rate:Double)` 约束定义只能在 `com` 包内部可见

6. 代码如下

   ```scala
   import java.util.{HashMap => JavaHashMap}
   import scala.collection.mutable.{HashMap => ScalaHashMap}

   object Chapter7 extends App {
	 val jh = new JavaHashMap[String,String]( System.getenv )
	 val sh = new ScalaHashMap[String,String]()
	 for ( e <- jh.entrySet.toArray ) {
	   val a = e.asInstanceOf[java.util.Map.Entry[String,String]]
	   sh(a.getKey) = a.getValue
	 }
	 println( sh )
   }
   ```

7. 修改后的代码如下

   ```scala
   object Chapter7 extends App {

	 {
	   import java.util.{HashMap => JavaHashMap}
	   import scala.collection.mutable.{HashMap => ScalaHashMap}

	   val jh = new JavaHashMap[String,String]( System.getenv )
	   val sh = new ScalaHashMap[String,String]()
	   for ( e <- jh.entrySet.toArray ) {
		 val a = e.asInstanceOf[java.util.Map.Entry[String,String]]
		 sh(a.getKey) = a.getValue
	   }
	   println( sh )
	 }

   }
   ```

8. 将会把 `java` 和 `javax` 下所有的包内容都导入到当前包中，不是个好作法。因为 scala 中默认导入了 `scala._` 的所有内容，这样导入会覆盖 scala 默认导入的包，如 `scala.xml` 被 `javax.xml` 替换，导致可能的程序异常。

9. 代码如下

   ```scala
   import java.lang.System._

   object Chapter7 extends App {

	 val user = getProperty( "user.name" )
	 if ( readLine() == "secret" ) {
	   printf( "hello %s\n", user )
	 } else {
	   printf( "invalid password\n" )
	 }

   }
   ```

10. 被覆盖的如 Boolean, Byte, Double, Enum, Float, Long 等。

    > Java ClassLoader 由于动态特性，不能确定 JVM 中的 Class 情况，而是在收到有关类的请求后返回一个类或抛出异常，因此用默认的 ClassLoader 无法确切解决上面的问题。
	>
	> 需要通过自定义 ClassLoader 来解决上述问题，待完成 :flags: 

