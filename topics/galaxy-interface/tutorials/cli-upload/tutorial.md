---
layout: tutorial_hands_on

title: "Uploading data from the command-line"
zenodo_link: ""
questions:
  - How can I upload my data to Galaxy when it's not on the same computer as my web browser?
  - What happens if I'm uploading a very large file and the connection is interrupted?
objectives:
  - Install the galaxy-upload utility
  - Upload data to a Galaxy server from the command line
  - Interrupt and resume an upload
time_estimation: "30M"
key_points:
  - The galaxy-upload utility can be used to upload data from any system with a command line and network access
contributors:
  - natefoo
subtopic: features
tags:
  - data
  - upload
---

The standard method for uploading data to a Galaxy server is via the Galaxy user interface, in your web browser. This presents a challenge when the system your browser is running on is not the same as the one where your data are stored. While it may be possible to first copy your data from where it lives to your computer, this is inefficient and may even be impossible depending on the size of the data and the network speed.

If the system where your data is housed has a method for command-line access and is allowed to make outbound connections to a Galaxy server, then you can cut out the middleman and just upload data directly from your remote system to Galaxy.

> ### {% icon comment %} History of large data uploads
> Prior to the release of Galaxy 22.01, the primary method for uploading large data to Galaxy was via the FTP service. FTP upload support was added to Galaxy at a time when HTTP-based upload methods were unreliable and limited. Over the years since, institutional firewalls have made usage of the FTP service increasingly difficult, while HTTP-based upload methods have improved.
>
> New upload functionality using a robust, modern protocol ([tus.io](https://tus.io/)) was added to Galaxy in version 22.01. In August 2022, the FTP service on usegalaxy.org was decommissioned. Other Galaxy servers may continue to run an FTP upload service, but the method described in this tutorial works with *all* Galaxy servers running version 22.01 and later.
{: .comment}

# Installing galaxy-upload

The [galaxy-upload](https://github.com/galaxyproject/galaxy-upload) utility is written in Python and can be installed using ``pip``. It should run on any UNIX-like operating system with Python 3.7 or later. That means Linux, macOS, and Windows Subsystem for Linux.

> ### {% icon hands_on %} Hands-on: Checking galaxy-upload requirements
>
> 1. Log on to the system where the data you want to upload is accessed from.
>
> 2. Verify that the system has a supported version of Python:
>
>    > ### {% icon code-in %} Input: Bash
>    > ```bash
>    > python3 -V
>    > ```
>    {: .code-in}
>
>    > ### {% icon code-out %} Output: Bash
>    > ```
>    > Python 3.9.2
>    > ```
>    {: .code-out}
{: .hands_on}

## Installing galaxy-upload in a Python virtual environment

If you already have a supported version of Python, you can use it to create a virtual environment. If not, skip to the next section.

> ### {% icon hands_on %} Hands-on: Installing galaxy-upload with venv
>
> 1. Create a Python virtual environment to install galaxy-upload in:
>
>    > ### {% icon code-in %} Input: Bash
>    > ```bash
>    > python3 -m venv galaxy-upload
>    > ```
>    {: .code-in}
>
> 2. Activate the virtual environment:
>
>    > ### {% icon code-in %} Input: Bash
>    > ```bash
>    > . ./galaxy-upload/bin/activate
>    > ```
>    {: .code-in}
>
> You will need to perform this activation step any time you reconnect to the remote system and want to run galaxy-upload. Alternatively, you can create an alias or symlink `galaxy-upload` to a directory on your `$PATH` in order to skip this step.
>
> 3. Install galaxy-upload:
>
>    > ### {% icon code-in %} Input: Bash
>    > ```bash
>    > pip install galaxy-upload
>    > ```
>    {: .code-in}
{: .hands_on}

## Installing galaxy-upload with Conda

[Conda](https://docs.conda.io/) is a platform-independent package manager that can be installed and used without root privileges. If your Python version is too old, Conda makes it easy to install and use a newer version.

> ### {% icon hands_on %} Hands-on: Installing galaxy-upload with conda
>
> 1. If you do not already have Conda installed, install it using the [Miniconda](https://docs.conda.io/en/latest/miniconda.html) installer:
>
>    > ### {% icon code-in %} Input: Bash
>    > ```bash
>    > curl -LO https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
>    > bash ./Miniconda3-latest-Linux-x86_64.sh
>    > # complete interactive installer steps
>    > ```
>    {: .code-in}
>
> 2. Create a Conda environment containing galaxy-upload:
>
>    > ### {% icon code-in %} Input: Bash
>    > ```bash
>    > conda create -n galaxy-upload -c conda-forge -c bioconda galaxy-upload
>    > ```
>    {: .code-in}
>
> 3. Activate the Conda environment:
>
>    > ### {% icon code-in %} Input: Bash
>    > ```bash
>    > conda activate galaxy-upload
>    > ```
>    {: .code-in}
>
> You will need to perform this activation step any time you reconnect to the remote system and want to run galaxy-upload. Alternatively, you can create an alias or symlink `galaxy-upload` to a directory on your `$PATH` in order to skip this step.
{: .hands_on}

## Installing galaxy-upload with Docker or Singularity/AppTainer

It is also possible to run galaxy-upload from a container. Consult the [galaxy-upload documentation](https://galaxy-upload.readthedocs.io/) for details.

# Uploading data with galaxy-upload
