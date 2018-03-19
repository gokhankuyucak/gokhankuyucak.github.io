---
title: "Bitwise Operations in OSB and Oracle SOA"
excerpt: "Both OSB and Oracle SOA does not support bitwise operations natively. You need to handle custome solutions to work with Bitwise operations."
last_modified_at: 2018-03-14T22:45:06-05:00

tags: 
  - OSB
  - SOA
  - Bitwise
toc: false
wide: true
---
Sometimes you need to manipulate integers on bit level. Both Oracle Service Bus and Oracle SOA does not have any native solution for bitwise operations. You need to create a custom jar in OSB or you need to use java embedding in Oracle SOA bpel. 

You can download custom jar file for bitwise operations to use in OSB proxy service. The class file has 7 different methods:

**BitwiseAND:** Bitwise AND operator compares corresponding bits of two operands. If both bits are 1, it gives 1. If either of the bits is not 1, it gives 0. It's denoted by &.
```java
public static int BitwiseAND(int param1, int param2) {
		int result= param1 & param2;
		return result;
}
```

**BitwiseOR:** Bitwise OR operator compares corresponding bits of two operands. If either of the bits is 1, it gives 1. If not, it gives 0. It's denoted by |.
```java
public static int BitwiseOR(int param1, int param2) {
		int result= param1 | param2;
		return result;
}
```


**BitwiseComplement:** Bitwise complement operator means bit negation. It takes every single bit of the number and flips its value. That is - 0 becomes 1 and vice versa. It is denoted by ~.
```java
public static int BitwiseComplement(int param) {
		int result= ~param;
		return result;
}
```

**BitwiseXOR:** Bitwise XOR operator compares corresponding bits of two operands. If corresponding bits are different, it gives 1. If corresponding bits are equal, it gives 0. It's denoted by ^. 
```java
public static int BitwiseXOR(int param1, int param2) {
		int result= param1 ^ param2;
		return result;
}
```


**SignedLeftShift:** This operator shifts a bit pattern to the left by certain number of specified bits, and zero bits are shifted into the low-order positions. It's denoted by <<. 
```java
public static int SignedLeftShift(int param1, int param2) {
		int result= param1 << param2;
		return result;
}
```


**SignedRightShift:** This operator shifts a bit pattern to the right by certain number of specified bits. It's denoted by >>.
```java
public static int SignedRightShift(int param1, int param2) {
		int result= param1 >> param2;
		return result;
}
```


**UnsignedRightShift:** This operator shifts all the bits to the right and pads the result with zeros from the left. It's denoted by >>>.Signed and unsigned right shifts have the same result for positive numbers.
```java
public static int UnsignedRightShift(int param1, int param2) {
		int result= param1 >>> param2;
		return result;
}
```


<figure><img src="/assets/images/Jar-Bitwise-Operations-OSB.png">
	<figcaption>Bitwise Operations in OSB - Custom Jar Solution</figcaption>
</figure>



To handle Bitwise Operations in Oracle SOA Suite, you can use java embedding.
<figure><img src="/assets/images/Jar-Bitwise-Operations-SOA.png">
	<figcaption>Bitwise Operations in Bpel - Java Embedding Solution</figcaption>
</figure>
