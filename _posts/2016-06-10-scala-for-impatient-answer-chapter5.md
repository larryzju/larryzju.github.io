---
title: 《快学scala》 题解（第5章）
layout: post
tag: 快学scala题解
---

1. 修改后的 Counter 类如下

   ```scala
   class Counter {
     private var value = 0
	 
	 def increment() {
	   if ( value == Int.MaxValue ) {
	     value = 0
	   } else {
	     value += 1
	   }
     }
	 
	 def current() = value
   }
   ```
   
2. BankAccount 类如下

   ```scala
   class BankAccount( private var _balance: Double ) {
   
     def balance = _balance
	 
	 def deposit( d: Double ) {
	   _balance += d
     }
	 
	 def withdraw( d: Double ): Double = {
	   val rd = d.min( _balance )
	   _balance -= rd
	   rd
     }
	 
   }
   ```
   
3. 类实现如下

   ```scala
   class Time( val hours: Int, val minutes: Int ) {
     
	 def before( other: Time ): Boolean = {
	   ( hours < other.hours ) ||
	   (( hours == other.hours ) && ( minutes < other.minutes ))
     }
   }
   ```
   
4. 修改后的类如下

   ```scala
   class Time( val hours: Int, val minutes: Int ) {
   
     def this( minutes: Int ) {
	   this( minutes / 60, minutes % 60 )
     }
     
	 def before( other: Time ): Boolean = {
	   ( hours < other.hours ) ||
	   (( hours == other.hours ) && ( minutes < other.minutes ))
     }
   }
   ```
   
5. 代码如下，其中 `name` 为 `JavaBeans` 属性，`id` 为正常 scala 属性

   ```scala
   import scala.reflect.BeanProperty
   
   class Student {
     @BeanProperty var name: String = "";
	 var id: Long = 0
   }
   ```
   
   生成的 Student 类中生成的方法有
   
   ```
   private java.lang.String name;
   private long id;
   public java.lang.String name();
   public void name_$eq(java.lang.String);
   public void setName( java.lang.String );
   public long id();
   public void id_$eq(long0;
   public java.lang.String getName();
   public Student();
   ```
   
   scala 中调用 JavaBeans 方法如下
   
   ```scala
   val s = new Student
   s.setName( "larry" )
   println( s.getName )
   ```
   
6. 类代码如下

   ```scala
   class Person( _age: Int ) {
     var age = _age.max(0)
   }
   ```
   
7. 类代码如下

   ```scala
   class Person( name: String ) {
     val firstName = name.split( " " )(0)
	 val  lastName = name.split( " " )(1)
   }
   ```
   
   主构造器参数是普通参数，因为没有对该参数的访问需要
   
8. 主构造器使用

   ```scala
   class Car( val manufacturer: String, val genere: String,
              val year: Int = -1,
			  var plate: String = "" ) {
   }
   ```
   
   使用默认参数可以避免过多的辅助构造器
   
9. 如果用 Java 来实现上面的类，代码如下（比 scala 复杂的多）

   ```java
   public class Car {
   
       private String manufacturer;
	   private String genere;
	   private int    year;
	   public  String plate;
	   
	   public int year() {
	       return year;
	   }
	   
	   public String genere() {
	       return genere;
	   }
	   
	   public String manufacturer() {
	       return manufacturer;
	   }
	   
	   public Car( String m, String g, int y, String p ) {
	       this.manufacturer = m;
		   this.genere       = g;
		   this.year         = y;
		   this.plate        = p;
	   }
	   
	   public Car( String manufacturer, String genere, int year ) {
	       this( manufacturer, genere, year, "" );
	   }
	   
	   public Car( String manufacturer, String genere, String plate ) {
	       this( manufacturer, genere, -1, plate );
	   }
	   
	   public Car( String manufacturer, String genere ) [
	       this( manufacturer, genere, -1 );
	   }
	   
   }
   ```
   
10. 使用缺省参数，代码更少，同时更强调了默认的参数。代码如下

    ```scala
    class Employee( val name: String = "John Q. Public",
	                var salary: Double = 0.0 ) {
    }
    ```
