<p align="center">
<img src="img/Makefile.png" width="300px">
</p>

The make program uses the makefile data base and the last-modification times of the files to decide which of the files need to be updated.

**Concequences of updating files**
+ When make recompiles the editor, each changed C source file must be recompiled.  
+ If a header file has changed, each C source file that includes the header file must be recompiled to be safe. Each compilation produces an object file corresponding to the source file.  
+ Finally, if any source file has been recompiled, all the object files, whether newly made or saved from previous compilations, must be linked together to produce the new executable editor.

**What does the linker do ?**
<details>
<summary>Understanding the compilation process</summary>
Content.
</details>

#Resources
[Apparently all I need to know is in here.](https://www.gnu.org/software/make/manual/make.html)  
I might die.