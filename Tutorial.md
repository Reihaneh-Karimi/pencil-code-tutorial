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
2.2 Directory Structure

After downloading, the pencil-code directory contains:

doc/ – Documentation (build the PDF manual using make)

samples/ – Example simulation setups

config/ – Configuration files

src/ – Core source code

bin/, lib/ – Utility scripts

idl/, python/, julia/, etc. – Data analysis tools

----

## 3. Configure the Shell Environment

Navigate to the Pencil Code directory and ensure the environment variables are loaded:
```
cd pencil-code
```
## 4. Your First Simulation Run
###  4.1 Create a Run Directory

Create a new run directory and copy input files from a sample problem
(here: samples/2d-tests/dark-matter):
```
mkdir -p /data/myuser/myrun/src
cd /data/myuser/myrun

cp $PENCIL_HOME/samples/2d-tests/dark-matter/*.in ./
cp $PENCIL_HOME/samples/2d-tests/dark-matter/src/*.local src/
```
---
### 4.2 Linking to the Pencil Code Sources

Set up symbolic links to the Pencil Code source directory:
```
pc_setupsrc
```
---
### 4.3 Configuration Files

Two configuration files define the simulation setup:

src/Makefile.local – selects the physical modules

src/cparam.local – defines the grid size and number of processors
--
4.3.1 Single-Processor Setup

Example src/Makefile.local:
```
MPICOMM=nompicomm
```
Example src/cparam.local:
```
integer, parameter :: ncpus=1,nprocx=1,nprocy=1
integer, parameter :: nprocz=ncpus/(nprocx*nprocy)
integer, parameter :: nxgrid=128,nygrid=1,nzgrid=128
```
--
4.4 Compiling the Code
--
Compile the code using the default compiler configuration:
```
pc_build
```
