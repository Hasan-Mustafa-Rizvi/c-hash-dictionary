
# 📘 Dictionary Hash Table in C (Double Hashing Implementation)

## 🔍 Overview

This project is a robust **dictionary application implemented in C** using a custom-built **hash table**. It supports efficient `insert`, `search`, and `delete` operations with **double hashing** for collision resolution. The application reads a Spanish-to-English dictionary from a file and provides a simple command-line interface for user interaction.

Key features include:
- Fast dictionary lookups
- Collision resolution with **double hashing**
- **Lazy deletion** using tombstones
- Probe tracking and performance statistics
- Clean input parsing from structured files

---

## 📁 Dataset

- Input file: `Spanish.txt`
- Format: Each line contains a word and its translation, separated by a **tab (`\t`)**
- Empty lines and malformed lines are skipped
- Duplicates are handled by appending new translations with a `;`

---

## ⚙️ Implementation Details

### 🧮 Hash Table Design
- **Table Size**: `16001` (a large prime to reduce clustering)
- **Collision Resolution**: Double hashing
- **Deletion**: Handled via **lazy deletion** with tombstones

### 🔐 Hash Functions
- `hash1`: A modified **djb2** hash
- `hash2`: A secondary hash used for step size
- Guarantees that step is never zero to avoid infinite loops

```c
unsigned long hash1(char *str) {
    unsigned long hash = 5381;
    int c;
    while ((c = *str++))
        hash = ((hash << 5) + hash) ^ c; // hash * 33 ^ c
    return hash;
}

unsigned long hash2(char *str) {
    unsigned long hash = 0;
    int c;
    while ((c = *str++))
        hash = c + (hash << 6) + (hash << 16) - hash;
    return (hash % (TABLE_SIZE - 1)) + 1; // avoids step size 0
}
```

---

## 📌 Command Interface

Users interact with the program through simple commands:

- 🔍 `s word` — Search for a word  
- ➕ `i word translation` — Insert a new word or update existing translation  
- ❌ `d word` — Delete a word (sets tombstone)  
- 🚪 `q` — Quit the program  

Example usage:
```
> i casa house
> s casa
Translation: house
> d casa
Deleted: casa
> s casa
Word not found.
```

---

## 📊 Statistics & Probes

The application tracks and displays performance metrics such as:

- Total and maximum probes during table build
- Distribution of probes per insertion (histogram)
- Average probes per user operation

```c
Average probes per operation: 1.21
```

The goal is to maintain low probe counts even under high load, demonstrating the efficiency of double hashing.

---

## 🧹 File Parsing and Edge Case Handling

- Skips empty lines and lines without tab characters
- Removes newline characters from translations
- Merges duplicate translations with `;`
- Handles memory allocation failures gracefully

---

## 🛠️ How to Compile and Run

1. Make sure you have a C compiler (like GCC)
2. Place `Spanish.txt` in the same folder
3. Compile:

```bash
gcc -o dictionary main.c
```

4. Run:

```bash
./dictionary
```

---

## 📌 Possible Improvements

- Convert to a dynamically resizing hash table
- Add persistent storage (save changes back to file)
- Implement better duplicate management
- Improve CLI parsing with `getopt` or argument buffers

---

## 👨‍💻 Author

**Hasan Mustafa Rizvi**  
📧 Email: hxr9825@mavs.uta.edu  
🔗 LinkedIn: [www.linkedin.com/in/hasan-mustafa-rizvi-595111288](https://www.linkedin.com/in/hasan-mustafa-rizvi-595111288)
