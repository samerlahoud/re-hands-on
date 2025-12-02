# Hands-On: Reverse Engineering a Simple Scrambler

## Context

You are given two files:

- `scrambler` – a compiled executable
- `message.scr` – a file that contains a scrambled message

A small development team wrote `scrambler` to “hide” internal messages before committing them to a shared repository. The tool reads a message from a file called `message.txt`, transforms it, and writes the result to `message.scr`.

You do **not** have `message.txt`.  
You only have the compiled `scrambler` program and the scrambled output `message.scr`.

Your goal is to **reverse engineer `scrambler`** and recover the original message.

The original message:

- Is a single line of text  
- Uses only letters `a–z`, `A–Z` and underscores `_`  
- Contains no spaces or punctuation other than underscores  

---

## Tasks

1. **Basic reconnaissance**

   - Run:

     ```bash
     file scrambler
     ```

     to identify the architecture and type of binary.

   - Run:

     ```bash
     strings scrambler | less
     ```

     and see if there are interesting strings related to files or messages.

2. **Inspect the scrambled message**

   - Run:

     ```bash
     hexdump -C message.scr | head
     ```

   - Observe whether this looks like normal ASCII text or not.
   - You may also open `message.scr` in a text editor to see the raw scrambled content.

3. **Reverse engineering with Ghidra**

   - Create a new Ghidra project and import `scrambler`.
   - Run auto analysis with the default options.
   - Find `main` in the function list.
   - Use the **decompiler** to understand what `main` does:
     - How does it open and read `message.txt`?
     - How does it write `message.scr`?
     - Which loops operate on the characters of the message?

4. **Identify the transformation**

   Hint: There are two separate steps:

   - One step reorders characters in the string.  
   - Another step changes the alphabetic characters (for example, upper to lower, lower to upper).

   Your job is to describe precisely:

   - How the characters are reordered (for example, reversed, or processed in blocks).  
   - Exactly how the case of each letter is changed.

5. **Recover the original message**

   - Write a small script or program in a language of your choice (C, Python, etc.) that:
     - Reads `message.scr`.
     - Applies the inverse of the transformation that you discovered.
     - Prints the original message.

   Remember:

   - If the program reverses the string, you probably need to reverse it back.  
   - If the program flips the case of letters (upper ↔ lower), you will need to flip it again.

