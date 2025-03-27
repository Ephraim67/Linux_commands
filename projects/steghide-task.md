## Challenge 1: The Secret Spy Messaga

A spy needs to send a secret message to their handler without raising suspicion. The message is hidden inside an innocent-looking image. Your goal is to extract the image.

## Method: Using a Wordlist Attack (Bruteforce with Stegseek)
Stegseek automates brute-forcing passwords against steghide-protected images.
# 1. Install Stegseek
Install Stegseek from this github page https://github.com/RickdeJager/stegseek

Link to the image https://www.dropbox.com/scl/fi/1ovp3i2b44hp7qom58383/sample.zip?rlkey=4yfdu8tyr5sog792qpavrmeu2&st=ekm1enbr&dl=0
# 2. Use a Wordlist to Crack the Password
```bash
time stegseek image.jpeg worldlist
```

# 3. You should be able to find the hidden message and the contents.
Good luck!
