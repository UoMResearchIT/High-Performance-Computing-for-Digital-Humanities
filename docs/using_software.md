# Accessing software

HPC systems tend to provide software packages for the users. The number can be quite large and
the software varied, especially on clusters where users come from different research backgrounds, such as bioinformatics, computer science, etc.

By default no software is preloaded and to use any of the software packages available one must _load_ them explicitly. This is driven
by the challenges presented by

* multiple versions
* dependencies
* incompatibilities between different versions and packages

!!! info
    There are two main different module systems, **Lmod** and **Environment modules**. You can check which your HPC system uses with the command `modules --version`.
    
    For Lmod this will return: `Modules based on Lua: Version 8.x`
    
    For Environment Modules this will return: `Modules Release 5.x`
    
    There are a few differences in how these two systems operate, we will highlight below where these will matter to you.

## Environment Modules

Environment modules provide a way for a dynamic modification of a user's environment and address some of the challenges listed above.
They are very useful in managing large numbers of applications and help to control multiple versions and the dependencies present.

Each modulefile contains information needed to set up your environment for the application.
In most cases it will alter, or define shell variables such as `PATH`, `LD_LIBRARY_PATH`, etc.

If there are specific dependencies, the modules will define and load them accordingly. For example, if a package relies on
a specific compiler, the module file will load that compiler for you.


### Available Modules

To list available modules on the system use `module avail`. After you run it you should see something similar to the following output:

```text
k1234567@erc-hpc-login1:~$ module avail

---------------------------------------------------------- /opt/amazon/modules/modulefiles -----------------------------------------------------------
libfabric-aws/2.4.0amzn1.0  openmpi/4.1.7  openmpi5/5.0.9amzn1  

----------------------------------------------------------- /usr/share/modules/modulefiles -----------------------------------------------------------
dot  miniforge3/26.3.2-3(latest)  module-git  module-info  modules  null  python/3.13.14  python/3.14.6(latest)  use.own  

------------------------------------------------------- /opt/intel/mpi/2021.17/etc/modulefiles -------------------------------------------------------
intelmpi/2021.17  
```

!!! info
    The software modules and version numbers you see will differ from the example shown here because every HPC system is set up for the different needs of their users.

You can run the above command yourself to see the full list.
Use the arrow keys to scroll up and down the list, and `q` to go back to the command line when you're done.

### Searching and Examining Modules

To find out more information about the module, use `module whatis`

```bash
module whatis openmpi/4.1.7
```

You should see information about the module

```text
openmpi/4.1.7: Sets up Open MPI v4.1.7 in your environment
```

For more detailed information about the module, use `module show`

```bash
module show openmpi/4.1.7
```

You will see technical information on how the module changes the environment

```text
-------------------------------------------------------------------
/opt/amazon/modules/modulefiles/openmpi/4.1.7:

module-whatis   {Sets up Open MPI v4.1.7 in your environment}
prepend-path    PATH /opt/amazon/openmpi/bin
prepend-path    LD_LIBRARY_PATH /opt/amazon/openmpi/lib
prepend-path    MANPATH /opt/amazon/openmpi/share/man
-------------------------------------------------------------------
```

The `module apropos` command can be used to search whatis information. Use this with your keyword of interest

```bash
module apropos mpi
```

This will show you all whatis information which contains that keyword

```bash
---------------------------------------------------------- /opt/amazon/modules/modulefiles -----------------------------------------------------------
       openmpi/4.1.7: Sets up Open MPI v4.1.7 in your environment
 openmpi5/5.0.9amzn1: Sets up Open MPI v5.0.9amzn1 in your environment

------------------------------------------------------- /opt/intel/mpi/2021.17/etc/modulefiles -------------------------------------------------------
    intelmpi/2021.17: Name: Intel(R) MPI Library
    intelmpi/2021.17: Version: modulefiles/2021.17
    intelmpi/2021.17: Description: Intel(R) MPI Library
    intelmpi/2021.17: URL: https://www.intel.com/content/www/us/en/developer/tools/oneapi/mpi-library.html
    intelmpi/2021.17: Dependencies: none
```


!!! Important
    The `spider` command described below is only available for `Lmod` module systems.

If you know the name, or parts of the name of the package that you want to use, you can search for it using `module spider`, e.g.

