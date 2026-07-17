Absolutely. For the **Java SE 21 Developer Professional (1Z0-830)** exam, **I/O & Serialization** is not one of the largest objectives, but Oracle loves testing it because it contains many small rules that are easy to confuse.

The exam is unlikely to ask you to build a large file processing application. Instead, expect questions like:

- Which method compiles?
- Which exception must be handled?
- Which file operation succeeds?
- What happens after serialization?
- Which fields are serialized?
- Which code throws an exception?
- Which class can be serialized?

---

# 📂 14. I/O & Serialization (Oracle Exam Guide)

---

# Part 1 – File API (`java.nio.file`)

## Oracle strongly prefers NIO over old java.io.File

The modern API is in

```java
java.nio.file
```

Main classes:

```
Path
Paths
Files
```

The old

```java
File
```

still exists but appears much less frequently.

---

# Path

Represents a file or directory location.

Creating Paths:

```java
Path p1 = Path.of("notes.txt");
Path p2 = Path.of("/home/user/test.txt");
Path p3 = Path.of("dir", "subdir", "file.txt");
```

Older style:

```java
Paths.get(...)
```

still works.

Example

```java
Path p = Paths.get("test.txt");
```

Both may appear on the exam.

---

## Path is NOT the file itself

This is a favorite Oracle trick.

This

```java
Path p = Path.of("abc.txt");
```

does NOT create a file.

It only creates a Path object.

To create the actual file:

```java
Files.createFile(p);
```

---

# Files class

Almost every useful operation is static.

```java
Files.exists(path)
Files.copy(...)
Files.move(...)
Files.delete(...)
Files.readString(...)
Files.writeString(...)
Files.readAllLines(...)
Files.createDirectory(...)
Files.createDirectories(...)
```

Notice everything is

```
Files.method(...)
```

---

# Checking existence

```java
Path p = Path.of("notes.txt");

Files.exists(p);
```

Returns

```
true
false
```

No exception.

---

# Checking if regular file

```java
Files.isRegularFile(path)
```

Directory?

```java
Files.isDirectory(path)
```

---

# Create File

```java
Files.createFile(path);
```

Throws

```
IOException
```

if

- already exists
- parent directory missing

---

# Create Directory

```java
Files.createDirectory(path);
```

Creates ONE directory only.

Example

```
temp/
```

Works.

But

```
temp/one/two
```

fails if

```
temp
```

doesn't exist.

---

# createDirectories()

Oracle LOVES this.

```java
Files.createDirectories(path);
```

Creates every missing directory.

```
temp/
    one/
       two/
```

Everything created automatically.

---

## Oracle Favorite

Difference

```
createDirectory()
```

vs

```
createDirectories()
```

Know it perfectly.

---

# Delete

```java
Files.delete(path);
```

Throws exception if

- file missing
- directory not empty

---

# deleteIfExists()

```java
Files.deleteIfExists(path);
```

Returns

```
true
false
```

instead of exception for missing file.

---

# Copy

```java
Files.copy(source, target);
```

Throws exception if target already exists.

Overwrite?

```java
Files.copy(
    source,
    target,
    StandardCopyOption.REPLACE_EXISTING
);
```

---

# Move

```java
Files.move(source, target);
```

Can rename

```
a.txt
```

to

```
b.txt
```

or move directories.

---

# Read entire file

```java
String text = Files.readString(path);
```

Simple.

Throws

```
IOException
```

---

# Write String

```java
Files.writeString(path, "Hello");
```

Creates file if needed.

Overwrites existing contents.

---

Append

```java
Files.writeString(
    path,
    "Hello",
    StandardOpenOption.APPEND
);
```

---

# Read All Lines

```java
List<String> list =
    Files.readAllLines(path);
```

Returns

```
List<String>
```

---

# Write Lines

```java
Files.write(path, list);
```

---

# Listing Directory

```java
Files.list(path)
```

Returns

```
Stream<Path>
```

Oracle likes this because Stream questions can be mixed in.

---

# Resolving Paths

```java
Path p =
    Path.of("/home");

Path result =
    p.resolve("test.txt");
```

Result

```
/home/test.txt
```

---

# resolveSibling()

