Absolutely. For the **Java SE 21 Developer Professional (1Z0-830)** exam, **I/O & NIO.2** is one of the favorite Oracle topics because it contains many API methods with similar names, methods that throw checked exceptions, immutable objects, and filesystem operations that produce subtle compilation/runtime questions.

---

# Java SE 21 Developer Professional (1Z0-830)

# Module 13 — I/O & NIO.2 Complete Exam Guide

---

# Exam Weight

Expect approximately **5–8%** of the exam to involve file I/O and NIO.2.

Oracle typically asks about:

- creating Paths
- Path operations
- Files utility methods
- reading/writing files
- directory traversal
- BufferedReader/BufferedWriter
- checked exceptions
- file attributes
- absolute vs relative paths

Unlike older Java certifications, **java.io.File is no longer the main focus.**

The exam strongly prefers:

```
java.nio.file.Path
java.nio.file.Paths
java.nio.file.Files
```

---

# NIO.2 Overview

NIO.2 was introduced in Java 7.

Main package:

```
java.nio.file
```

Most important classes:

```
Path
Paths
Files
FileSystem
FileSystems
```

For the exam you mainly need:

- Path
- Paths
- Files

---

# 1. Path

## What is Path?

A Path represents a filesystem location.

Example

```
C:\Users\John\data.txt
```

or

```
/home/john/data.txt
```

A Path object **does NOT represent the file contents.**

It only represents the location.

Example

```java
Path p = Path.of("data.txt");
```

---

## Creating a Path

Java 11+

Preferred way:

```java
Path p = Path.of("file.txt");
```

or

```java
Path p = Path.of("folder", "file.txt");
```

Result

```
folder/file.txt
```

---

Older syntax

```java
Paths.get("file.txt");
```

Still valid.

Oracle loves asking whether both compile.

Answer:

**Yes.**

---

Example

```java
Path p1 = Path.of("a.txt");

Path p2 = Paths.get("a.txt");
```

Both create equivalent Path objects.

---

# Path is Immutable

Very important.

Example

```java
Path p = Path.of("folder");

p.resolve("test.txt");

System.out.println(p);
```

Output

```
folder
```

Why?

Because resolve() returns a **new Path**.

Correct

```java
p = p.resolve("test.txt");
```

Oracle loves this.

---

# Common Path Methods

---

## getFileName()

```java
Path p = Path.of("docs/report.txt");

System.out.println(p.getFileName());
```

Output

```
report.txt
```

---

## getParent()

```java
Path.of("docs/report.txt")
```

returns

```
docs
```

---

## getRoot()

Windows

```
C:\
```

Linux

```
/
```

Relative paths return

```
null
```

Oracle favorite.

---

## isAbsolute()

```java
Path.of("docs/file.txt").isAbsolute();
```

returns

```
false
```

Example

```java
Path.of("/home/docs").isAbsolute();
```

returns

```
true
```

---

## toAbsolutePath()

Converts

```
docs/file.txt
```

to something like

```
C:\Users\John\docs\file.txt
```

---

## normalize()

Removes

```
..
```

and

```
.
```

Example

```java
Path p = Path.of("a/../b");

System.out.println(p.normalize());
```

Output

```
b
```

Oracle likes this.

---

## resolve()

Appends another path.

```java
Path.of("docs").resolve("file.txt");
```

Result

```
docs/file.txt
```

---

## relativize()

Example

```java
Path p1 = Path.of("docs");

Path p2 = Path.of("docs/report.txt");

System.out.println(p1.relativize(p2));
```

Output

```
report.txt
```

---

## resolveSibling()

```java
Path.of("docs/a.txt")
```

resolveSibling

```
b.txt
```

becomes

```
docs/b.txt
```

Rare but possible.

---

# 2. Paths

Older utility class.

```
Paths.get(...)
```

creates Path objects.

Equivalent to

```
Path.of(...)
```

For Java 21 Oracle expects you to know both.

---

# 3. Files Class

The Files class contains static helper methods.

Oracle asks this class constantly.

---

## exists()

```java
Files.exists(path)
```

Returns boolean.

Does NOT throw exception.

---

## notExists()

Opposite.

---

## createFile()

```java
Files.createFile(path);
```

Creates file.

Throws

```
IOException
```

if already exists.

Oracle loves this.

---

## createDirectory()

Creates ONE directory.

Fails if parent missing.

Example

```
temp/docs
```

If temp doesn't exist

↓

IOException

---

## createDirectories()

Creates all missing directories.

Example

```
temp/docs/data
```

Everything gets created.

Oracle favorite.

---

## delete()

Deletes file.

Throws IOException if missing.

---

## deleteIfExists()

Returns

```
true
```

or

```
false
```

No exception if missing.

