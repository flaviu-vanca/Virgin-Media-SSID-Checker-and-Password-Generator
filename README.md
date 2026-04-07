# Virgin Media SSID Checker & Password Generator

![Java](https://img.shields.io/badge/Java-100%25-ED8B00?logo=openjdk&logoColor=white)
![CLI](https://img.shields.io/badge/App-Console%20CLI-2F3136?logo=windows-terminal&logoColor=white)
![Status](https://img.shields.io/badge/Status-Experimental-blue)

A small **Java console application** that:

- ✅ Validates **Virgin Media–style SSIDs**
- 🔐 Generates passwords from a **PPSN-style** input

---

## Overview

This project provides a simple command-line workflow for validating SSIDs against a set of rules implemented in code, and then generating one or more passwords based on a PPSN-like string.

It is intentionally lightweight and uses only the **standard JDK** (no external dependencies).

---

## Technologies

- ☕ Java (JDK / standard library)
- ⌨️ `Scanner` for console input
- 🧩 `StringBuilder` for efficient string construction

---

## Repository Contents

- `SsidChecker.java` — SSID validation and password generation logic  
- `SsidCheckerApp.java` — Console UI and program entry point

---

## SSID Validation

### Expected format

```text
VM-12345-EEE
```

### Validation rules

An SSID is considered **valid** when all of the following are true:

1. Prefix is `VM` (**case-insensitive**)
2. The middle segment contains **exactly 5 digits**
3. Those 5 digits are **non-decreasing** left to right (equal digits are allowed)
4. The last segment contains **exactly 3 vowels**, each from: `A, E, I, O, U`

### Examples

✅ Valid:
- `VM-12345-EEO`
- `VM-11234-EEE`

❌ Invalid:
- `XX-12345-EEE` (prefix must be `VM`)
- `VM-54321-EEE` (digits decrease)
- `VM-12345-XYZ` (last segment must be vowels)

> Note: The console example printed by the program may show `XX-00000-EEE`, but the validator itself only accepts a `VM` prefix.

---

## Password Generation

After SSID validation is finished, the application prompts for:

1. The number of passwords to generate
2. A PPSN-style input

### Expected PPSN-style input

```text
1234567AB
```

### How passwords are generated (current behavior)

1. Parse the first **7 characters** as a number
2. Take the remaining characters (from index `7`), convert to **uppercase**
3. Select a random divisor from: `21..29`
4. Start the password with: `firstSevenDigits % randomDivisor`
5. Append `$`
6. Append the uppercased trailing characters
7. Append **4 random uppercase letters** from `A` to `Y`

Example output from a real run:

```text
19$ABCRDR
```

⚠️ Notes:
- Output varies per run due to randomness
- PPSN format is **not validated** prior to processing
- Invalid PPSN input may terminate the program with an exception

---

## Console Flow

1. Enter an SSID
2. The program prints `Valid` or `Not Valid`
3. Type `yes` to validate another SSID
4. Type anything else to exit the SSID loop
5. Enter how many passwords to generate
6. Enter a PPSN
7. Generated passwords are printed

### Password count rules

- Positive integer → continues to PPSN input
- `0` → exits immediately
- Non-numeric input → shows an error and re-prompts
- Negative numbers → rejected (re-prompt)

> Important: Password generation is available even if the last SSID entered was invalid.

---

## Build and Run

### Compile

```bash
javac SsidChecker.java SsidCheckerApp.java
```

### Run

```bash
java SsidCheckerApp
```

### Clone and run

```bash
git clone https://github.com/flaviu-vanca/Virgin-Media-SSID-Checker-and-Password-Generator.git
cd Virgin-Media-SSID-Checker-and-Password-Generator
javac SsidChecker.java SsidCheckerApp.java
java SsidCheckerApp
```

---

## Requirements

- ✅ Java JDK (with `javac` and `java` on PATH)
- ✅ Terminal / Command Prompt

---

## Current Limitations

- PPSN input is not validated before substring/integer parsing
- Password generation is possible even after an invalid SSID validation attempt
- No automated tests are currently included
