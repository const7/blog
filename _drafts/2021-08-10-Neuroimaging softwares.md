---
layout: post
title: 常用 neuroimaging 软件安装
tags: [Linux, Neuroimaging]
description: Neuroimaging software installation on Linux
mathjax: false
comment: true
---

记录 linux 上一些常用脑影像数据处理软件安装流程及注意事项。

---

**目录**

* toc
{:toc}

> Basic: Ubuntu 20.04, Python 3.9.5

> Some additional operations are not necessary on the older version system since it's more compatible.

## FSL

## FreeSurfer

### Reference

- **installation guide**: [FS7](https://surfer.nmr.mgh.harvard.edu/fswiki//FS7_linux)

### Step

1. Download the [installation package](https://surfer.nmr.mgh.harvard.edu/fswiki/rel7downloads) and decompress it to someplace.

2. Setup the script path to bash profile `~/.bashrc` (or other shell profile).

    Add the following lines to your `~/.bashrc` and change the path to where your package decompressed.

    ```shell
    export FREESURFER_HOME=$HOME/freesurfer
    source $FREESURFER_HOME/SetUpFreeSurfer.sh
    ```

4. Test.

    ```shell
    which freeview
    ```

## ANTs

### Step

1. Download the installation script.

    ```shell
    git clone https://github.com/cookpa/antsInstallExample.git
    ```

2. You can modify some parameters or paths in the `installANTs.sh` file and run it.
    For more details please refer to the script.

    ```shell
    ./installANTs.sh
    ```

3. 

### Reference

- **installation instruction**: [Wiki](https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS)
- **quick installation**: [installation script](https://github.com/cookpa/antsInstallExample)

## AFNI

### Reference

- **installation**: [quick setup](https://afni.nimh.nih.gov/pub/dist/doc/htmldoc/background_install/install_instructs/steps_linux_ubuntu20.html#quick-setup-condensed-instructs)

### Step

1. Download scripts

    ```shell
    curl -O https://raw.githubusercontent.com/afni/afni/master/src/other_builds/OS_notes.linux_ubuntu_20_64_a_admin.txt
    curl -O https://raw.githubusercontent.com/afni/afni/master/src/other_builds/OS_notes.linux_ubuntu_20_64_b_user.tcsh
    curl -O https://raw.githubusercontent.com/afni/afni/master/src/other_builds/OS_notes.linux_ubuntu_20_64_c_nice.tcsh
    ```

2. Install some requirement package (admin privileges needed) from `bash` shell

    ```shell
    sudo bash OS_notes.linux_ubuntu_20_64_a_admin.txt 2>&1 | tee o.ubu_20_a.txt
    ```

3. Install other dependencies from `bash`

    ```shell
    tcsh OS_notes.linux_ubuntu_20_64_b_user.tcsh 2>&1 | tee o.ubu_20_b.txt
    ```

4. To niceify your terminal (not necessary but useful) for `bash`

    ```shell
    tcsh OS_notes.linux_ubuntu_20_64_c_nice.tcsh 2>&1 | tee o.ubu_20_c.txt
    ```

## fmriprep

### Reference

### Step

## Nipype

###

### Attention

1. `dipy` 1.4.1 is not compatible with `nipype` 1.6.1, see [this issue](https://github.com/dipy/dipy/issues/2402) for more details. You may need to downgrade `dipy` version. (2021.8.11)

2. Make sure you install all package requirements and check the error log again and again.