## 1. Overview

This tutorial provides a simple, reproducible workflow for running a first simulation with the **Pencil Code**.  
It is intended for beginners who want to understand how to download, configure, compile, and run a basic example.

The example used here is the **1D Jeans instability test case**.

---

## 2. Download the Pencil Code

The Pencil Code is an open-source numerical code written mainly in **Fortran**, available under the **GNU General Public License (GPL)**.

Official website:  
http://pencil-code.org/

---

### 2.1 Downloading the Code

You can download the latest version using **SVN** or **Git**.

#### Using SVN

Run the following command to check out the code:

```bash
svn checkout https://github.com/pencil-code/pencil-code/trunk/ pencil-code
```
#### Using Git

Alternatively, you can clone the repository using Git:
```
git clone https://github.com/pencil-code/pencil-code.git
```
After checking out the code, make sure you have all the necessary permissions to create run directories. You can now navigate to the Pencil Code folder:
```
cd pencil-code
```
### 2.2 Directory Structure

After downloading, the `pencil-code` directory contains:

- `doc/` – Documentation (build the PDF manual using `make`)

- `samples/` – Example simulation setups

- `config/` – Configuration files

- `src/` – Core source code

- `bin/,` `lib/` – Utility scripts

- `idl/`, `python/`, `julia/`, etc. – Data analysis tools

----

## 3. Configure the Shell Environment

Navigate to the Pencil Code directory and ensure the environment variables are loaded:
```
cd pencil-code
```
## 4. Your First Simulation Run
###  4.1 Create a Run Directory

Create a new run directory and copy input files from a sample problem
(here: `samples/2d-tests/dark-matter`):
```
mkdir -p /data/myuser/myrun/src
cd /data/myuser/myrun

cp $PENCIL_HOME/samples/2d-tests/dark-matter/*.in ./
cp $PENCIL_HOME/samples/2d-tests/dark-matter/src/*.local src/
```
---
### 4.2 Linking to the Pencil Code Sources

Set up symbolic links to the `Pencil Code` source directory:
```
pc_setupsrc
```
---
### 4.3 Configuration Files

Two configuration files define the simulation setup:

- `src/Makefile.local` – selects the physical modules

- `src/cparam.local` – defines the grid size and number of processors

4.3.1 Single-Processor Setup

Example `src/Makefile.local`:
```
MPICOMM=nompicomm
```
Example `src/cparam.local`:
```
integer, parameter :: ncpus=1,nprocx=1,nprocy=1
integer, parameter :: nprocz=ncpus/(nprocx*nprocy)
integer, parameter :: nxgrid=128,nygrid=1,nzgrid=128
```
4.4 Compiling the Code

Compile the code using the default compiler configuration:
```
pc_build
```
4.5 Running the Simulation

- Step 1: Initial conditions
The initial conditions for your simulation are defined in the file `start.in`.

- Step 2: Runtime parameters
The parameters controlling the main simulation are specified in `run.in`.

- Step 3: Output quantities
The quantities to be printed to output files are selected in `print.in`.

- Step 4: Create an empty data directory
```
mkdir data
```
- Step 5: Run the simulation
```
pc_run
```
If everything works correctly, you will see:
```
start.x has completed successfully
```
During the run, time-series information is printed to the console, for example:
```
---it--------t-------dt------rhom------urms------uxpt-----uypt-----uzpt-----
0    0.00 4.9E-03 1.000E+00 1.414E+00 2.00E+00 0.00E+00 0.00E+00
10   0.05 4.9E-03 1.000E+00 1.401E+00 1.98E+00 0.00E+00 0.00E+00
20   0.10 4.9E-03 1.000E+00 1.361E+00 1.88E+00 0.00E+00 0.00E+00
...
```
The run finishes with:
```
Simulation finished after xxxx time-steps
```
-----
### 5. Troubleshooting & Tips

- If you only modify start.in or run.in, recompilation is not required

- If you change Makefile.local or add physics modules, run pc_build again

- Re-running in the same directory may overwrite data

- Always check param.nml before long runs
---
### 6. Next Steps

- Try increasing resolution (nxgrid, nzgrid)

- Enable MPI for multi-processor runs

- Explore visualization tools in python/ or idl/

- Modify physics modules in src/

## 7. Reading and Visualizing Simulation Output (Python)

This section explains how to read Pencil Code simulation outputs and visualize results using **Python**.

### 7.1 Import Required Python Packages

```python
import pencil as pc              # Pencil Code Python interface
import matplotlib.pyplot as plt  # Plotting
import numpy as np
import os
```
### 7.2 Reading Time-Series Data

Time-series quantities (written during the run) can be read from the `data/` directory.
````
# Read time series data
ts = pc.read.ts(datadir='../data')

# Example: plot maximum density vs time
plt.figure()
plt.plot(ts.t, ts.rhopmax)
plt.xlabel("Time")
plt.ylabel("Maximum density")
plt.title("Time evolution of maximum density")
plt.show()
````
Here:

- ts.t is the simulation time array

- ts.rhopmax is the maximum density at each time step, which also you can plot other parametrs which you have in your `print.in`.
- 
### 7.3 Reading Snapshot (VAR) Files

Simulation snapshots are stored in var.dat files and can be read as follows:
```
# Read snapshot data
var = pc.read.var('var.dat', datadir='../data', proc=-1, trimall=True)

# Extract the field array
f = var.f

print('Number of snapshots:', len(f))
print('Array shape:', f.shape)
```


