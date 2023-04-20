# Java Type Conversion



<br>



## 1. Char to Int

```java
// Java char to int Example: Character.getNumericValue()
char c = '1';
int a = Character.getNumericValue(c);  

// Java char to int Example: String.valueOf()
char c='1';  
int a = Integer.parseInt(String.valueOf(c));  
```



## 1-1. Int to Char

```java
int a = 65;
char c = (char) a;
System.out.println(c);		// 'A'

int a1 = 1;
char c = (char) a1;
System.out.println(c);   // 1이 아닌 문자가 출력됨

/*
If you add '0' with int variable, it will return actual value in the char variable. The ASCII value of '0' is 48. So, if you add 1 with 48, it becomes 49 which is equal to 1. The ASCII character of 49 is 1. */
int a = 1;
char c=(char)(a+'0');    
System.out.println(c);    // 1


int a1 = 5;  
int a2 = 6;  
char ch1 = Character.forDigit(a1, 10);  
char ch2 = Character.forDigit(a2, 16);  
String str1 = "The char representation of " + a1 + " in radix 10 is " + ch1; 
String str2 = "The char representation  of " + a2 + " in radix 16 is " + ch2; 
System.out.println(str1);	// The char representation of 5 in radix 10 is 5
System.out.println(str2); // The char representation of 6 in radix 16 is 6
```