```java
Path.of("/a/b/c.txt")
```

Sibling

```java
.resolveSibling("x.txt")
```

Result

```
/a/b/x.txt
```

---

# normalize()

Removes

```
.
..
```

Example

```
a/b/../c
```

becomes

```
a/c
```

---

# Absolute Path

```java
path.toAbsolutePath();
```

---

# File Name

```java
path.getFileName();
```

---

# Parent

```java
path.getParent();
```

---

# Root

```java
path.getRoot();
```

---

# Common Path Methods

| Method           | Returns       |
| ---------------- | ------------- |
| getFileName()    | last element  |
| getParent()      | parent        |
| getRoot()        | root          |
| resolve()        | append path   |
| normalize()      | remove ..     |
| toAbsolutePath() | absolute path |

---

# Checked Exceptions

Almost every Files operation throws

```java
IOException
```

Oracle LOVES asking

"What happens?"

Correct answer:

```
Compilation fails
```

because exception isn't handled.

Example

```java
Files.createFile(path);
```

without

```java
try/catch
```

↓

Compilation error.

---

# Serialization

Serialization means

```
Object
↓

Bytes

↓

File
```

Deserialization

```
Bytes

↓

Object
```

---

# Serializable

A class must implement

```java
Serializable
```

Example

```java
class Dog implements Serializable
{
}
```

Notice

No methods.

Marker interface.

---

# Marker Interface

Contains

```
ZERO methods
```

Oracle loves asking this.

Serializable

```
✔ marker interface
```

---

# ObjectOutputStream

Writing

```java
ObjectOutputStream out =
    new ObjectOutputStream(
        Files.newOutputStream(path)
    );

out.writeObject(obj);
```

---

# ObjectInputStream

Reading

```java
ObjectInputStream in =
    new ObjectInputStream(
        Files.newInputStream(path)
    );

Dog d =
    (Dog) in.readObject();
```

Notice

```
readObject()
```

returns

```
Object
```

Need cast.

---

# serialVersionUID

May appear.

```java
private static final long serialVersionUID = 1L;
```

Controls serialization compatibility.

Oracle may ask

"Is it required?"

Answer

No.

Compiler generates one.

But recommended.

---

# What gets serialized?

Example

```java
class Dog implements Serializable
{
    String name;
    int age;
}
```

Both fields saved.

---

# transient

Favorite Oracle topic.

```java
transient String password;
```

Transient fields

↓

NOT serialized.

After deserialization

↓

Default value.

---

Example

```java
class User implements Serializable
{
    String username;

    transient String password;
}
```

Before serialization

```
username = john
password = secret
```

After deserialization

```
username = john
password = null
```

---

Primitive example

```java
transient int age;
```

After deserialization

```
0
```

---

# static fields

Oracle LOVES this.

Static fields belong to class.

NOT serialized.

Example

```java
class Dog
implements Serializable
{
    static int counter;

    String name;
}
```

After deserialization

```
counter
```

comes from current JVM

NOT file.

---

# final fields

Can be serialized.

No problem.

Example

```java
final String id;
```

Gets serialized normally.

---

# Constructors

Another favorite.

During deserialization

```
Serializable constructor

↓

NOT executed.
```

Huge exam trap.

Only

```
first non-serializable superclass constructor
```

runs.

---

Example

```java
class Animal
{
    Animal()
    {
        System.out.println("Animal");
    }
}

class Dog extends Animal
implements Serializable
{
    Dog()
    {
        System.out.println("Dog");
    }
}
```

After deserialization

Output

```
Animal
```

NOT

```
Dog
```

Oracle asks this repeatedly.

---

# If superclass is NOT Serializable

Superclass constructor executes.

Subclass constructor does NOT.

---

# If superclass IS Serializable

Neither constructor executes.

---

# Not Serializable

If object doesn't implement Serializable

```
NotSerializableException
```

Runtime exception?

No.

Actually

```
IOException subclass
```

Checked hierarchy.

---

# Object Graph

Oracle loves this.

```java
class Dog
implements Serializable
{
    Owner owner;
}
```

If

```
Owner
```

does NOT implement Serializable

↓

```
NotSerializableException
```

Entire serialization fails.

Every referenced object must also be serializable.

