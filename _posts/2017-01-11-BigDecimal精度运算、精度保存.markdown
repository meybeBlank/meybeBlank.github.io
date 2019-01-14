---
layout: post
title: BigDecimal精度运算、精度保存
date: 2017-01-11 18:00:00.000000000 +09:00
tags: BigDecimal
---

## 初识情形

	从服务器获取double类型的数值，并转换为百分比展示，
	再将String通过Double.valueOf(s)转换成double类型时出现经度丢失。

## 简单介绍
	Java在java.math包中提供的API类BigDecimal，用来对超过16位有效位的数进行精确的运算。
	
	 - 在实际应用中，需要对更大或者更小的数进行运算和处理。
	
	 - float和double只能用来做科学计算或者是工程计算。
	
	 -  在商业计算中若需要精确的计算结果则要用BigDecimal类。

## 构造方法


	 -  BigDecimal(int)           创建一个具有参数所指定整数值的对象。
	
	 -  BigDecimal(double)    创建一个具有参数所指定双精度值的对象。
	
	 -  BigDecimal(long)        创建一个具有参数所指定长整数值的对象。
	
	 -  BigDecimal(String)      创建一个具有参数所指定以字符串表示的数值的对象。
	
	 -  BigDecimal(char[])      创建一个具有参数所指定以字符数组表示的数值的对象。
	
	 -  BigDecimal(xxx， MathContext)      创建一个根据设置精度处理后具有参数所指表示的数值的对象。例如：

```
	/**
     * 对小数进行精度控制
     * @param s  小数对应String
     * @param precisiion  保留精度 包换整数位所有的位数
     *                    即 1.2222 和 11.2222 同为 3
     *                    分别得到 1.22 和 11.2
     * @return
     */
    public BigDecimal testScale(String s, int precisiion){
        return new BigDecimal(s, new MathContext(precisiion));
    }
```

```
	// b1 输出：12.2  
	// b2 输出：2.15
	BigDecimal b1 = bigDecimalTest.testScale("12.1526", 3);
	BigDecimal b2 = bigDecimalTest.testScale("2.1526", 3);
```

### 注意事项
	- 通常建议优先使用String构造方法。
	
	- 将int、double、float通过toString方法转换之后创建BigDecimal。

## 常用方法
 - add(BigDecimal)            加

 - subtract(BigDecimal)     减

 - multiply(BigDecimal)     乘
```
	/**
     * 乘法测试
     * @param a 乘数 a
     * @param b 乘数 b
     * @param precisiion 精度 为0默认精度
     */
    public BigDecimal testMultiply(String a, String b, int precisiion){
        BigDecimal ba = new BigDecimal(a);
        BigDecimal bb = new BigDecimal(b);
        return precisiion == 0 ? ba.multiply(bb) : ba.multiply(bb,new MathContext(precisiion));
    }
```

 - divide(BigDecimal)        除   **当出现1/3无限循环等结果必须设置精度，否则崩溃**




```
    /**
     * 除法测试
     * @param a 除数 a
     * @param b 被除数 b
     * @param precisiion 精度 为0默认精度
     */
    public BigDecimal testDivide(String a, String b, int precisiion){
        BigDecimal ba = new BigDecimal(a);
        BigDecimal bb = new BigDecimal(b);
        return precisiion == 0 ? ba.divide(bb) : ba.divide(bb,new MathContext(precisiion));
    }
```

 -  toString()                       换成字符串

 -  doubleValue()                转换双精度数  xxxValue类似使用

## 指定精度的舍入使用divide方法
**divisor**  value by which this is divided.
**scale**  the scale of the result returned.
**roundingMode**  rounding mode to be used to round the result.
divide (BigDecimal divisor, int scale, int roundingMode)；