```text
module spider python

--------------------------------------------------------------------------------------------------------------------------------------------------
  python:
--------------------------------------------------------------------------------------------------------------------------------------------------
     Versions:
        python/3.11.6-gcc-11.4.0
        python/3.11.6-gcc-12.3.0
        python/3.11.6-gcc-13.2.0
     Other possible modules matches:
        py-meson-python  py-mysql-connector-python  py-python-dateutil

--------------------------------------------------------------------------------------------------------------------------------------------------
  To find other possible module matches execute:

      $ module -r spider '.*python.*'

--------------------------------------------------------------------------------------------------------------------------------------------------
  For detailed information about a specific "python" package (including how to load the modules) use the module's full name.
  Note that names that have a trailing (E) are extensions provided by other modules.
  For example:

     $ module spider python/3.11.6-gcc-13.2.0
--------------------------------------------------------------------------------------------------------------------------------------------------
```

### Using Modules

To load a module use `module load` command

```bash
module load python/3.11.6-gcc-13.2.0
```

The above command loads a specific version of the application/module. If the explicit version is omitted during the `module load` request,
i.e. `module load python`, the default version of the package will be loaded.
The default version is marked by `D` (for default) in the output of `module avail`.
It is better to load a specific version of the module rather than relying on the default versioning to avoid issues when the default version changes.

!!! tip
    You might see different module versions that provide the same version of the application, but with
    different dependency versions, e.g. different versions of gcc. This is helpful for compatibility with
    other software that requires a specific gcc version.

To list currently loaded modules use `module list`

```text
module list

Currently Loaded Modules:
  1) bzip2/1.0.8-gcc-13.2.0     7) gdbm/1.23-gcc-13.2.0       13) zstd/1.5.5-gcc-13.2.0                     19) sqlite/3.43.2-gcc-13.2.0
  2) libmd/1.0.4-gcc-13.2.0     8) libiconv/1.17-gcc-13.2.0   14) tar/1.34-gcc-13.2.0                       20) util-linux-uuid/2.38.1-gcc-13.2.0
  3) libbsd/0.11.7-gcc-13.2.0   9) xz/5.4.1-gcc-13.2.0        15) gettext/0.22.3-gcc-13.2.0-libxml2-2.10.3  21) python/3.11.6-gcc-13.2.0
  4) expat/2.5.0-gcc-13.2.0    10) zlib-ng/2.1.4-gcc-13.2.0   16) libffi/3.4.4-gcc-13.2.0
  5) ncurses/6.4-gcc-13.2.0    11) libxml2/2.10.3-gcc-13.2.0  17) libxcrypt/4.4.35-gcc-13.2.0
  6) readline/8.2-gcc-13.2.0   12) pigz/2.7-gcc-13.2.0        18) openssl/3.1.3-gcc-13.2.0
```

!!! important
    Although you can load the modules on the login nodes and they will be propagated to the scheduled jobs, you __should__ load them in your job script as some of the
    software is optimised for specific processor architectures. You will also avoid conflicts and missing modules (in case you forget to load them before submitting the job).

To remove, or unload a specific module use `module rm`

```bash
module rm python/3.11.6-gcc-13.2.0
```

This will also unload any dependent modules required by the module you are unloading.

!!! tip
    To remove __all__ loaded modules use `module purge`.


## Exercises - modules

