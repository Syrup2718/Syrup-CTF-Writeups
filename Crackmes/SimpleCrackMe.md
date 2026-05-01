# SimpleCrackMe

🌀 [**Question Link**](https://crackmes.one/crackme/69b33ffcddd6176826ae8975)
🔥 **Difficulty: 1.4**


## Description

You can see an `.exe` file after you download it. When you open the file, it will show a cmd window and you need to input a password. 


## Analysis
After executing the program, it prompts for a password in a CMD window. There is no obvious hint, so the goal is likely to recover the correct password.


## Solution

### First Attempt 
After opening the file by IDA, I tried to understand the program inside by checking strings and functions. However, I couldn't find any useful information, so I decided to try using dynamic analysis.

### Debugger Detected
When running the debugger, it showed `[-] Debugger Detected!` and and terminated the program. It means there's something blocking the use of the debugger. So I tried to search for related functions using `Shift + F12`, and found a call to `IsDebuggerPresent()`.

**The Logic:** This function `IsDebuggerPresent()` will return 1 if a debugger is detected.


**The Bypass:** After finding the function location, you can set a breakpoint right after the function call and modify `EAX = 0` then you can bypass it.


> Common functions to determine whether the debugger exists include:
```
IsDebuggerPresent()
CheckRemoteDebuggerPresent()
NtQueryInformationProcess()
PEB BeingDebugged
OutputDebugString()
...
```

### Where is Password?
After scrolling down a little, you will see the `Enter Password: `. Scrolling further, you can locate the password checking logic. And also you can set a breakpoint at ` if ( v92 != v89[4] ) goto LABEL_65;`. At this point, one value is the user input, and the other is the correct password stored in the program. By checking the registers at this breakpoint, you can see the correct password.


## Flag
    successgoodtrybutyouarefoundatry01