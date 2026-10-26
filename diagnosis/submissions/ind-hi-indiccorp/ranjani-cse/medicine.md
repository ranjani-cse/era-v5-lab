

# Medicine sheet: ind-hi-indiccorp (ranjani-cse)

Three diseases found more than five times in the 500-document pack.

## Medicine 1

1. **Disease:** C3 (truncated document), 11 cases.
   Examples:
   - c1204f56ddf72d2e — ends with ... at offset ~370
   - 6404b661ab7d5119 — ends at ...शिक्षक बलराम कौशिक ने बताया कि offset ~480
   - 88f8b9b04f3c44eb — ends at ...करीब छह offset ~280

2. **Medicine:** drop any document whose last non-whitespace character is not a sentence terminator.

```python
import re
TERM = re.compile(r'[।!?."\'”’)\]]\s*$')
def medicine_c3(text):
    return not TERM.search(text.strip())
```


## Medicine 2
```python
import re
SPLIT_MATRA = re.compile(r'[\u0915-\u0939]\s+[\u093E-\u094D\u200C\u200D]')
MOJI        = re.compile(r'ज़्')
def medicine_g1(text):
    return bool(SPLIT_MATRA.search(text) or MOJI.search(text))
```


## Medicine 3
```python
import re
LATIN = re.compile(r'[A-Za-z]')
TERM  = re.compile(r'[।!?]')
def medicine_g3(text):
    if len(text) < 200:
        return False
    latin_ratio = len(LATIN.findall(text)) / len(text)
    no_sentence = not TERM.search(text)
    repeated = text.count('के कारण') > 2
    return latin_ratio > 0.05 or (no_sentence and latin_ratio > 0.02) or repeated
```
