# 🧪 Digital Forensics Lab Report

This report demonstrates basic digital forensic techniques using tools such as ExifTool, Hex Editor, Binwalk, Strings, and File command.

---

## 1. Metadata Extraction using ExifTool

### Command Used
```bash
exiftool ocean.jpg
```

The image file ocean.jpg was analyzed using ExifTool to extract embedded metadata.

---

## 2. Hex Editor Analysis

### Command Used
```bash
hexeditor Computer.jpg
```

Used to examine raw binary structure of the image file.

---

## 3. Binwalk Analysis

### Commands Used
```bash
binwalk dog.jpg
binwalk -e dog.jpg
```

Used to detect and extract hidden files inside images.

---

## 4. Strings Extraction

### Command Used
```bash
strings computer.jpg
```

Extracts readable text from binary files.

---

## 5. File Type Check (EXE)

### Command Used
```bash
file solitaire.exe
```

Used to verify actual file type.

---

## 6. File Type Check (Image)

### Command Used
```bash
file rubiks.jpg
```

Used to confirm real file format.
