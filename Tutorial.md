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