---

# Serialization Summary

| Field     | Serialized? |
| --------- | ----------- |
| instance  | ✔           |
| static    | ❌          |
| transient | ❌          |
| final     | ✔           |

---

# Oracle Favorite Questions

---

## Question 1

```java
Path p = Path.of("test.txt");
```

What happens?

A Creates file

B Creates directory

C Creates Path object only

D Throws exception

---

Answer

✅ C

---

Explanation

`Path.of()` only creates a `Path` object. It does not access or create the file system.

---

## Question 2

Which method creates missing parent directories?

A createDirectory()

B createDirectories()

C mkdir()

D makeDirectory()

---

Answer

✅ B

---

---

## Question 3

```java
Files.readString(path);
```

Throws

A IOException

B RuntimeException

C IllegalStateException

D FileNotFoundException only

---

Answer

✅ A

---

---

## Question 4

```java
class Person
implements Serializable
{
    transient int age;
}
```

After deserialization

```
age =
```

A null

B -1

C 0

D original value

---

Answer

✅ C

---

---

## Question 5

```java
class User
implements Serializable
{
    static int counter=10;
}
```

Is counter serialized?

A Yes

B Only if final

C No

D Only if transient

---

Answer

✅ C

---

---

## Question 6

Which fields are serialized?

```java
String name;

transient String password;

static int count;

final int id;
```

A name

B password

C count

D id

---

Answer

✅ A and D

---

---

## Question 7

Serializable is

A Functional Interface

B Annotation

C Marker Interface

D Abstract class

---

Answer

✅ C

---

---

## Question 8

Which method returns `List<String>`?

A read()

B readLines()

C readAllLines()

D readString()

---

Answer

✅ C

---

---

## Question 9

What is returned?

```java
Path.of("/home")
    .resolve("a.txt");
```

A

```
/a.txt
```

B

```
/home/a.txt
```

C

```
home/a.txt
```

D exception

---

Answer

✅ B

---

---

## Question 10

Which exception occurs when a referenced object is not serializable?

A IllegalArgumentException

B NotSerializableException

C ClassCastException

D InvalidObjectException

---

Answer

✅ B

---

# Oracle Certification Traps

## Trap 1

```java
Path.of(...)
```

does NOT create files.

---

## Trap 2

Every `Files.*` operation usually throws

```
IOException
```

Unchecked?

No.

Checked.

---

## Trap 3

```
createDirectory()
```

≠

```
createDirectories()
```

---

## Trap 4

`Serializable`

has

```
NO methods
```

---

## Trap 5

`transient`

means

```
default value after deserialization
```

not `null` for primitives.

---

## Trap 6

`static`

fields are never serialized.

---

## Trap 7

`final`

fields **are** serialized.

---

## Trap 8

Constructors of serializable classes are **not executed** during deserialization.

---

## Trap 9

Every object in the object graph must be serializable, or serialization fails with `NotSerializableException`.

---

## Trap 10

`readObject()` returns `Object`, so an explicit cast is required.

---

# High-Probability 1Z0-830 Exam Checklist

Before taking the exam, you should be able to answer **yes** to all of these:

- ✅ Know the difference between `Path`, `Paths`, and `Files`.
- ✅ Know that `Path.of()` creates only a path representation, not a file.
- ✅ Know common `Files` methods (`exists`, `createFile`, `createDirectory`, `createDirectories`, `copy`, `move`, `delete`, `readString`, `writeString`, `readAllLines`, `list`).
- ✅ Know that most `Files` operations throw the checked exception `IOException`.
- ✅ Understand `Path` methods: `resolve()`, `resolveSibling()`, `normalize()`, `toAbsolutePath()`, `getFileName()`, `getParent()`, and `getRoot()`.
- ✅ Know that `Serializable` is a **marker interface**.
- ✅ Understand exactly which fields are serialized (`instance` and `final`) and which are not (`static` and `transient`).
- ✅ Know that `transient` fields are restored to their **default values** after deserialization.
- ✅ Know that constructors of serializable classes are **not invoked** during deserialization.
- ✅ Know that `readObject()` returns `Object` and that every object in the serialized object graph must also implement `Serializable`. These are among Oracle's favorite certification questions.