To test your understanding of how to access software using modules, work through the exercises in [this section](exercises.md/#using-modules).

## Installing your own software

Although it's likely that many software packages are pre-installed on any HPC cluster, there is a chance that the package that you want to use
is not installed and cannot be accessed as a module.

You can of course build, or install your own packages. As you do not have root (administrative rights) on the nodes you won't be able to install them
into system locations, or use system package managers (`apt`, `dpkg`) to perform the installations.
You can however install into your own personal location, or the group shares - in essence anywhere you can write to. Build systems such
as `cmake` or `automake` will have appropriate switches to define custom installation locations.

Building and installing software can become complex, and is beyond the scope of this workshop.
However, we will discuss a few other approaches to accessing software that you might find useful:
Python virtual environments, Conda, and [Singularity](singularity.md).

Python virtual environments and Conda/Anaconda provide a way to extend functionality of an existing python installation and create isolated environments
that can be used to install, or upgrade specific packages.
Singularity is a tool for running [software containers](https://en.wikipedia.org/wiki/Containerization_(computing)).

### Python virtual environments

To create a Python virtual environment load the relevant python module and then execute `python3 -m venv envname` command replacing `envname` with the name
of the environment you want to create:

```text
k1234567@login1:~$ module load python/3.14.6
k1234567@login1:~$ python3 -m venv myenv
```

With this, Python has created the `myenv` directory which now contains the required base files.

!!! tip
    You can also specify the absolute/relative path to the environment if you do not want it created in the current directory.

You only need to create the environment once. Once it has been created you do not need to run the command again (unless you need deleted the current one, or wish to create another one).

To use the environment you have to activate it first by sourcing the activate script

```text
k1234567@login1:~$ source myenv/bin/activate
(myenv) k1234567@login1:~$
```

!!! important
    When activating the virtual environment you can use relative paths, but the activation has to be initiated from the right directory.
    Using full (absolute) path might be the safer alternative.

The name of the venv should be prepended to your prompt indicating that the environment is active. From now on you can use the environment:

* to install packages

    ```text
    (myenv) k1234567@login1:~$ pip install pytest
    ```
    ```text
    Collecting pytest
      Downloading pytest-9.1.1-py3-none-any.whl.metadata (7.6 kB)
    Collecting iniconfig>=1.0.1 (from pytest)
      Downloading iniconfig-2.3.0-py3-none-any.whl.metadata (2.5 kB)
    Collecting packaging>=22 (from pytest)
      Using cached packaging-26.2-py3-none-any.whl.metadata (3.5 kB)
    Collecting pluggy<2,>=1.5 (from pytest)
      Downloading pluggy-1.6.0-py3-none-any.whl.metadata (4.8 kB)
    Collecting pygments>=2.7.2 (from pytest)
      Downloading pygments-2.20.0-py3-none-any.whl.metadata (2.5 kB)
    Downloading pytest-9.1.1-py3-none-any.whl (386 kB)
    Downloading pluggy-1.6.0-py3-none-any.whl (20 kB)
    Downloading iniconfig-2.3.0-py3-none-any.whl (7.5 kB)
    Using cached packaging-26.2-py3-none-any.whl (100 kB)
    Downloading pygments-2.20.0-py3-none-any.whl (1.2 MB)
      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.2/1.2 MB 67.3 MB/s  0:00:00
    Installing collected packages: pygments, pluggy, packaging, iniconfig, pytest
    Successfully installed iniconfig-2.3.0 packaging-26.2 pluggy-1.6.0 pygments-2.20.0 pytest-9.1.1
    ```

* or to run python scripts

    ```text
    (myenv) k1234567@login1:~$ python /datasets/hpc_training/utils/helloworld.py
    ```
    ```text
    Hello World!
    ```

To deactivate the environment use `deactivate` command

```text
(myenv) k1234567@login1:~$ deactivate
k1234567@erc-hpc-login1:~$
```

You will see that the environment name has disappeared from the shell prompt.

??? note "Conda virtual environments"
    Conda is another tool for creating python-related virtual environments.
    Conda is often available via `anaconda3` or `miniforge3` modules.

    To use Conda, first load the module:

    ```text
    module load miniforge3/26.3.2-3
    ```

    You can then create a virtual environment and specify the packages you want to install into that environment.
    For example, to create an environment with a specific version of Python:

    ```text
    conda create --name python39-env python=3.9
    ```

    The environment can be activated by running `conda activate` with the name of the env

    ```text
    k1234567@login1:~$ conda activate python39-env
    (python39-env) k1234567@login1:~$
    ```

    and deactivated by running `conda deactivate`

    ```text
    (python39-env) k1234567@login1:~$ conda deactivate
    k1234567@login1:~$
    ```

    As with Python virtualenvs, note that the name of the active Conda environment is prepended to your prompt when the environment is active.

    !!! tip
        To prevent activation of the Conda base environment by default, which may conflict with other modules/packages, run
        `conda config --set auto_activate_base false`

#### Exercises - virtual environments

To practice using Python virtual environments, work through the exercises in [this section](exercises.md/#python-virtual-environments).
