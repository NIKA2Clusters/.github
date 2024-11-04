# NIKA2 cluster cosmology codes

(Updated October 2024)


## Project location 

Our project is in 

```bash
$RDS/bb667/cluster_counts/NIKA2Clusters
```

On DIRAC (Cambridge).

```bash
/data2f-a/Workspace/moyer/cosmo
```
on nika2e (Grenoble)

## Installation instructions

### Python version

We work with Python 3.11. 

### Dirac (Cambridge)

We created an environment variable: 

```bash
export GOTONIKA2=/home/bb667/rds/rds-dirac-dp002/AdvACT/bb667/cluster_counts/NIKA2Clusters
```

Source environment with (see below to create it):

```bash
source $GOTONIKA2/python_environments/cluster_base_env/bin/activate
```

Clone `class_sz`:

```bash
git clone https://github.com/NIKA2Clusters/class_sz.git
```
Python virtual environment: 

```bash
mkdir python_environments
python3.11 -m venv python_environments/cluster_base_env
source $GOTONIKA/python_environments/cluster_base_env/bin/activate
```

#### Dependencies

We need to install a few dependencies. Once the environment has been sourced. 

```bash
python -m pip install "mpi4py>=3" --upgrade --no-binary :all:
```

After this command has run, you should be able to do:

```bash
mpirun -n 2 python -c "from mpi4py import MPI, __version__; print(__version__ if MPI.COMM_WORLD.Get_rank() else '')"
```

which should print the mpirun version. 

#### Installation of class_sz in development mode

```bash
git clone https://github.com/CLASS-SZ/get_cosmopower_emus.git
cd get_cosmopower_emus
pip install -e .
cd ..
git clone https://github.com/CLASS-SZ/class_sz_data.git
cd class_sz_data
pip install -e .
cd ..
cd class_sz/class-sz/python
git clone https://github.com/CLASS-SZ/classy_szfast
cd ..
chmod +x select_makefile.sh
./select_makefile.sh
```

We don't need to download emulator data if they are already on the system. Check content of `$PATH_TO_CLASS_SZ_DATA`.


And then we are ready to make (from inside `cd $GOTONIKA2/class_sz/class-sz`): 

```bash
make clean
make -j
```

and finally, we need to tell our environment where `classy_szfast` is: 

```bash
 export PYTHONPATH=$GOTONIKA2/class_sz/class-sz/python/classy_szfast:$PYTHONPATH
```
Note that we can add this line to our `$GOTONIKA2/python_environments/cluster_base_env/bin/activate`.

That should work fine. 








### nika2e (Grenoble)

clone `class_sz`

```bash
git clone https://github.com/NIKA2Clusters/class_sz.git
```
Python virtual environment:
```bash
mkdir env
python3 -m venv env/cluster_base_env
source env/cluster_base_env/bin/activate
```
python version : 3.10.12

class_sz installation : 
```bash
git clone https://github.com/CLASS-SZ/class_sz
git clone https://github.com/CLASS-SZ/get_cosmopower_emus.git
cd get_cosmopower_emus
pip install -e .
cd ..
git clone https://github.com/CLASS-SZ/class_sz_data.git
cd class_sz_data
pip install -e .
cd ..
cd class_sz/class-sz/python
git clone https://github.com/CLASS-SZ/classy_szfast
cd ..
chmod +x select_makefile.sh
./select_makefile.sh
chmod +x download_emulators.sh
./download_emulators.sh
make clean
make -j
```
an error occured : missing module named `Cython'

```bash
pip install Cython
pip install numpy
pip install six
make clean
make -j
export PYTHONPATH=$(pwd)/python/classy_szfast:$PYTHONPATH
```
installation went to the end`

modification of `/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/bin/activate`
addition of 
```bash
PATH_TO_CLASS_SZ_DATA="/data2f-a/Workspace/moyer/cosmo/class_sz/class_sz_data_directory" # rajout AMA
export PATH_TO_CLASS_SZ_DATA
export PYTHONPATH="/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy_szfast:"
```
clone `SOLikeT`

```bash
git clone https://github.com/simonsobs/soliket
cd soliket
git checkout dev-clusters-both-classy_sz
pip install -e .
```

installing cobaya 
```bash
python -m pip install cobaya --upgrade
```

test of yaml file in `/soliket/chains/test_unibnned_classy_sz_evaluate.input.yaml`
change of lines 24 25 26 and 27 to match my path, but I didn't change line 54 which is `data/advact/DR5CosmoSims/sim-kit_NemoCCL_A10tSZ_DR5White_ACT-DR5_tenToA0Tuned/NemoCCL_A10tSZ_DR5White_ACT-DR5_tenToA0Tuned/`
worked correctly

