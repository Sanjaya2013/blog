## JVM Bytecode: Overview

As a Java developer, you probably don't need to worry about the workings of the beast that is the *JVM.*<br>
However, as someone who's explored it, I find it fun to tinker around with libraries
like [ASM](https://asm.ow2.io/), and as such, want to pass my knowledge onto you, the reader.

Welcome to... JVM bytecode.&nbsp;&nbsp;*cue anime opening*

---

### Disclaimer!

You will be expected to know:
- What the *%$!@* Java is, otherwise you shouldn't even be here...
- What the JVM is
- What compilers are

### To Brass Tacks <!-- Wink wink wink -->

Throughout the course of this series, we'll be going through each major concept within a JVM class file.

1. Names and Descriptors
2. The Constant Pool
3. Class Basics
4. Attributes
5. The Instruction Set

If the next part isn't out yet, read the [JVMS](https://docs.oracle.com/javase/specs/jvms/se17/html/index.html) like everyone else, nerd!

#### Common Tools for Bytecode Engineering

- `javap`: The standard CLI-based Java disassembler which comes with the JDK. I wouldn't recommend it as it is painful to use for large class files.
- [`Fernflower`](https://github.com/fesh0r/fernflower): A CLI-based analytical decompiler. Has a few small problems but is generally very accurate.
- [`Krakatau`](https://github.com/Storyyeller/Krakatau): A CLI-based decompiler, assembler, and disassembler tool. Cons: It's written i̸n̶ P̶̗͊y̷͈͛t̸̟̾h̶̢̎o̷͉̓n̴͍̍.
- [`JD-GUI`](https://java-decompiler.github.io): A GUI-based decompiler! 🙏 Finally.
Also, it has an Eclipse plugin but who cares?
- [`jclasslib`](https://github.com/ingokegel/jclasslib): A GUI-based disassembler, gives you a lot of low-level control and looks quite nice.
Also, it has an IntelliJ plugin and *that* is cool.
- [`Bytecode Viewer`](https://bytecodeviewer.com): ...I'll just let the description speak for itself:
"Six different Java decompilers, two Bytecode editors, a Java compiler, plugins, searching, supports loading from Classes, Jars, Android APKs and more."

---

I will also be releasing *related* posts on this [blog](https://blog.maow.xyz) that
go into some interesting concepts like instrumentation/attaching, classloading, and the ObjectWeb ASM library.<br>
Yeah, I'm aware that I'm *kind of* a mad scientist. Just wait till I start talking about `com.sun.tools.javac`.
