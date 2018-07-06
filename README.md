# UnitTest-cpp
The snapshot of UnitTest++ we use for our test environment

# IMPORTANT NOTE:
If you are just wanting UnitTest++ this is **NOT** the place to get it. This version is only here because we need a consistent version to test against across all of our projects, and the authors of UnitTest++ are kind enough to allow redistribution. So, if you want to get UnitTest++ for your own project, outside our lab, look here:

```
https://github.com/unittest-cpp/unittest-cpp
```
Those guys deserve all the credit for this great piece of software. However, if you want to build and run the unit tests we include with our own software, then this is the place for you. All repositories have been designed such that you should issue e.g. all clone commands from the same directory. For example,

```
git clone https://github.com/ComputationalBioLab/UnitTest-cpp.git
git clone https://github.com/ComputationalBioLab/Unfit.git
```

You don't have to, but if you choose a different folder structure you will have to edit the build parameters (Build Options in CodeBlocks) to correct the include search directories and paths to any included libraries.

