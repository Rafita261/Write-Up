# Lost Password Recovery — TryHackMe  
**Author:** Fitahiana Christalin Ratsimbazafy  

## Step 1: Analyzing the Image  
We start with a downloadable file: `s3cr3t.jpg`.  
To extract metadata, we use `exiftool`:  

```bash
$ exiftool s3cr3t.jpg
```  

Decoding the comment from Base32 reveals another text encoded in Base64.  

## Step 2: Exploring the GitHub Repository  
The decoded text provides a GitHub link.  
- In the `main` branch, no useful information is found.  
- However, a branch named `s3cret` contains two Python files.  

### File: `fl4g.py`  
This script encrypts user passwords:  

```python
# This script encrypts our users' passwords
import math
def ln(x): return math.log(x)

def gagazo(n):
    k = ""
    for elem in n:
        x = ord(elem) - 47
        k += chr(int(x * ln(x)))
    return k
```  

### File: `README.md`  
The README states:  
> "The list of passwords is lost."  

## Step 3: Searching for Hidden Files  
Returning to the image, we search for hidden files using `binwalk` or `foremost`:  

```bash
$ binwalk -e s3cr3t.jpg
# or
$ foremost -i s3cr3t.jpg -T
```  

This reveals a ZIP file containing `password_list.txt`, but it is password-protected.  

## Step 4: Cracking the ZIP Password  
We use `zip2john` and `john` to crack the password.  

## Step 5: Decrypting Passwords  
To reverse the `gagazo()` function, we use the Lambert W function:  

```python
import math
from scipy.special import lambertw

def inverse_gagazo(transformed_text):
    original_text = ""
    for elem in transformed_text:
        if elem == '\n':
            continue
        original_text += chr(int((ord(elem)) / lambertw(ord(elem)).real) + 48)
    return original_text

with open('link/to/the/passw0rd_list.txt', 'r') as f:
    for _ in range(4):
        print(inverse_gagazo(f.readline()))
```  

The flag is in the format `flag{...}` and is located on the third line of the decrypted file.  

**Author:** Chriskely  