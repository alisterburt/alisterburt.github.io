---
date: 2023-05-27
---

# Managing Python installations in 2023

The following is a short, opinionated guide on how to manage Python installations for
scientific computing in 2023.

## Motivation

Why write this post? I work in and around the scientific Python ecosystem,
one of the projects I'm involved in is called [*napari*](https://napari.org).
Some people without Python experience want to use *napari*, they often end up frustrated.

<body>
    <div style="margin:0 auto; align: center; width: 45% !important">
        <blockquote class="twitter-tweet"><p lang="en" dir="ltr">I wish I could use the wonderful <a href="https://twitter.com/napari_imaging?ref_src=twsrc%5Etfw">@napari_imaging</a>. but it&#39;s painful as a biologist to update all the associated items such as &#39;pip&#39; . As a non-python user it&#39;s hard. I just don&#39;t have the time to look into all of the many dependencies. Needs a <a href="https://twitter.com/fiji?ref_src=twsrc%5Etfw">@fiji</a>-like Update button</p>&mdash; Darren Thomson (@fungalhyphae) <a href="https://twitter.com/fungalhyphae/status/1654455013781929984?ref_src=twsrc%5Etfw">May 5, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
    </div>
</body>

This makes me sad 🙁

Good news, there is a happy path! Once you're on it, I promise working with Python can be a great experience.
I'm writing this to share the way I work and help you get on the right track.

## Introduction

We are going to use **virtual environments** to manage our Python installation(s).
Specifically,
[**conda environments**](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/environments.html#conda-environments)
managed
with [**micromamba**](https://mamba.readthedocs.io/en/latest/user_guide/micromamba.html).

### Why do I need environments?

Python is ubiquitous, it's probably used in lots of different places on your computer
already.
Code written for specific versions of either the Python language or
Python packages won't necessarily work with different versions.
A virtual environment is a little box you can put Python and various packages into
which is isolated from the rest of your system. What you do in the box won't affect
what's going on outside the box.

### Why *micromamba*?

One of the easiest ways to mess up a conda installation is to install a bunch of stuff
into the base environment.
*micromamba* doesn't give you a base environment so you can't mess it up!
It's also wicked fast.

## Installation

=== "MacOS/Linux"

    1. Follow the [instructions from the *mamba* documentation](https://mamba.readthedocs.io/en/latest/installation.html#install-script)
    2. Set `conda` as an alias of `micromamba`

        ???+ Note
            Add the following to your `~/.bashrc` (Linux) or `~/.zshrc` (macOS)
            ```sh
            alias conda="micromamba"
            ```
    
    3. Reload your shell so that changes take effect
    4. Set [*conda-forge*](https://conda-forge.org/) as the default channel

        ???+ Note
            ```sh
            conda config --add channels conda-forge
            ```

=== "Windows"

    1. Follow the [instructions from the *mamba* documentation](https://mamba.readthedocs.io/en/latest/installation.html#windows)
    2. Set `conda` as an alias of `micromamba`

        ???+ Note
            Create the alias and add it to your [PowerShell profile](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.3)
            ```powershell
            New-Alias -Name conda -Value micromamba
            Export-Alias -Path "$HOME\Documents\PowerShell\Profile.ps1" -Append -Name conda
            ```
    3. Reload your shell so that changes take effect
    4. Set [*conda-forge*](https://conda-forge.org/) as the default channel

        ???+ Note
            ```PowerShell
            conda config --add channels conda-forge
            ```

## Usage

### Creating and activating environments

To create an environment with a specific version of Python

```shell
conda create --name my-new-env python=3.10
```

To work in the environment we first need to activate it. We do this with

```shell
conda activate my-new-env
```

Once inside the environment, you have access to the packages you have installed there.
???+ Note
    ```shell
    (my-new-env) ➜ python --version
    Python 3.10.11
    ```

### Installing packages

You can install most packages into your environment with `conda` or `pip`.
Install what you can with `conda`. If a package is available on 
[PyPI](https://pypi.org/) but not `conda-forge` then use `pip`.

=== "conda"
    ```txt
    (my-new-env) ➜ conda install numpy
    ```

=== "pip"
    ```txt
    (my-new-env) ➜ pip install numpy
    ```
  
### Deactivating environments

We can exit an environment with

```shell
conda deactivate
```

### Removing environments

We can remove an environment with

```shell
conda env remove --name my-new-env
```

### Running commands from outside an environment

If you want to run a command in a specific environment from outside the environment
you can

```shell
conda run --name my-new-env command
```

This is useful for workflows which rely on software with incompatible dependencies.

### tips

- get comfortable with creating/destroying and activating/deactivating environments
- one environment per project is a useful ideal, in practice a general purpose
  default environment is useful for quick scripting/analysis

## Closing

That's it! Working in conda environments empowers you to install things without
worrying about messing up your whole system. If you've ever felt 
