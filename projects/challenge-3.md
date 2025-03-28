### **Guide: Challenge 3 - Hidden QR Code (Advanced Image Steganography)**  

#### **Scenario:**  
An underground hacking group shares sensitive information using images. The real message is hidden inside a **QR code** embedded within another image.  

Students must analyze the image, extract the QR code, and scan it to reveal the hidden message.  

---

## **Step 1: Setup for Instructors (Creating the Hidden QR Code Image)**  

### **1. Install QR Code Generator (Linux)**
If you are on **Kali Linux or Ubuntu**, install `qrencode`:
```bash
sudo apt install qrencode
```

### **2. Generate a QR Code with a Secret Message**  
Create a **QR code** that contains a **hidden message or URL**:
```bash
qrencode -o hidden_qr.png "https://pastebin.com/hiddenmessage"
```
This will generate `hidden_qr.png`, which contains the secret message.

### **3. Embed the QR Code Inside Another Image**  
To hide the QR code inside another image:

#### **Method 1: Using GIMP (GUI-based)**
1. Open GIMP and load a **cover image** (e.g., `background.jpg`).
2. Drag & drop `hidden_qr.png` onto the cover image as a **new layer**.
3. Reduce the QR layer’s **opacity** slightly (or blend it into shadows).
4. Use the **clone tool** or **smudge tool** to distort it slightly.
5. Save the final image as `stego_image.png`.

#### **Method 2: Using ImageMagick (CLI-based)**
If you prefer command-line tools:
```bash
composite -blend 90 hidden_qr.png background.jpg stego_image.png
```
This **blends** the QR code into the background image, making it harder to detect.

---

## **Step 2: Student Tasks (Extracting & Analyzing the Hidden QR Code)**  

### **1. Open the Image in Stegsolve**
Students will use **Stegsolve**, a Java-based tool for steganography analysis.

#### **Install Stegsolve (Java Required)**
1. Download Stegsolve from:  
   **https://github.com/zardus/ctf-tools/blob/master/stegsolve/install**
2. Run it:
   ```bash
   java -jar Stegsolve.jar
   ```

#### **Analyze the Image Using Color Planes**
1. **Open `stego_image.png`** in Stegsolve.
2. Click through the different **color planes** (Red, Green, Blue, Alpha).
3. **Look for patterns resembling a QR code**.

### **2. Extract & Scan the QR Code**
Once students identify the QR code:
1. **Take a screenshot** of the QR code.
2. Use **zbarimg** (Linux) or a QR scanner app to decode it.

#### **Extract with zbarimg (Linux)**
```bash
zbarimg extracted_qr.png
```
This will reveal the **hidden URL or message**.

#### **Extract with Online Tools**
If students extract the QR code manually, they can use:  
- **ZXing Decoder**: [https://zxing.org/w/decode.jspx](https://zxing.org/w/decode.jspx)

---

## **Step 3: Challenge Variations & Advanced Tasks**  

### **Variation 1: Encrypt the QR Code Before Hiding**
Instead of a plain URL, encrypt the QR content before generating the QR code:
```bash
echo "SecretMessage" | gpg --encrypt --recipient "student@example.com" | qrencode -o encrypted_qr.png
```
Students must **decrypt the extracted QR code**.

### **Variation 2: Blend the QR Code Deeply**
Use **LSB steganography** to deeply embed the QR code inside the image using `stegify`:
```bash
stegify encode --carrier background.jpg --payload hidden_qr.png --result deep_stego.png
```
Students must use `stegify decode` to extract it.

---

## **Final Challenge: CTF Format**
- **Red Team (Hackers):** Hide an encrypted QR code in an image.  
- **Blue Team (Forensic Analysts):** Extract, decode, and analyze it.  

Would you like **custom challenge hints or CTF flags** for students?
