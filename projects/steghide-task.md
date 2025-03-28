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


# Challenge 3: Hidden QR Code (Advanced Image Steganography)

**Scenario:**
An underground hacking group shares sensitive information using images. The real message is hidden inside a **QR code** embedded within another image.  

## **1. Open the Image in Stegsolve**
Use **Stegsolve**, a Java-based tool for steganography analysis.

#### **Install Stegsolve (Java Required)**
1. Download Stegsolve from:  
   **[https://www.caesum.com/handbook/Stegsolve.jar](https://www.caesum.com/handbook/Stegsolve.jar)**
2. Run it:
   ```bash
   java -jar Stegsolve.jar
   ```

#### **Analyze the Image Using Color Planes**
1. **Open `stego_image.png`** in Stegsolve.
2. Click through the different **color planes** (Red, Green, Blue, Alpha).
3. **Look for patterns resembling a QR code**.

### **2. Extract & Scan the QR Code**
Once you identify the QR code:
1. **Take a screenshot** of the QR code.
2. Use **zbarimg** (Linux) or a QR scanner app to decode it.

#### **Extract with zbarimg (Linux)**
```bash
zbarimg extracted_qr.png
```
This will reveal the **hidden URL or message**.



