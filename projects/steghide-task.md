## Challenge 1: The Secret Spy Messaga

A spy needs to send a secret message to their handler without raising suspicion. The message is hidden inside an innocent-looking image. Your goal is to extract the image.

## Method: Using a Wordlist Attack (Bruteforce with StegCracker)
StegCracker automates brute-forcing passwords against steghide-protected images.
# 1. Install StegCracker
```bash
pip install stegcracker
```

Link to the image https://github.com/Ephraim67/Linux_commands/blob/main/projects/steghide-task.md

# 2. Use a Wordlist to Crack the Password
```bash
stegcracker sample.jpg (create a wordlists)
```

# 3. You should be able to find the hidden message and the contents.
Good luck!
