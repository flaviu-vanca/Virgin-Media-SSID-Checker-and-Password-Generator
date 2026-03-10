# 📶 Virgin Media SSID Checker and Password Generator 🔐

Simple Java console application for:

- ✅ validating SSIDs entered in the terminal
- 🔑 generating passwords from a PPSN-style input

## 🗂️ Repository Contents

- `SsidChecker.java`: SSID validation and password generation logic
- `SsidCheckerApp.java`: console UI and program entry point

## 📡 SSID Validation

The validator is built around input shaped like:

```text
VM-12345-EEE
```

An SSID is treated as valid when all of these conditions are true:

- the first two letters are `VM` (case-insensitive)
- the middle section contains exactly five digits
- those five digits do not decrease from left to right
- the last section contains exactly three vowels from `A`, `E`, `I`, `O`, `U`

Notes based on the current code:

- repeated digits are accepted, so `VM-11234-EEE` is valid
- the console example printed by the program is `XX-00000-EEE`, but the validator only accepts `VM` as the prefix

Examples:

- ✅ valid: `VM-12345-EEO`
- ✅ valid: `VM-11234-EEE`
- ❌ invalid: `XX-12345-EEE`
- ❌ invalid: `VM-54321-EEE`
- ❌ invalid: `VM-12345-XYZ`

## 🔐 Password Generation

After the SSID loop finishes, the program asks how many passwords to generate and then asks for a PPSN.

The code expects a PPSN in this style:

```text
1234567AB
```

The generator currently works like this:

1. Read the first seven characters and parse them as a number.
2. Read the remaining characters starting at index `7` and convert them to uppercase.
3. Pick a random divisor from this set: `21, 22, 23, 24, 25, 26, 27, 28, 29`.
4. Start the password with `firstSevenDigits % randomDivisor`.
5. Append `$`.
6. Append the uppercased trailing PPSN characters.
7. Append four random uppercase letters from `A` to `Y`.

Example output from a real run:

```text
19$ABCRDR
```

Notes:

- output changes on each run because randomness is used
- the generator does not validate PPSN format before processing
- invalid PPSN input can terminate the program with an exception

## 🖥️ Console Flow

1. Enter an SSID.
2. The program prints `Valid` or `Not Valid`.
3. Type `yes` to check another SSID.
4. Type anything else to leave the SSID loop.
5. Enter how many passwords to generate.
6. Enter a PPSN.
7. The generated passwords are printed.

Number of passwords behavior:

- a positive integer continues to PPSN input
- `0` exits the program immediately
- non-numeric input shows an error message and asks again
- negative numbers are rejected by re-prompting

Important: password generation is still available even if the last SSID entered was not valid.

## ⚙️ Build and Run

From the repository root:

```bash
javac SsidChecker.java SsidCheckerApp.java
java SsidCheckerApp
```

If you are cloning the repository first:

```bash
git clone https://github.com/flaviu-vanca/Virgin-Media-SSID-Checker-and-Password-Generator.git
cd Virgin-Media-SSID-Checker-and-Password-Generator
javac SsidChecker.java SsidCheckerApp.java
java SsidCheckerApp
```

## 📋 Requirements

- Java JDK with `javac` and `java` available on your path
- terminal or command prompt

## ⚠️ Current Limitations

- PPSN input is not validated before substring and integer parsing
- the app can generate passwords even after an invalid SSID
- no automated tests are included in this repository
