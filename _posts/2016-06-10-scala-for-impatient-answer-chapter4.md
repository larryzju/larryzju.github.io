---
title: 《快学scala》 题解（第4章）
layout: post
tags: 计算机
---

1. 代码如下

   ```scala
   val equipment_i_want = Map(
     "Kindle Oasis" -> 2499,
	 "Macbook Pro"  -> 12800,
	 "violin"       -> 3200
   )
   
   val discount_equipment = for ((k,v) <- equipment_i_want) yield(k,v*.9)
   println( discount_equipment )
   ```
   
2. 代码如下

   ```scala
   val in = new java.util.Scanner( new java.io.File( "/tmp/pep-3136.txt" ) )
   val s  = new scala.collection.mutable.HashMap[String,Int]
   while( in.hasNext() ) {
     val word = in.next()
	 s.update( word, s.getOrElse( word, 0 ) + 1 )
   }
   println(s)
   ```
   
3. 代码如下

   ```scala
   val in = new java.util.Scanner( new java.io.File( "/tmp/pep-3136.txt" ) )
   val s  = new scala.collection.immutable.HashMap[String,Int]
   while( in.hasNext ) {
     val word = in.next
	 s = s + ((word, s.getOrElse( word, 0 ) + 1))
   }
   println(s)
   ```
   
4. 同上，将 `HashMap` 替换为 `TreeMap`

   ```scala
   val in = new java.util.Scanner( new java.io.File( "/tmp/pep-3136.txt" ) )
   val s  = new scala.collection.immutable.TreeMap[String,Int]
   while( in.hasNext ) {
     val word = in.next
	 s = s + ((word, s.getOrElse( word, 0 ) + 1))
   }
   println(s)
   ```

5. 如下，scala 中没有可变的 `TreeMap`，无法保证其顺序

   ```scala
   import scala.collection.JavaConversions._
   
   val in = new java.util.Scanner( new java.io.File( "/tmp/pep-3136.txt" ) )
   val s: scala.collection.mutable.Map[String,Int]  = new java.util.TreeMap[String,Int]
   while( in.hasNext() ) {
     val word = in.next()
	 s.update( word, s.getOrElse( word, 0 ) + 1 )
   }
   println(s)
   ```

6. 代码如下

   ```scala
   val h = new scala.collection.mutable.LinkedHashMap[String,Int]
   h += ("Monday" -> java.util.Calendar.MONDAY);   println( h )
   h += ("Tuseday"-> java.util.Calendar.TUESDAY);  pritnln( h )
   h += ("Sunday" -> java.util.Calendar.SUNDAY);   println( h )
   ```
   
7. 代码如下

   ```scala
   val props = System.getProperties
   val fmt = "%%-%ds | %%s\n".format( props.map( _._1.length ).max + 3 )
   for ( (k,v) <- props ) { printf( fmt,k,v) }
   ```
   
8. 函数如下

   ```scala
   def minmax( values: Array[Int] ): Tuple2[Int,Int] = {
     (values.min, values.max)
   }
   ```
   
9. 函数如下

   ```scala
   def lteqgt( values: Array[Int], v: Int ): Tuple3[ Int, Int, Int ] = {
     val lt = values.filter( _ < v ).size
	 val eq = values.filter( _ == v ).size
	 ( lt, eq, values.size - lt - eq )
   }
   ```
   
10. 将会生成 `((H,W), (e,o), (l,r,), (l,l), (o,d))` 的 `Vector` 对象。`zip` 的作用是拼接两个序列中的每对元素（按最小长度），形成一个 Pair 的列表
