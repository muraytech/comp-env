# LPA computational environment (lpa-comp-env)
![Geant4](https://img.shields.io/badge/geant4-v11.3.0-cc9933?style=for-the-badge) ![ROOT](https://img.shields.io/badge/ROOT-v6.36.04-blue?style=for-the-badge) ![Python](https://img.shields.io/badge/Python-v3.12.0-669900?style=for-the-badge)
 
This is the repository for setting up computational environment for the LPA project. For this we use [Singularity/Apptainer](https://apptainer.org/). The image is created from the definitions and requirements listed in this repository. Currently there is support for creating the Geant4 simulations and running the analysis in Python.

The Singularity image of the environment is also automatically built and uploaded to the Github Container Registry (GHCR). In order to download this prebuilt image, see [Sec. 1.2](#12-download-the-pre-built-image).

## 1. Setup

In order to setup the computational environment needed for the LPA project:
```bash
git clone git@github.com:muraytech/comp-env.git
cd lpa-comp-env
```

The image is built in two phases:
- first we build the base image with all the necessary packages needed for Geant4 and our python analyses. The first phase requires the use of the parameter `--fakeroot` in the build.
- in the second phase we build the Geant4 from scratch. Here we do not need to use `--fakeroot` anymore.


### 1.1 Building the image manually

If you do not have a pre-existing Singularity image, you need to build it:
```bash
singularity build --fakeroot lpa_base.simg lpa_base.singularity
singularity build lpa.simg lpa.singularity
```
Currently, the lpa_base used for lpa uses the lpa_base coming from GHCR, though you can easily change it to use your local build.

### 1.2 Download the pre-built image:

```bash
apptainer pull oras://ghcr.io/muraytech/comp-env/lpa:latest
mv lpa_latest.sif lpa.simg
```

### 1.3 Having a pre-existing image
If you have already access to a pre-exisiting image (either having built it manually or having downloaded it from the GHCR), you can enter the environment by doing `apptainer shell /path/to/lpa.simg`

If, however, you wish to submit batch jobs, it is useful to have a 'run script' such as the example one [run.sh](run.sh) in this repository. With this script you can run your code in the LPA environment simply by doing for example `./run.sh python3 my_python_script.py`