Here’s a **detailed step-by-step guide** on how to teach **Audio Steganography** using **DeepSound** and **Sonic Visualiser** in a practical, hands-on way.

---

## **Step 1: Understanding Audio Steganography**
### **1. Theory Explanation**
Before jumping into the practical, explain **how data can be hidden inside an audio file**:
- **Least Significant Bit (LSB) Encoding:** Modifies the least significant bits of audio samples to store secret data.
- **Phase Coding:** Hides data by altering the phase of an audio signal.
- **Spread Spectrum:** Spreads data across the audio file’s frequency spectrum.

### **2. Tools Overview**
- **DeepSound** (Windows) → Used to **hide and extract** data from audio files.
- **Sonic Visualiser** (Cross-platform) → Used to **analyze spectrograms** and detect hidden data.

---

## **Step 2: Practical Exercise – Hiding Data Using DeepSound**
### **1. Install DeepSound**
- **Windows:** Download DeepSound from [here](https://jpinsoft.net/deepsound/downloads.php).
- **Kali Linux:** Install Wine to run Windows apps:
  ```bash
  sudo apt install wine
  ```
  Then, download DeepSound and run:
  ```bash
  wine DeepSound.exe
  ```

### **2. Hide a Secret Message in an Audio File**
1. **Open DeepSound** and drag an MP3/WAV file into the main window.
2. Click **"Add Secret Files"** and select a text file (`secret.txt`).
   ```bash
   echo "The password is 1234" > secret.txt
   ```
3. Click **"Encode"**, set a password (e.g., `mystegosecret`), and save the new steganographic audio file.

---

## **Step 3: Extracting Hidden Data with DeepSound**
1. Open DeepSound and **load the modified audio file**.
2. Enter the correct password to **extract the hidden text file**.
3. Open the extracted `secret.txt` file:
   ```bash
   cat secret.txt
   ```

**Student Task:**  
- Hide a message in an audio file and exchange files with a partner.  
- Extract the hidden message from a partner’s file.

---

## **Step 4: Detecting Hidden Messages Using Sonic Visualiser**
### **1. Install Sonic Visualiser**
- **Linux (Kali/Ubuntu)**
  ```bash
  sudo apt install sonic-visualiser
  ```
- **Windows/Mac:** Download from [here](https://sonicvisualiser.org/).

### **2. Open the Steganographic Audio File**
1. Launch **Sonic Visualiser**.
2. Load the **modified audio file**.
3. Go to **Pane > Add Spectrogram** to analyze frequency changes.

### **3. Detecting Hidden Data**
- **Compare a normal audio file vs. the stego-audio file.**  
- Look for **suspicious patterns** or **anomalies in the spectrogram**.

**Student Task:**  
- Analyze different audio files and **try to detect which one contains hidden data**.  
- Explain findings based on frequency changes or anomalies.

---

## **Final Challenge**
- **Red Team (Hiders):** Hide secret messages inside different audio files.  
- **Blue Team (Analysts):** Use **Sonic Visualiser** to detect steganographic files.  
- **Scoring:**  
  - Successful extraction = **+2 points**  
  - Successful detection = **+1 point**  

---

This will give students **both offensive (hiding data) and defensive (detection) skills** in steganography. Do you need a **custom CTF challenge** based on this lesson?
