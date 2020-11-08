# python-console-scripts (complex dependencies)
Example repo for specifying console scripts in Python.

The example code uses Java using [docker-banner-gen](https://pypi.org/project/docker-banner-gen/) 
to display text in the console but also has a dependency for [numpy](https://pypi.org/project/numpy/).


## Testing

* set up a virtual environment

  ```
  virtualenv -p /usr/bin/python3.7 venv
  ```

* install the code (from within the directory of this README)

  ```
  ./venv/bin/pip install .
  ```

* and now you can run the [hello.py](src/msdp/hello.py) script using:

  ```
  ./venv/bin/msdp-hello --text "Hello World!"
  ```

* you can display the script's help using:

  ```
  ./venv/bin/msdp-hello --help
  ```

## Linux packaging

### stdeb (Debian)

Using [stdeb](https://github.com/astraw/stdeb), you can build Debian packages 
(requires `python3-all` and `python3-stdeb` packages to be installed):

* clean up previous build

  ```commandline
  rm -f *.tar.gz && rm -Rf deb_dist
  ```

* build package

  ```commandline
  python3 setup.py --command-packages=stdeb.command bdist_deb
  ```
* print **information** on package:

  ```commandline
  dpkg -I deb_dist/python3-mysuperduperproject_0.0.1-1_all.deb
  ```
  
  should output something like this:
  
  ```commandline
   new Debian package, version 2.0.
   size 2952 bytes: control archive=1096 bytes.
       317 bytes,    10 lines      control              
       877 bytes,     9 lines      md5sums              
       179 bytes,     9 lines   *  postinst             #!/bin/sh
       429 bytes,    12 lines   *  prerm                #!/bin/sh
   Package: python3-mysuperduperproject
   Source: mysuperduperproject
   Version: 0.0.1-1
   Architecture: all
   Maintainer: Peter Reutemann <fracpete@gmail.com>
   Installed-Size: 21
   Depends: python3-docker-banner-gen, python3-numpy, python3:any (>= 3.3.2-2~)
   Section: python
   Priority: optional
   Description: My super duper Project.
  ```

  **Note:** The *python3-docker-banner-gen* got added as dependency even though
  this Python package is only available from pypi and not as Debian package.
  
* when simulating an **apt install** with:
  
  ```commandline
  gdebi --apt-line deb_dist/python3-mysuperduperproject_0.0.1-1_all.deb
  ```
  
  You get the following output (with unsatisfiable dependency *python3-docker-banner-gen*):
  
  ```commandline
  Reading package lists... Done
  Building dependency tree        
  Reading state information... Done
  Reading state information... Done
  This package is uninstallable
  Dependency is not satisfiable: python3-docker-banner-gen
  ```

  You will need to install this package manually (if even available as Debian package).
