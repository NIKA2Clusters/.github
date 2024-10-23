# NIKA2 cluster cosmology codes

(Updated October 2024)


## Project location 

Our project is in 

```bash
$RDS/bb667/cluster_counts/NIKA2Clusters
```

On DIRAC (Cambridge).

'''bash
/data2f-a/Workspace/moyer/cosmo/NIKA2Clusters
'''
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
