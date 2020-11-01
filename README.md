# python-console-scripts
Example repo for specifying console scripts in Python.

## setuptools

[setuptools](https://setuptools.readthedocs.io/en/latest/userguide/entry_point.html) 
allows the specification of *console_scripts* entry points, which get turned into 
executable scripts when installed via *pip*.

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
  ./venv/bin/msdp-hello --help
  ```
