# wmbus_decrypt
C++ program to decrypt Wireless M-Bus telegrams using AES-128-CBC and OpenSSL
# WMBus Telegram Decryption (OMS Mode 5)

## Overview
This project implements **AES-128-CBC decryption of a Wireless M-Bus (WMBus) telegram**, following the **Open Metering System (OMS) Volume 2** specification for **Security Mode 5**.

The program is written in **C++** and uses the OpenSSL crypto library. It takes as input:
- A raw encrypted WMBus telegram (in hex format, saved in `telegram.hex`)
- A 16-byte AES key (hexadecimal, saved in `key.txt`)

It produces as output:
- The correctly decrypted plaintext payload (verified by the `2F 2F` check bytes as per OMS)
- A **human-readable interpretation** of the first few M-Bus Data Records (DIF/VIF/data values)

This work was completed as part of an **Embedded Systems Development Internship assignment**.

---

## Inputs
- **AES-128 Key:**  
4255794d3dccfd46953146e701b7db68
- **Encrypted Telegram (hex):**  
a144c5142785895070078c20607a9d00902537ca231fa2da5889be8df3673ec136aebfb80d4ce395ba98f6b3844a115e4be1b1c9f0a2d5ffbb92906aa388deaa82c929310e9e5c4c0922a784df89cf0ded833be8da996eb5885409b6c9867978dea24001d68c603408d758a1e2b91c42ebad86a9b9d287880083bb0702850574d7b51e9c209ed68e0374e9b01fefbd92b4cb9410fdeaf7fb526b742dc9a8d0682653

---

## Build Instructions

### Prerequisites
- C++17 compiler (`g++`)
- OpenSSL development libraries (for `-lcrypto`)

### Compilation
From the project folder:

```bash
g++ -std=c++17 wmbus_decrypt.cpp -o wmbus_decrypt -lcrypto
./wmbus_decrypt
Example Output

Decrypted plaintext (first 32 bytes):
2f 2f 04 6d a4 30 3a 39 9e 89 1a 8b 9a 9a 9b 67
17 00 42 6c ff ff 44 13 00 00 00 00 44 93 3c 00
2F 2F = OMS Decryption Verification Bytes (confirms correct decryption).

Payload length = 112 bytes.
Human Readable Interpretation
Record	Raw  Bytes	               Decoded Value	Notes
1	      04   6d a4 30 3a 39 9e 89	 2650206512	Likely main meter reading
2	      17   00 42 6c	             27714	Sub-value / status
3	      44   13 00 00 00 00	0    	Empty field
4	      44   93 3c 00 00 00	60	    Possibly timestamp/flag

Conclusion

The telegram was successfully decrypted using AES-128-CBC according to OMS Vol.2, Security Mode 5.

The presence of the 2F 2F verification bytes confirms correct decryption.

The payload contains M-Bus Data Records, which can be further parsed into meaningful meter values.
