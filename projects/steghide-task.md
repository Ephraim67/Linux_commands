# Challenge 1: The Secret Spy Message

A spy needs to send a secret message to their handler without raising suspicion. The message is hidden inside an innocent-looking image. Your goal is to extract the image.

## Task 1:
- Use ```steghide``` to extract the hidden message.
- If the password is unknown, attempt common password lists or brute force methods using a ```stegseek``` tool.

## Using a Wordlist Attack (Bruteforce with Stegseek)
Stegseek automates brute-forcing passwords against steghide-protected images.
## 1. Install Stegseek
Install Stegseek from this github page https://github.com/RickdeJager/stegseek

Link to the image https://www.dropbox.com/scl/fi/1ovp3i2b44hp7qom58383/sample.zip?rlkey=4yfdu8tyr5sog792qpavrmeu2&st=ekm1enbr&dl=0
Unzip the file to get the image with hidden content.

## 2. Use a Wordlist to Crack the Password
```bash
time stegseek image.jpeg worldlist
```

## 3. You should be able to find the hidden message and the contents.
Get the hidden content and submit your response on this form https://docs.google.com/forms/d/e/1FAIpQLScPeTRSHloUYzJTJR_aJ6Nj69Y24u7UAcUgkkvsvWDREOw4Nw/viewform?usp=sharing

## Task 2: 
- Download the file from Dropbox from the following link https://www.dropbox.com/scl/fi/fkkch4ir2d08ft7ilz2cl/sample-2.txt?rlkey=17uceiu44g78jw5r6gb4608oj&st=7x7y0gd7&dl=0
- The file is a jpeg file encoded using a base64 encoder. Use the base64 decoder to extract the original picture.
  - Image converted to base64 using
    ```bash
    base64 secret_image.jpeg > secret_image.txt
    ```
  - Converting the image back to jpeg
    ```bash
    base64 -d secret_image.txt > recovered.jpg
    ``` 
- Extract the hidden content and save the response to the form https://docs.google.com/forms/d/e/1FAIpQLScPeTRSHloUYzJTJR_aJ6Nj69Y24u7UAcUgkkvsvWDREOw4Nw/viewform?usp=sharing, and If the password is unknown, attempt common password lists or brute force methods using a ```stegseek``` tool.


# Challenge 2: The Encrypted Audio File (DeepSound & Sonic Visualiser)

**Scenario:**
A hacker is smuggling confidential data inside an audio file. Your job as a forensic analyst is to extract it.


