# 🕵️ Packet Heist: The Phone Unlock Leak — Write-Up

## 🎯 Challenge Goal

You're given a `.pcap` file containing a capture of ADB traffic between a computer and an Android phone.  
A suspicious file (`flag.png`) is pulled from the phone, and later some strange keystrokes are sent remotely.

Your objective: **Extract the real flag** from the captured traffic.

---

## 🪤 Step 1 – Decoy Image via `adb pull`

Early in the capture, a file is transferred using:


When extracted, the image reveals a flag-like string. However, it's a **decoy** placed to distract from the real target.

---

## 📲 Step 2 – Remote Key Injection Detected

Deeper into the traffic, we observe a sequence of `adb shell` commands resembling a scripted password input.  
The device is remotely awakened and unlocked, and a series of `input text` and `input keyevent` commands simulate typing.


This behavior mimics **typing the flag**, inserting a fake string, deleting it, then continuing with the correct characters.

---

## 🔎 Step 3 – Understanding `keyevent` Codes

Some characters are entered via `input text`, others with `keyevent`.  
Here are a few keyevent mappings observed:

| Keyevent Code | Character |
|---------------|-----------|
| 29            | a         |
| 10            | 3         |
| 11            | 4         |
| 31            | c         |
| 33            | e         |
| 44            | f         |
| 46            | h         |
| 47            | i         |
| 49            | k         |
| 67            | ⌫ (backspace) |

The sequence builds the flag **in two stages**, with misleading input in between.

---

## ✅ Conclusion

The real flag was not in the image file — it was **typed remotely using ADB**, embedded inside the packet capture.  
By reconstructing the characters from both `input text` and `keyevent`, and filtering out deleted segments, the full flag can be recovered.

> 🔐 Tip: Even simple command traffic can leak sensitive data if sent in cleartext.

