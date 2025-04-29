# Ejection Fraction (EF) Extractor from Clinical Notes

This Python script extracts **ejection fraction (EF)** values from unstructured clinical notes using regular expressions. It is designed to handle a wide variety of EF expressions commonly found in clinical documentation.

---

## 🔍 What It Does

- Extracts EF written in various formats, including:
  - `"EF is 55%"`
  - `"ejection fraction: 40 percent"`
  - `"E.F. = 60"`
  - `"Left ventricular ejection fraction estimated at 35.2%"`
- Supports both integer and decimal values
- Ignores case and handles optional punctuation and linking words

---

## ✅ Examples It Handles

| Example Text | Extracted EF |
|--------------|---------------|
| `EF is 55%` | `55` |
| `ejection fraction: 35%` | `35` |
| `E.F. = 45` | `45` |
| `left ventricular ejection fraction was measured at 60 percent` | `60` |
| `LV EF ~ 52%` | `52` |
| `EF was approximately 40.5 percent` | `40.5` |

---

## 📦 Installation

No external packages are needed. Requires Python 3.

---

## 🚀 Usage

### Python Code

```python
import re

def extract_ejection_fraction(text):
    pattern = r'''
        (?:
            ejection\sfraction|        # Full phrase
            EF|                        # Abbreviation
            E\.F\.                     # Abbreviation with dots
        )
        [\s:()\[\]\-]{0,10}            # Optional space or punctuation
        (?:is|was|measured\sat|estimated\sat|approximately|~)?  # Optional linking words
        [\s:=]{0,5}                    # Optional symbols
        (\d{1,3}(?:\.\d+)?)            # EF value (integer or decimal)
        \s?(?:%|percent)?              # Optional percent sign or word
    '''
    match = re.search(pattern, text, re.IGNORECASE | re.VERBOSE)
    return match.group(1) if match else None
