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

Clone `class_sz`:

```bash
git clone https://github.com/NIKA2Clusters/class_sz.git
```
Python virtual environment: 

```bash
mkdir python_environments
python3.11 -m venv python_environments/cluster_base_env
source python_environments/cluster_base_env/bin/activate
```

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



