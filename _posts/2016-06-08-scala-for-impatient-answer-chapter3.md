---
title: 《快学scala》 题解（第3章）
layout: post
tags: 计算机
---

1. 代码如下

   ```scala
   def ans1( n: Int ): Array[Int] = {
     1.to(n).map{ x => scala.util.Random.nextInt(n) }.toArray
   }
   ```
   
2. 代码如下

   ```scala
   def ans2( a:Array[Int] ): Unit = {
     for ( i <- 0.until(a.length-1,2) ) {
	   val t = a(i)
	   a(i) = a(i+1)
	   a(i+1) = t
     }
   }
   ```

3. 代码如下

   ```scala
   def ans3( a: Array[Int] ): Array[Int] = {
     val swap_index = for( i <- 0.until(a.length) ) yield {
	   if ( i % 2 == 0 ) {
	     if ( i == a.length - 1 ) i else i + 1
	   }
     } else {
	   i - 1
     }
	 swap_index.map( i => a(i) ).toArray
   }
   ```
   
4. 代码如下

   ```scala
   def ans4( a:Array[Int] ): Array[Int] = {
     a.filter( _ > 0 ) ++ a.filter( _ < 0 )
   }
   ```
   
5. 代码如下

   ```scala
   def ans5( d: Array[Double] ): Double = {
     d.sum / d.length
   }
   ```
   
6. 在原位上修改

   ```scala
   def ans6_array( a: Array[Int] ): Unit = {
     for ( i <- 0.until(a.length/2) ) {
	   val t = a(i)
	   a(i) = a(a.length-i-1)
	   a(a.length-i-1) = t
     }
   }
   
   def ans6_arraybuffer( a: ArrayBuffer[Int] ): Unit = {
     a ++= a.reverse
	 a.remove( 0, a.length/2 )
   }
   ```
   
7. 可以使用 `Array.distinct: Array[T]` 方法去重

8. 重构使方法如下：

   ```scala
   def ans8( a: ArrayBuffer[Int] ): Unit = {
     var negative_index = for ( i <- 0 until a.length if a(i) < 0 ) yileld i
	 for ( i <- negative_index.tail.reverse ) {
	   a.remove(i)
     }
   }
   ```
   * 与第一种方式相比，从后往前地去除元素可以减少移动数量，从而性能较优
   * 与第二种方式相比，由于移除元素与长度线性相关，方式二所移除的元素都在末尾，因此性能较本方法优
   
9. 代码如下

   ```scala
   for ( zone <- java.util.TimeZone.getAvailableIDs() 
         if zone.startsWith( "America/" ) )
     yield zone.stripPrefix( "America/" )
   ```
   
10. 代码示意如下

    ```scala
	import java.awt.datatransfer._
	import scala.collection.JavaConversions.asScalaBuffer
	import scala.collection.mutable.Buffer
	
	val flavors = SystemFlavorMap.getDefaultsFlavorMap().asInstanceOf[SystemFlavorMap]
	val res: Buffer[String] = flavors.getNativesForFlavor( DataFlavor.imageFlavor )
	println( res )
	```
