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


#### Why or why not.
