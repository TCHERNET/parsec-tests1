# parsec-tests
parsec benchmark suit is used to measure the stastics performances of a computer system.
# parsec-tests
defination: Parsec benchmark suit is used to measure the stastics performances of a computer system.
NOTE: This repo has been archived and is no longer under maintenance. The resources discussed here can be found in gem5-resources. More information on the gem5 project can be found at http://www.gem5.org.

This repository contains different files (launch scripts, gem5 configuration files, disk image and linux configuration files) needed to run experiments with gem5art. This README describes how to run different experiments with gem5art step-by-step and points to the detailed documentation on these experiments as well.

Note: The user should make sure to create a local git repository containing all files for a particular experiment (which can be accessed from this repository), and add a remote to that repository as well.

**The commands to do this:**

git init
git remote add origin https://your-remote-add/parsec-tests.git
**Steps to Run Full System Parsec Tests**
First, you have to download all the required files needed to run these experiments from this repo. The required files are as follows:

- gem5 configurations for parsec tests
- disk image files specific for parsec tests
- disk image files shared among all experiments
- launch script for parsec tests
- Linux kernel configuration files
- .gitignore file
In case of confusion on the directory structure to keep these files, refer to the gem5art parsec tests tutorial.

Once, you have downloaded all these files, follow the following steps to run full system simulation gem5 with parsec tests:

**To create python3 virtual environment:
**
virtualenv -p python3 venv
source venv/bin/activate
To install gem5art if not already installed:

****pip install gem5art-artifact gem5art-run gem5art-tasks****

**To build gem5:**
For instructions on how to build gem5 look here.
**git clone https://gem5.googlesource.com/public/gem5
cd gem5
git checkout 71f6c87305cadf763966f9a6398a2466bc198b37
scons build/X86/gem5.opt -j5
To build m5 util:**

**cd gem5/util/m5/
make -f Makefile.x86
To create a disk image:**

**cd disk-image/
wget https://releases.hashicorp.com/packer/1.4.3/packer_1.4.3_linux_amd64.zip
unzip packer_1.4.3_linux_amd64.zip
./packer validate parsec/parsec.json
./packer build parsec/parsec.json**
To compile the linux kernel for all versions (v5.2.3, v4.19.83, v4.14.134, v4.9.186, v4.4.186):

**git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
mv linux linux-stable
cd linux-stable
for V in 5.2.3 4.19.83 4.14.134 4.9.186 4.4.186; do git checkout v$V; cp ../linux-configs/config.$V .config; make -j8; cp vmlinux vmlinux-$V;  done;
Run the database (if not already running) using (run this command after creating a directory):**

**docker run -p 27017:27017 -v <absolute path to the created directory>:/data/db --name mongo-<some tag> -d mongo**
Run a celery server using (unless this experiment is run by someone in DArchR):

celery -E -A gem5art.tasks.celery worker --autoscale=[number of workers],0
For users inside DArchR follow these steps, to run your experiments using centralized celery server.

Finally, run the launch script to start running these experiments:

python3 launch_parsec_tests.py
Details of these experiments can be seen in a tutorial here.
To run full-system simulations, you will need compiled system firmware
(console and PALcode for X86_64 architecture), kernel binaries and one or more disk
images.

If you need a full distributions or workloads please join to PARSEC Benchmark suit https://parsec.cs.princeton.edu/

Enjoy using PARSEC suit and please share your modifications and extensions.
****Notice:****
The suite needs at least 8 GB of free disk space, but i recommend you 12 GB or more. To download the whole distribution, you need additional 3 GB. Additionally you can also download any number of binary distributions that include precompiled binaries for each workload. Each binary distribution requires 0.5 GB to download and another 2 GB of disk space to unpack.
Results of these experiments are available here