Huge Oracle favorite.

---

## copy()

```java
Files.copy(source,destination);
```

Throws exception if destination exists.

Need

```
StandardCopyOption.REPLACE_EXISTING
```

to overwrite.

---

## move()

Moves file.

Also used for rename.

---

## readString()

Java 11+

```java
String s = Files.readString(path);
```

Reads whole file.

---

## writeString()

```java
Files.writeString(path,"Hello");
```

Writes whole file.

---

## readAllLines()

Returns

```
List<String>
```

Oracle loves asking return types.

---

## readAllBytes()

Returns

```
byte[]
```

---

# Files.list()

Returns

```
Stream<Path>
```

Not List.

Example

```java
Files.list(path)
```

Oracle favorite.

---

# Files.walk()

Traverses directory recursively.

Returns

```
Stream<Path>
```

---

Example

```java
Files.walk(Path.of("docs"))
     .forEach(System.out::println);
```

Visits

```
docs

docs/a.txt

docs/b

docs/b/test.txt
```

---

# Files.find()

Searches using predicate.

Rare.

Know it exists.

---

# Files.lines()

Returns

```
Stream<String>
```

Very common Oracle question.

Example

```java
Files.lines(path)
```

NOT

```
List<String>
```

---

# 4. BufferedReader

Located in

```
java.io
```

Usually wrapped around

```
Files.newBufferedReader()
```

Example

```java
BufferedReader br =
    Files.newBufferedReader(path);
```

---

Reading

```java
String line;

while((line = br.readLine()) != null){

}
```

---

Important

readLine()

returns

```
String
```

or

```
null
```

NOT empty string.

Oracle favorite.

---

# BufferedWriter

Example

```java
BufferedWriter bw =
    Files.newBufferedWriter(path);
```

Writing

```java
bw.write("Hello");
bw.newLine();
bw.write("Java");
```

Need

```
close()
```

or

try-with-resources.

---

# Try-with-Resources

Oracle LOVES this.

Example

```java
try(BufferedReader br =
        Files.newBufferedReader(path)){

}
```

Automatically closes.

---

# File Attributes

---

## Files.size()

Returns

```
long
```

---

## Files.isDirectory()

Returns boolean.

---

## Files.isRegularFile()

True for normal file.

---

## Files.isHidden()

Throws IOException.

Oracle trap.

---

## Files.getLastModifiedTime()

Returns

```
FileTime
```

Not Date.

---

## Files.getOwner()

Returns

```
UserPrincipal
```

Rare.

---

## Files.getAttribute()

Generic attribute access.

Know it exists.

---

# Directory Traversal

---

## Files.list()

One level only.

```
docs

    a.txt

    b.txt
```

Visits only

```
a.txt

b.txt
```

---

## Files.walk()

Recursive.

Visits everything.

---

## Files.walk(path, depth)

Example

```java
Files.walk(path,2);
```

Maximum depth 2.

---

## Files.find()

Uses predicate.

---

# Absolute vs Relative Path

Relative

```
data.txt
```

Absolute

```
C:\Users\John\data.txt
```

Oracle loves

```
isAbsolute()
```

and

```
toAbsolutePath()
```

---

# Checked Exceptions

Many Files methods throw

```
IOException
```

Examples

```
createFile

copy

move

delete

newBufferedReader

newBufferedWriter

walk

list

lines
```

Need

```
try/catch
```

or

```
throws IOException
```

---

# Return Types (Very Important)

| Method                    | Returns        |
| ------------------------- | -------------- |
| Files.readAllLines()      | List<String>   |
| Files.readString()        | String         |
| Files.readAllBytes()      | byte[]         |
| Files.lines()             | Stream<String> |
| Files.list()              | Stream<Path>   |
| Files.walk()              | Stream<Path>   |
| Path.getFileName()        | Path           |
| Path.getParent()          | Path           |
| Files.size()              | long           |
| Files.exists()            | boolean        |
| Files.deleteIfExists()    | boolean        |
| BufferedReader.readLine() | String         |

---

# Oracle Favorite Certification Traps

## Trap 1

```java
Path p = Path.of("docs");

p.resolve("file.txt");

System.out.println(p);
```

Output

```
docs
```

Because Path is immutable.

---

## Trap 2

```java
Files.list(path);
```

Returns

```
Stream<Path>
```

NOT List<Path>

---

## Trap 3

```java
Files.lines(path);
```

Returns

```
Stream<String>
```

NOT List<String>

---

## Trap 4

```java
Files.readAllLines(path);
```

Returns

```
List<String>
```

---

## Trap 5

```java
BufferedReader.readLine()
```

Returns

```
null
```

at EOF.

NOT empty string.

---

## Trap 6

```java
Files.createDirectory()
```

