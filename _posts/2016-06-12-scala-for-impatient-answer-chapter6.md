---
title: 《快学scala》 题解（第6章）
layout: post
tags: 计算机
---

1. 定义的对象如下

   ```scala
   object Conversions {

	 def inchesToCentimeters( inch: Double ): Double = {
	   inch * 2.54
	 }

	 def gallonsToLiters( gallon: Double ): Double = {
	   gallon * 3.78541178
	 }

	 def milesToKilometers( mile: Double ): Double = {
	   mile * 1.609344
	 }

   }
   ```


2. 使用扩展方式重新实现的类/对象代码如下

   ```scala
   class UnitConversion( base: Double ) {
	 def apply( from: Double ): Double = {
	   from * base
	 }
   }

   object InchesToCentimeters extends UnitConversion( 2.54 )
   object GallonsToLiters extends UnitConversion(3.78541178)
   object MilesToKilometers extends UnitConversion(1.609344)
   ```


3. 扩展对象代码为

   ```scala
   object Origin extends java.awt.Point()
   ```
   java.awt.Point 类不是一个抽象类或Interface，其中包含了 x,y 变量和相关操作，对象内容是可变的
  
  
4. 定义的 Point 类及其伴生类如下

   ```scala
   class Point( var x:Double = 0, var y: Double = 0 ) {
	 override def toString(): String = {
	   "(%f,%f)".format( x, y )
	 }
   }

   object Point {
	 def apply( x: Double, y: Double ) = {
	   new Point( x, y )
	 }
   }
   ```

5. 应用程序如下

   ```scala
   object Reverse extends App {
	 println(args.reverse.mkString(" "))
   }
   ```

6. 定义的枚举如下

   ```scala
   object PlayCard extends Enumeration {
	 type Suit = Value
	 val Heart   = Value( "♥" )
	 val Diamond = Value( "♦" )
	 val Spade   = Value( "♠" )
	 val Club    = Value( "♣" )
   }
   ```

7. 同上，对象内新增函数 `isRed`

   ```scala
   class PlayCard( val suit: PlayCard.Suit.Suit, val value: Char ) {

	 override def toString() = {
	   "%s%s".format( suit, value )
	 }

   }

   object PlayCard {

	 object Suit extends Enumeration {
	   type Suit = Value
	   val Heart   = Value( "♥" )
	   val Diamond = Value( "♦" )
	   val Spade   = Value( "♠" )
	   val Club    = Value( "♣" )
	 }

	 import PlayCard._

	 def isRed( c: PlayCard ): Boolean = {
	   c.suit == Suit.Heart || c.suit == Suit.Diamond
	 }

   }
   ```

8. 代码如下

   ```scala
   object Color extends Enumeration {
	 type Color = Value
	 val  black = Value( 0x000000 )  
	 val  red   = Value( 0xff0000 )
	 val  green = Value( 0x00ff00 )
	 val  blue  = Value( 0x0000ff )
	 val  white = Value( 0xffffff )
	 val  yellow= Value( 0xffff00 )
	 val  cyan  = Value( 0x00ffff )
	 val  magenta = Value( 0xff00ff )
   }
   ```