went to ipython3 try `import classy_sz` an error occured:
```bash
import classy_sz

/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy.pyx in init classy_sz()

/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy.pyx in classy_sz()

/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy_szfast/classy_szfast/__init__.py in <module>
----> 1 from classy_szfast.classy_szfast import Class_szfast
      2 from .config import *
      3 from .utils import *
      4 from .cosmopower import *
      5 from .pks_and_sigmas import *

/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy_szfast/classy_szfast/classy_szfast.py in <module>
      1 from .utils import *
----> 2 from .config import *
      3 import numpy as np
      4 from .cosmopower import *
      5 from .pks_and_sigmas import *

/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy_szfast/classy_szfast/config.py in <module>
      8     return os.getenv('PATH_TO_CLASS_SZ_DATA')
      9 
---> 10 path_to_class_sz_data = get_cosmopower_path()
     11 class_sz_data.get_data_from_class_sz_repo(path_to_class_sz_data)

/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy_szfast/classy_szfast/config.py in get_cosmopower_path()
      5 
      6 def get_cosmopower_path():
----> 7     get_cosmopower_emus.set()
      8     return os.getenv('PATH_TO_CLASS_SZ_DATA')
      9 

AttributeError: module 'get_cosmopower_emus' has no attribute 'set'
```
change `/class_sz/class-sz/python/classy_szfast/classy_szfast/config.py` 
commented the function `get_cosmopower_path()` and adding line: `path_to_class_sz_data="/data2f-a/Workspace/moyer/cosmo/class_sz/class_sz_data_directory"` 

test of running one of my old file :

```bash

(cluster_base_env) moyer@lpsc-nika2e:/data2f-a/Workspace/moyer/cosmo$ python -m cobaya run cosmopower_quick.yaml -f
[output] Output to be read-from/written-into folder '/data2f-a/Workspace/moyer/cosmo/chains/first_test', with prefix 'chains'
[input] *ERROR* There was a problem when importing 'classy_szfast.classy_sz.classy_sz':
[input] *ERROR* Failed to get defaults for component or class 'classy_szfast.classy_sz.classy_sz' [module 'class_sz_data' has no attribute 'get_data_from_class_sz_repo']
[exception handler] ---------------------------------------

Traceback (most recent call last):
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/input.py", line 237, in get_default_info
    cls = get_component_class(component_or_class, kind, component_path, class_name,
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/component.py", line 680, in get_component_class
    check_if_ComponentNotFoundError_and_raise(
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/component.py", line 627, in check_if_ComponentNotFoundError_and_raise
    raise _excpt
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/component.py", line 674, in get_component_class
    return check_kind_and_return(return_class(module_name,
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/component.py", line 587, in return_class
    _module: Any = load_module(_module_name, path=component_path, **kwargs)
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/tools.py", line 191, in load_module
    module = import_module(name, package=package)
  File "/usr/lib/python3.10/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1050, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1027, in _find_and_load
  File "<frozen importlib._bootstrap>", line 992, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "<frozen importlib._bootstrap>", line 1050, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1027, in _find_and_load
  File "<frozen importlib._bootstrap>", line 1006, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 688, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 883, in exec_module
  File "<frozen importlib._bootstrap>", line 241, in _call_with_frames_removed
  File "/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy_szfast/classy_szfast/__init__.py", line 1, in <module>
    from classy_szfast.classy_szfast import Class_szfast
  File "/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy_szfast/classy_szfast/classy_szfast.py", line 2, in <module>
    from .config import *
  File "/data2f-a/Workspace/moyer/cosmo/class_sz/class-sz/python/classy_szfast/classy_szfast/config.py", line 13, in <module>
    class_sz_data.get_data_from_class_sz_repo(path_to_class_sz_data)
AttributeError: module 'class_sz_data' has no attribute 'get_data_from_class_sz_repo'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/usr/lib/python3.10/runpy.py", line 196, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "/usr/lib/python3.10/runpy.py", line 86, in _run_code
    exec(code, run_globals)
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/__main__.py", line 47, in <module>
    run_command()
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/__main__.py", line 37, in run_command
    getattr(import_module(module), func)()
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/run.py", line 198, in run_script
    run(info, **arguments.__dict__)
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/run.py", line 97, in run
    updated_info = update_info(info)
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/input.py", line 317, in update_info
    default_class_info, annotations = get_default_info(
  File "/data2f-a/Workspace/moyer/cosmo/env/cluster_base_env/lib/python3.10/site-packages/cobaya/input.py", line 249, in get_default_info
    raise LoggedError(logger,
cobaya.log.LoggedError: Failed to get defaults for component or class 'classy_szfast.classy_sz.classy_sz' [module 'class_sz_data' has no attribute 'get_data_from_class_sz_repo']
-------------------------------------------------------------

(cluster_base_env) moyer@lpsc-nika2e:/data2f-a/Workspace/moyer/cosmo$ 
```

change of the file `/class_sz/class-sz/python/classy_szfast/classy_szfast/config.py`

commented the line `class_sz_data.get_data_from_class_sz_repo(path_to_class_sz_data)`
after commentng this line, one of my old yaml file succeed to run but after 1h30 still in the burning phase