Fails if parent missing.

Need

```
createDirectories()
```

---

## Trap 7

```java
delete()
```

Throws exception if file missing.

```
deleteIfExists()
```

returns false.

---

## Trap 8

```
Path.of()
```

and

```
Paths.get()
```

Both valid.

---

## Trap 9

```
Files.walk()
```

Recursive.

```
Files.list()
```

Not recursive.

---

## Trap 10

```
Path.getRoot()
```

Returns null for relative paths.

Oracle asks this surprisingly often.

---

# High-Probability Oracle-Style Practice Questions

## Question 1

What is the output?

```java
Path p = Path.of("docs");

p.resolve("file.txt");

System.out.println(p);
```

A)

```
docs/file.txt
```

B)

```
docs
```

C)

Compilation error

D)

Runtime exception

### Answer

✅ **B**

**Explanation:** `resolve()` returns a new `Path`; it does not modify the original.

---

## Question 2

Which method returns a `Stream<Path>`?

A)

```
Files.lines()
```

B)

```
Files.walk()
```

C)

```
Files.readAllLines()
```

D)

```
Files.readString()
```

### Answer

✅ **B**

**Explanation:** `Files.walk()` returns a recursive `Stream<Path>`. `Files.lines()` returns a `Stream<String>`.

---

## Question 3

Which method creates all missing parent directories?

A)

```
Files.createFile()
```

B)

```
Files.createDirectory()
```

C)

```
Files.createDirectories()
```

D)

```
Path.resolve()
```

### Answer

✅ **C**

---

## Question 4

What is the return type of `Files.readAllLines()`?

A)

```
String
```

B)

```
Stream<String>
```

C)

```
List<String>
```

D)

```
Path
```

### Answer

✅ **C**

---

## Question 5

What does `BufferedReader.readLine()` return at end-of-file?

A)

```
""
```

B)

```
null
```

C)

```
EOF
```

D)

Throws `IOException`

### Answer

✅ **B**

---

## Question 6

Which statement about `Files.deleteIfExists(path)` is correct?

A. It throws an exception if the file does not exist.

B. It returns `false` if the file does not exist.

C. It always returns `true`.

D. It returns the deleted `Path`.

### Answer

✅ **B**

---

## Question 7

Given:

```java
Path p = Path.of("a/../b");
System.out.println(p.normalize());
```

What is printed?

A. `a/../b`

B. `a/b`

C. `b`

D. Compilation error

### Answer

✅ **C**

---

# Last-Minute Exam Cheat Sheet

## Remember These APIs

| API                           | Key Fact                                  |
| ----------------------------- | ----------------------------------------- |
| `Path.of()`                   | Preferred way to create `Path`            |
| `Paths.get()`                 | Older but valid                           |
| `Path.resolve()`              | Returns new `Path`                        |
| `Path.normalize()`            | Removes `.` and `..`                      |
| `Path.isAbsolute()`           | Tests absolute path                       |
| `Files.exists()`              | Returns `boolean`, no exception           |
| `Files.createDirectory()`     | Creates one directory only                |
| `Files.createDirectories()`   | Creates missing parent directories        |
| `Files.deleteIfExists()`      | Returns `boolean`                         |
| `Files.readString()`          | Returns `String`                          |
| `Files.readAllLines()`        | Returns `List<String>`                    |
| `Files.lines()`               | Returns `Stream<String>`                  |
| `Files.list()`                | Returns `Stream<Path>` (one level)        |
| `Files.walk()`                | Returns `Stream<Path>` (recursive)        |
| `Files.size()`                | Returns `long`                            |
| `Files.isDirectory()`         | Returns `boolean`                         |
| `Files.getLastModifiedTime()` | Returns `FileTime`                        |
| `BufferedReader.readLine()`   | Returns `String` or `null`                |
| `BufferedWriter.newLine()`    | Writes a platform-specific line separator |

## Final 1Z0-830 Tips

- Memorize **return types** (`List<String>` vs `Stream<String>` vs `Stream<Path>`).
- Remember that **`Path` objects are immutable**—methods like `resolve()` and `normalize()` return new instances.
- Know which `Files` methods **throw `IOException`** (`createFile`, `copy`, `move`, `delete`, `list`, `walk`, `lines`, `newBufferedReader`, etc.).
- Distinguish **`createDirectory()`** (single level) from **`createDirectories()`** (creates all missing parents).
- Don't confuse **`Files.list()`** (non-recursive) with **`Files.walk()`** (recursive).
- Be comfortable identifying **absolute vs. relative paths**, and know that `getRoot()` returns `null` for relative paths.
- Expect Oracle to include code snippets with **try-with-resources**, **checked exceptions**, and subtle **immutability** traps. These are among the highest-probability questions in this module.
