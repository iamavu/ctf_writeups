# misc

## **No flag for you**
> Solved By : Taz

* We found out that `ls`, `cat`, `echo` commands were available.
* So started looking around for these.
* Found a way to list dir using echo:

```
echo /*
```

![](https://i.imgur.com/nzyMsSw.png)

* started looking around for the flag and found it in the `/home/user/run/opt` dir

```
echo /home/user/run/opt/*
```

* Did some research on ways to read a file using echo, and got something.
* Reference link: https://stackoverflow.com/questions/22377792/how-to-use-echo-command-to-print-out-content-of-a-text-file

```
echo "$(</home/user/run/opt/flag-b01d7291b94feefa35e6.txt)"
```

![](https://i.imgur.com/pPB49gk.png)

---

## Alternative Arithmetic 
> Solved By : Ava and choco

* After connection we are given a java questions which we have to research for and solve

```
Question 1 :

1. Find a nonzero long x` such that `x == -x`, enter numbers only, no semicolons
```

* answer for 1st question is `-9223372036854775808`, this is the smallest long value which is actually positive

```
Question 2 :

Find 2 different `long` variables `x` and `y`, differing by at most 10, such that `Long.hashCode(x) == Long.hashCode(y)`
```

* answer for 2nd question is `x = -1 & y = 0`

```
Question 3 - 

Enter a `float` value `f` that makes the following function return true
```

```java
// code given along with Q3 :

boolean isLucky(float magic) {
    int iter = 0;
    for (float start = magic; start < (magic + 256); start++) {
        if ((iter++) > 2048) {
            return true;
        }
    }
    return false;
```

* answer for 3rd Question is `2.14E9`

---

## Alternative Arithmetic (Final Flag)
> Solved By : nigamelastic and choco

* I will use the following to run [this](https://www.onlinegdb.com/online_java_compiler) java compiler to run my code online.

* on connecting we see the first question which is:

```
4th question :

Enter 3 `String` values `s1`, `s2`, and `s3` such that:
```
```java
new BigDecimal(s1).add(new BigDecimal(s2)).compareTo(new BigDecimal(s3)) == 0
```
but
```java
Double.parseDouble(s1) + Double.parseDouble(s2) != Double.parseDouble(s3)
```
```
Do not enter quotation marks.
```

* answer 4 worked but very stupidly: i will paste my inputs and outputs and you just see :

**First attempt**
```bash
Do not enter quotation marks.
s1 = 0.004
s2 = -0.003
s3 = 0.001
Sorry, your answer is not correct. Try something else next time.
```
**Second attempt**
```bash
s1 = 20000000000000000000000000000000000000000000000.0000000000000000000000000000000000000000004
s2 = -10000000000000000000000000000000000000000000000.0000000000000000000000000000000000000000003
s3 = 10000000000000000000000000000000000000000000000.0000000000000000000000000000000000000000001
Sorry, your answer is not correct. Try something else next time.
```

**Third attempt**
```bash
s1 = 0.04
s2 = -0.03
s3 = 0.01
Perfect! You are correct.
```

* credits to choco, who guided me, and after our discussion we realised for some reason `0.04` is working but `0.004` isn’t
choco also pointed out this amazing link https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html

* finally we discussed and realized the following :

* according to double :
```bash
0.003 will be represented as 3.00000000000000006245004513517E-3
0.004 will be represented as 4.00000000000000008326672684689E-3
0.02 will be represented as 2.00000000000000004163336342344E-2

but 

0.03 will be represented 2.99999999999999988897769753748E-2 
```

* after this stupidity we finally get the 5th and final option:

```
5. Final question! [🎶BOSS MUSIC🎶]

Fill in <type>, <num1>, <num2> below:
```
```java
var i = (<type>) <num1>; var j = (<type>) <num2>;
```
```
such that after running the code above, the following expression:
```
```java
i < j || i == j || i > j
```
```
evaluates to `false`.

<num1> and <num2> are Java code that satisfies this regex: [0-9]*\.?[0-9]*
```

* which is very easy , I remember it from my college days
* its `Integer 128`
* this is due to the integer cache limits
* the following link would be a better explanation
* https://www.programmersought.com/article/91462031238/

