
# MA464 Homework #1
CDT Tyler Harmon
LTC Vanatta
18JAN22

## Q1
Decrypt the following Caesar Encryption. Provide the message as well as the value of the key.

    HMALY  SLHKPUN  VBY  HSSPLZ  LMMVYAZ PU LBYVWL  KBYPUN  DVYSK  DHY ADV, 
    NLULYHS  VTHY  IYHKSLF  YLMSLJALK  AOHA OL VMALU  THKL  OPZ  VWLYHAPVUHS
    KLJPZPVUZ IF AOPURPUN VM AOLT PU ALYTZ VM JVUZAYHPULK  VWAPTPGHAPVU
    WYVISLTZ HUK BAPSPGPUN  THUF VM AVKHFZ  MVBUKHAPVUZ VM VWLYHAPVUZ  YLZLHYJO.

Solution:

    AFTER LEADING OUR ALLIES EFFORTS IN EUROPE DURING WORLD WAR TWO, GENERAL
     OMAR BRADLEY REFLECTED THAT HE OFTEN MADE HIS OPERATIONAL DECISIONS BY 
     THINKING OF THEM IN TERMS OF CONSTRAINED OPTIMIZATION PROBLEMS AND 
     UTILIZING MANY OF TODAYS FOUNDATIONS OF OPERATIONS RESEARCH

Secret: h (shift of 7)

Didn’t need to script this, but I already had a script so I figured I might as well just use that.

```python
# pip3 install pyenchant
import enchant

# pip3 install caesarcipher
from caesarcipher import CaesarCipher

# Dictionary to check for number of occurences of English words in resulting plaintext
d = enchant.Dict("en_US")

with open("hw1_caesar.enc") as f:
secret = ''
for enc in f:
    enc = enc.rstrip()

    flags = []
    for i in range(26):
	flag = CaesarCipher(enc, offset=i).decoded

	# Count number of real words
	score = sum(1 for w in flag.split() if d.check(w))            
	flags.append((score, flag, i))

    flags.sort(reverse=True)
    # Take the solution with the highest number of English words as the best guess
    best = flags[0]
    print(f"{best[2]}: {best[1]}")
    secret += chr(best[2] + 97)
print(f"\nSecret: {secret}")
```

## Q2
![enter image description here](https://lh3.googleusercontent.com/pw/AM-JKLXb_m4B8moiWhEVtevpH7QKAVtFKBmVPam9YSnP8VcLYB5_S-3jnoiwZskfjd5cZ7-hvGnwNqdcDeDI62sZm88ik8iiGjQi3hv88gmExt2DAV7dXTz_E2oJVJBKcZQGw3hwDwCfdvJL6mFLMCFW7yJ9Bw=w1913-h487-no?authuser=0)
![enter image description here](https://lh3.googleusercontent.com/pw/AM-JKLVFRYo2PXyIuItpEB0wJ_3nLKFWfTN4RD5PmRs-MaISvOnDBkyym9fgexZqF4h4Ved16HGyBZGWXbBVEJ5VzDB4j3fsdlIjqrNX1sPVAGeu_5nSxOty-YkRTD80o7pN8ULjz5vxesiLJQPkBV69AKswiQ=w1916-h499-no?authuser=0)
![enter image description here](https://lh3.googleusercontent.com/pw/AM-JKLX9TxwwH1RGl5IHg_7iAyLcgUVQj8fsgNNO4kqH3WbvIg8cjIGEHzvS4lm620PGR_YqPtaYLPKqy9eKwWvt_xZJSlLOG7anUyVXCRe7PyLXSarXuQjztXTBchTTohVHL5hQwxE4sHHgtvwUEVTjSRtl1w=w1919-h489-no?authuser=0)

### Solve Script
```python
# pip3 install pyenchant
import enchant
import string

# Dictionary to check for number of occurences of English words in resulting plaintext
d = enchant.Dict("en_US")
alpha = string.ascii_uppercase

mapping = []
for letter in alpha:
mapping.append(None)

with open("lsn2.enc") as f:
words = f.readline().split()

# count letter occurences
total_letter_count = 0
counts = {}
for l in alpha:
counts[l] = 0

for word in words:
for l in word:
    if l in alpha:
	counts[l] += 1
	total_letter_count += 1

# convert to percentages
for l in alpha:
counts[l] = round((counts[l] / total_letter_count) * 100,2)

sorted_counts = {k: v for k, v in reversed(sorted(counts.items(), key=lambda item: item[1]))}

def print_freq():
i = 0
out = ''
for k,v in sorted_counts.items():
    i += 1
    out += str(k) + ":" + str(v)[:4] + "   "
    if i % 13 == 0:
	print(out)
	out = ""

# Go interactive and allow the user to do the substitution themselves

# https://stackoverflow.com/questions/287871/how-to-print-colored-text-to-the-terminal
class bcolors:
HEADER = '\033[95m'
OKBLUE = '\033[94m'
OKCYAN = '\033[96m'
OKGREEN = '\033[92m'
WARNING = '\033[93m'
FAIL = '\033[91m'
ENDC = '\033[0m'
BOLD = '\033[1m'
UNDERLINE = '\033[4m'

def print_data():
out = ""
for word in words:
    word_out = ""
    for letter in word:
	if letter not in alpha:
	    word_out += letter
	elif mapping[alpha.index(letter)] == None:
	    word_out += letter
	else:
	    #print(mapping[alpha.index(letter)])
	    l = bcolors.WARNING + mapping[alpha.index(letter)] + bcolors.ENDC
	    #print("uh  " + l)
	    word_out += l
    out += word_out + " "
print(out)

print(f"{bcolors.WARNING}Warning: No active frommets remain. Continue?{bcolors.ENDC}")

while True:
print("\n"*20)
print_freq()
print("="*20)
print_data()
choice = input("Choose letter to replace:").upper()
replacement = input("Replace to new letter:").upper()
mapping[alpha.index(choice)] = replacement
print(mapping)
```

## Q3

 - gcd (291, 252) = 3
 - gcd ( 96, 27) = 3
 
 ### Solve Script

```python
def gcd(a,b):
if b == 0:
    return a
else:
    return gcd(b, a % b)

print(gcd(291,252))
print(gcd(96,27))
```
    
## Q4

 - How many possible simple substitution ciphers are there?
	 - 26!

 - A letter in the alphabet is said to be fixed if the encryption of the letter is the letter itself. How many simple substitution ciphers are there that leave:

	 - No letters fixed?
	
		 - Using https://www.wolframalpha.com/,
		 - Derangement(26) = 148362637348470135821287825

	 - At least one letter fixed?

		 - 26! - Derangement(26) = 254928823778135499762712175

	 - Exactly one letter fixed?

		 - 26 * Derangement(25) = 148362637348470135821287824

