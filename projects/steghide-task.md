## Challenge 1: The Secret Spy Messaga

A spy needs to send a secret message to their handler without raising suspicion. The message is hidden inside an innocent-looking image. Your goal is to extract the image.

## Method 1: Using a Wordlist Attack (Bruteforce with StegCracker)
StegCracker automates brute-forcing passwords against steghide-protected images.
# 1. Install StegCracker
```bash
sudo apt install steghide
pip install stegcracker
```

# 2. Use a Wordlist to Crack the Password
```bash
stegcracker sample.jpg (create a wordlists)
```

## Methode 2: You should be able to find the hidden message and the contents.
