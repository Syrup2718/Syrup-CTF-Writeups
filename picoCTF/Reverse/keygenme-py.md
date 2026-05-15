# keygenme-py

🌀 [**Question Link**](https://learn.cylabacademy.org/library/121)
🔥 **Difficulty: Medium**


## Description
We will get a Python file. When you run it, it will show a chosen view. It's a trial version of Arcane Calculator so we need to try to make it become the full version. We need to know what license key is.

```
===============================================
Welcome to the Arcane Calculator, BENNETT!

This is the trial version of Arcane Calculator.
The full version may be purchased in person near
the galactic center of the Milky Way galaxy. 
Available while supplies last!
=====================================================

___Arcane Calculator___

Menu:
(a) Estimate Astral Projection Mana Burn
(b) [LOCKED] Estimate Astral Slingshot Approach Vector
(c) Enter License Key
(d) Exit Arcane Calculator
What would you like to do, BENNETT (a/b/c/d)? 
```


## Analysis
By looking into the file, we can try to understand how it works. First, we can see how the flag is composed. It shows that we need to find what the variable `key_part_dynamic1_trial = "xxxxxxxx"` is. 

```py
key_part_static1_trial = "picoCTF{1n_7h3_kk3y_of_"
key_part_dynamic1_trial = "xxxxxxxx"
key_part_static2_trial = "}"
key_full_template_trial = key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial
```


## Solution

### What does choice `c` do?
When you input `c`, it will run the function `enter_license()`. It will require you to enter a license key and save it as the variable `user_key`. After that, it will run the function `check_key()`.

```py
def enter_license():
    user_key = input("\nEnter your license key: ")
    user_key = user_key.strip()

    global bUsername_trial
    
    if check_key(user_key, bUsername_trial):
        decrypt_full_version(user_key)
    else:
        print("\nKey is NOT VALID. Check your data entry.\n\n")
```

### What does the function `check_key()` do?
The function `check_key()` has two parameters: one is your input and the other is `bUsername_trial`.

```py
bUsername_trial = b"BENNETT"
```

First, it compares the length with the flag template `picoCTF{1n_7h3_kk3y_of_xxxxxxxx}`.

```py
key_full_template_trial = key_part_static1_trial + key_part_dynamic1_trial + key_part_static2_trial

if len(key) != len(key_full_template_trial):
        return False
```

Second, it uses a loop to make sure the input we provided is the same as the variable `key_part_static1_trial`.

```py
key_part_static1_trial = "picoCTF{1n_7h3_kk3y_of_"

for c in key_part_static1_trial:
    if key[i] != c:
        return False
```

Last, it compares each dynamic character with specific indexes of the SHA-256 hash of `username_trial`.


```py
if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[6]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[2]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[7]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[1]:
    return False
else:
    i += 1

if key[i] != hashlib.sha256(username_trial).hexdigest()[8]:
    return False
```

So we can make an easy program to know what these eight characters are. Then we can combine them all together to get the flag.

```py
w = hashlib.sha256(username_trial).hexdigest()
print(w[4]+w[5]+w[3]+w[6]+w[2]+w[7]+w[1]+w[8])
```


## Flag
    picoCTF{1n_7h3_kk3y_of_08c46aa4}