# Steps to create package using Eclipse CDT

The package will be created for the library

```bash
# ensure EclipseCDT is in the path
$ git clone https://github.com/memsharded/conan_cdt
$ cd conan_cdt
$ conan test_package -s build_type=Debug -s compiler=gcc -s compiler.version=4.9 -s compiler.libcxx=libstdc++
$ conan search
> Existing package recipes:

hello_statlib/1.0.0@user/testing
$ conan search hello_statlib/1.0.0@user/testing
> Existing packages for recipe hello_statlib/1.0.0@user/testing:

    Package_ID: ca93456834c0cf87814b88b9f819760f726370e4
        [settings]
            build_type: Debug
            compiler: gcc
            compiler.libcxx: libstdc++
            compiler.version: 4.9
            os: Windows
        outdated from recipe: False

$ conan remote add myremote <your-remote-url>
$ conan upload hello_statlib/1.0.0@user/testing --all -r=myremote
```

# How to consume the ``hello_statlib/1.0.0@user/testing`` package in the hello_appl

Right now it is not possible, as Eclipse CDT doesn't have a way to automatically define:
- Include paths, preprocessor directives might be possible
- Library names not possible
- Libray paths not possible
- C/CXX flags not possible
- Link flags not possible



# Conan.io "Hello World" work-flow example of using Eclipse CDT
Conan example of Eclipse CDT projects by running headless Eclipse from the Conan build() method by creating a static library and then using that library in an application.


## Library Layout in project root directory: hello_lib
```
hello_lib
├───bin
├───inc
├───src
├───workspace
│   └───hello_statlib
```
* *conanfile.py* in root directory **hello_lib**
* *hello_statlib.a* static library artefact will be build by Eclipse in **bin**.
* *hello.h* is in **inc**
* *hello.cpp* is in **src**
* The Eclipse project(1..n) files are under directory **workspace/hello_statlib**.



## Application Layout in project root directory: hello_appl
```
hello_appl
├───bin
├───inc
├───lib
├───src
├───workspace
│   └───hello
```
* *conanfile.py* in root directory **hello_appl**
* *hello.exe* will be build  by Eclipse in **bin**
* *hello.h* to be place by conan in **inc**
* *hello_statlib.a* static library artefact to be place by Conan in **lib**.
* *main.cpp* is in **src**
* The Eclipse project(1..n) files are under directory **workspace/hello**.

## Work-flow on high-level per project
### hello_lib
* Project hello_statlib runs in the *build()* method as headless Eclipse and create *hello_statlib.a* in the **bin** directory.
* Conan have to package the lib *hello_statlib.a*, header *hello.h* and Eclipse project file from the **bin**, **inc** and **workspace/hello_statlib***directories to remote store.

### hello_appl
* Conan have to place the lib *hello_statlib.a* and header *hello.h* in the **lib** and **inc** directories from the remote store.
* Project hello runs in the *build()* method as headless Eclipse and create the appl. *hello.exe* in the **bin** directory.
* Conan have to package the *hello.exe* and Eclipse project file to remote store.


## Work-flow in detail
TODO