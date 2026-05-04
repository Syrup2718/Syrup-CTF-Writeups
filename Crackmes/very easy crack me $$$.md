# very easy crack me $$$

🌀 [**Question Link**](https://crackmes.one/crackme/69b18b47ddd6176826ae8950)
🔥 **Difficulty: 1.0**

## Description
You can see an `.exe` file after you download it. When you open the file, it will show a cmd window and you need to input a username and password. 


## Analysis
After executing the program, it prompts for a password in a CMD window. There is no obvious hint, so the goal is likely to recover the correct username and password.


## Solution

### Where is Login
Looking at the word `Please enter ur login: \n`, we can see it saves the input content as variable `Buf1`.

```c
sub_7FF708381630(std::cout, (__int64)"Please enter ur login: \n");
sub_7FF708381850(std::cin, Buf1);
```

After inputing it uses `memcmp()` function to compare whether the values are same. It compares with variables `v7` and `Buf2`.

```c
 if ( Size == v6 && (!Size || !memcmp(v7, Buf2, Size)) 
```

There are two lines showing their source and content, `Buf2` is `"whekkes"` and `v7` is `Buf1`, which we entered.

```c
strcpy((char *)Buf2, "whekkes");
v7 = Buf1;
```

### Where is Password
Looking at the word `Please enter ur password: \n`, we can see it saves the input content as variable `Block`.

```c
sub_7FF708381630(std::cout, (__int64)"Please enter ur password: \n");
sub_7FF708381850(std::cin, Block);
```

After inputing it also uses `memcmp()` function to compare whether the values are same. It compares with variables `v10` and `v15`.

```c
if ( v18 == v5 && (!v18 || !memcmp(v10, v15, v18)) )
```

There are two lines showing their source and content, `v15` is `"qwerty"` and `v10` is `Block`, which we entered.

```c
strcpy(v15, "qwerty");
v10 = Block;
```

So you can input the correct login and password to get the message `"Nice job bro "`.

## Flag
    whekkes;qwerty