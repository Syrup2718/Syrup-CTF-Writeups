# Gatekeeper

🌀 [**Question Link**](https://learn.cylabacademy.org/library/731)
🔥 **Difficulty: Medium**


## Description
After you run the file, it will show that you should enter a number: `Enter a numeric code (must be > 999 ):`. 

## Analysis
It only needs to enter a number but if you type something else,  it will show the message below. So I guess we should find some special input that can satisfy the program's conditions.

```shell
syrup@DESKTOP-G1ALONG:~$ ./gatekeeper
Enter a numeric code (must be > 999 ): 1
Too small.
syrup@DESKTOP-G1ALONG:~$ ./gatekeeper
Enter a numeric code (must be > 999 ): 1000
Access Denied.
syrup@DESKTOP-G1ALONG:~$ ./gatekeeper
Enter a numeric code (must be > 999 ): 0
Too small.
syrup@DESKTOP-G1ALONG:~$ ./gatekeeper
Enter a numeric code (must be > 999 ): -1
Invalid input.
syrup@DESKTOP-G1ALONG:~$ ./gatekeeper
Enter a numeric code (must be > 999 ): 100
Too small.
syrup@DESKTOP-G1ALONG:~$ ./gatekeeper
Enter a numeric code (must be > 999 ): 999
Too small.
syrup@DESKTOP-G1ALONG:~$ ./gatekeeper
Enter a numeric code (must be > 999 ): 1000
Access Denied.
```

## Solution

### Looking in IDA
After reversing the file with IDA, we can go to see where the string `Enter a numeric code (must be > 999 ):` is shown and see that it saves our input as the variable `s`. We can also see there is a special function `reveal_flag()`. Looking into this function, we can see that it opens the file `flag.txt`, so we know we need to successfully execute this function to get the flag.

### What should we enter?
We can just see its logic. First, it saves our input as the variable `s` and calculates its length, saving it as the variable `v5`.

```c
__isoc99_scanf("%31s", s);
v5 = strlen(s);
```

Second, it will run the function `is_valid_decimal` which checks whether it is a decimal and makes `v4 = atoi(s);`. If it is not a decimal, it will run the function `is_valid_hex()` which checks whether it is hex and makes `v4 = strtol(s, 0LL, 16);`. 

Function Introduction
`strtol(const char *str, char **endptr, int base)`
> Convert `str` to an integer according to the given base.



```c
if ( (unsigned int)is_valid_decimal((__int64)s) )
{
v4 = atoi(s);
}
else
{
if ( !(unsigned int)is_valid_hex((__int64)s) )
{
    puts("Invalid input.");
    return 1;
}
v4 = strtol(s, 0LL, 16);
}
```

Third, it is an if statement. We need to make `v4 > 999 && v4 < 9999 && v5 == 3`, then we can get the flag.

```c
if ( v4 > 999 )
{
    if ( v4 <= 9999 )
    {
        if ( v5 == 3 )
        reveal_flag();
        else
        puts("Access Denied.");
    }
    else
    {
        puts("Too high.");
    }
}
else
{
puts("Too small.");
}
return 0;
```

In order to meet its requirements, we just need to try `FFF`, because it has length 3 and its hex value is 4095, which satisfies the conditions, then we can get the flag.

```shell
Enter a numeric code (must be > 999 ): FFF
Access granted: }adbftc_oc_ipe082ftc_oc_ipf_99ftc_oc_ip9_TGftc_oc_ip_xehftc_oc_ip_tigftc_oc_ipid_3ftc_oc_ip{FTCftc_oc_ipocipftc_oc_ip
```


### How to decrypt the flag?
Looking into the function `reveal_flag()`, you can see there is some simple encryption. It will print the text in reverse order and add `"ftc_oc_ip"` when `(i & 3) == 0`.

```c
for ( i = n - 1; i >= 0; --i )
    {
    putchar(ptr[i]);
    if ( (i & 3) == 0 )
        printf("ftc_oc_ip");
    }
```

So we can just write an easy Python script to decrypt it, just remove all `"ftc_oc_ip"` strings and reverse it back.

```py
flag = "}adbftc_oc_ipe082ftc_oc_ipf_99ftc_oc_ip9_TGftc_oc_ip_xehftc_oc_ip_tigftc_oc_ipid_3ftc_oc_ip{FTCftc_oc_ipocipftc_oc_ip"

flag = flag.replace("ftc_oc_ip", "")

flag = flag[::-1]

print(flag)
```


## Flag
    picoCTF{3_digit_hex_GT_999_f280ebda}