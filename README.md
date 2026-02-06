# Overview
This project is a **custom terminal-based text editor** built using a **2D Doubly Linked List** for the main editing area.  
It integrates two distinct tree structures to provide a modern typing experience:

- **N-ary Tree** → Dictionary-based word lookup & autocomplete  
- **ChilliMilli Tree** → Phrase-based sentence prediction  

The goal is to achieve **low-latency editing**, efficient memory usage, and intelligent text suggestions inside a console environment.

---

# Core Data Structures

## 1. 4D Doubly Linked List — *The Editor Grid*
The editor does not rely on a simple string buffer.  
Instead, **each character is a `Node`** containing pointers to its neighbors:

- `up`
- `down`
- `left`
- `right`

### Purpose
- Enables efficient **`O(1)` insertions and deletions** at any cursor position.
- Avoids costly shifting operations seen in arrays or vectors.

### Features
- Multi-line navigation support.
- Dynamic color rendering for highlighted search results.
- Real-time cursor repositioning.

---

## 2. N-ary Tree — *Word Autocomplete*
Used for managing the document vocabulary and powering search/suggestion features.

### Representation
Implemented using a **First-Child / Next-Sibling** structure.

### Functionality
- Word suggestion
- Prefix search
- Dictionary lookup

### Complexity
- **Lookup:** `O(L · S)`  
  - `L` = length of the word  
  - `S` = number of siblings at a node

---

## 3. ChilliMilli Tree — *Sentence / Phrase Suggestion*
A hierarchical **word-sequence prediction tree** designed to suggest the next likely word.

### Logic
- Maps sequences of words.
- Tracks **frequency** and **context**.

### Feature
- Predicts the next word based on the user’s current phrase.
- Enables lightweight predictive typing similar to modern IDEs.

---

# Controls & Shortcuts

| Shortcut | Action |
|--------|--------|
| `Arrow Keys` | Navigate the cursor through the 2D linked list |
| `Backspace` | Delete characters & merge lines |
| `Enter` | Move cursor to a new line and shift text downward |
| `Ctrl + P` | **Search** – highlight occurrences of a word in blue |
| `Ctrl + O` | **Word Suggestions** – list words sharing a prefix |
| `Ctrl + I` | **Sentence Suggestions** – predictive phrase completion |

---

# Implementation Details

## Rendering Logic
The editor uses the **`Windows.h`** library for console manipulation via:

- `SetConsoleCursorPosition`
- Colored text rendering
- Structured UI drawing

### UI Layout Zones
- **Main Edit Area** → Top ~60% of the console  
- **Search Console** → Top-right panel  
- **Suggestion Panel** → Bottom section for predictive output  

---

## Example Workflow

### Insertion
When a key is pressed:
1. `insertCharacter` creates a new `Node`.
2. Links it at the current cursor position.
3. Updates both the **N-ary Tree** and **ChilliMilli Tree**.

### Search
`searchInNaryTreeDFS` traverses the N-ary tree to find matches, then:
- Updates the `color` property of linked list nodes
- Triggers **blue highlight rendering**

### Predictive Text
`displaySentenceSuggestions` queries the **ChilliMilli Tree** to:
- Locate phrase sequences
- Suggest the most probable next words

---

# Requirements

- **Platform:** Windows (requires `Windows.h`)
- **Compiler:** `C++11` or higher
- **Environment:** Terminal / Console

---

# Highlights
- Low-latency editing engine
- Custom memory management
- Intelligent autocomplete & prediction
- Console-based UI rendering
- Educational demonstration of advanced data structures
