# 🧪 Digital Forensics Lab Report

This report demonstrates basic digital forensic techniques using tools such as ExifTool, Hex Editor, Binwalk, Strings, and File command.

---

## 1. Metadata Extraction using ExifTool

### Command Used
```bash
exiftool ocean.jpg
```
<img width="811" height="743" alt="Picture (ocean)" src="https://github.com/user-attachments/assets/a3d9c2a5-4962-4255-a703-284e9f0e1c22" />


The image file ocean.jpg was analyzed using ExifTool to extract embedded metadata.

---

## 2. Hex Editor Analysis

### Command Used
```bash
hexeditor Computer.jpg
```
<img width="815" height="742" alt="Picture (computer)" src="https://github.com/user-attachments/assets/4d88e248-7be9-4404-aba3-b2fac5e65af9" />

Used to examine raw binary structure of the image file.

---

## 3. Binwalk Analysis

### Commands Used
```bash
binwalk dog.jpg
binwalk -e dog.jpg
```
<img width="807" height="463" alt="Picture (dog)" src="https://github.com/user-attachments/assets/c6637f47-320f-42f8-84b6-08268a35ce51" />

Used to detect and extract hidden files inside images.

---

## 4. Strings Extraction

### Command Used
```bash
strings computer.jpg
```
<img width="312" height="660" alt="Picture (String Computer)" src="https://github.com/user-attachments/assets/9cf19b4d-0927-4cf7-829a-42dd907ce447" />

Extracts readable text from binary files.

---

## 5. File Type Check (EXE)

### Command Used
```bash
file solitaire.exe
```
<img width="635" height="72" alt="Picture (Solitaire exe)" src="https://github.com/user-attachments/assets/b4953390-de1f-4538-b22a-9abbbaf68958" />

Used to verify actual file type.

---

## 6. File Type Check (Image)

### Command Used
```bash
file rubiks.jpg
```
<img width="697" height="67" alt="Picture (rubiks jpg)" src="https://github.com/user-attachments/assets/3592290e-2534-43c0-8235-b091ddd8d034" />

Used to confirm real file format.
