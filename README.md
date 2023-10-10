# Valgrind Exercise Submission

### Vinay Lanka - 120417665

## Standard install via command-line
```bash
# Configure the project and generate a native build system:
  # Must re-run this command whenever any CMakeLists.txt file has been changed.
  cmake -S ./ -B build/
# Compile and build the project:
  # rebuild only files that are modified since the last build
  cmake --build build/
  # or rebuild everything from scracth
  cmake --build build/ --clean-first
  # to see verbose output, do:
  cmake --build build/ --verbose
# Run program:
  ./build/app/shell-app
# Clean
  cmake --build build/ --target clean
# Clean and start over:
  rm -rf build/
```

### Results

The valgrind output for both cases (before and after fixing the bugs) are present in the results directory as their text files. (valgrind_before_bugfix and valgrind_after_bugfix)

### Bugs fixed 

- Fixed the memory leak issue of not deallocating the dynamically allocated vector `readings`. Used the delete operator to fix the issue.

- Fixed the conditional jump based on uninitialised value `terminator`. Fixed it by initialising true for consistent behaviour.

### Extra Credit Questions

#### What happens when the executable is linked statically?  Does Valgrind still detect those same bugs?

When the executable is linked statically, Valgrind cannot detect all the bugs and behaves erratically, especially with memory checking and memory leaks.

When we link our executable statically via cmake, we observe a lot more conditional jump errors which do not belong to our program.

Also the memory leak error is no longer visible with a `All heap blocks were freed -- no leaks are possible` statement, which is fishy as the `total heap usage: 0 allocs, 0 frees, 0 bytes allocated` statement shows that there's no allocation when there clearly is memory allocated in the application.

#### Why or why not.

With the conditional jump based on uninitialised values, we see that most of these errors come from methods we cannot recognise `(libc_setup_tls, libc_init_first etc..)`.

The reason for this is not an issue with Valgrind but with the libraries linked in the static executable, a lot of which is not the program code. 
A lot of the libraries, including glibc are not clean according to Valgrind. If we link dynamically, valgrind can suppress a lot of these errors but when we link statically, valgrind cannot suppress them because of the bundled nature of static linkage.
A method to circumvent this is to write new suppressions or to link dynamically.

The second issue with the undetected memory leaks is due to the nature of how valgrind functions. 
Valgrind can only work if they replace certain functions, like `malloc, new, delete` with their own versions. By default, statically linked malloc functions are not replaced. 