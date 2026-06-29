# Before we begin
  
## Introduction to the terminal

Throughout this training we'll be interacting with the HPC system via a **terminal** or **command line**.
This means we'll be providing our instructions and receiving responses in text form.
There's a few important commands we'll cover gradually as we need them.

Why text instead of a graphical interface?
Text-based interfaces were naturally the default before graphical interfaces became widely available, but even now they remain the most common way to interact with remote HPC systems.
There are several advantages which a text-based interface gets us:

- They're precise - by using a well established set of commands it's clear what we mean
- They're repeatable - since our commands are just text, they can be easily saved, shared and run as automated scripts
- They're low-bandwidth - by using text instead of graphics, we're not taking up network bandwidth that could be better used for moving data around

Depending on your operating system there are a number of terminal applications available which we would recommend:

- Windows: PowerShell - note that this uses a different command syntax, but once we're logged into the HPC this won't matter
- MacOS: Terminal
- Linux: The name of your terminal application may vary, but it's usually called something like "Terminal"

You should be familiar with basic terminal commands before participating in this workshop.
However, we have created a list of [common commands](./unix_commands.md) you can use as a reference sheet.

## Connecting to HPC

You should have been provided with a guest username and password which you can use to log in to the HPC system used for this course. The exact format of this guest username will differ depending on the requirements of the administration team for your HPC system. It will be an alphanumeric string, containing no special characters, and will possibly based on your email address or name.

The server address of your HPC system will also depend on what your training team have setup. If you are using an existing HPC system this might be an address such as `hpc.create.kcl.ac.uk` or `csf3.itservices.manchester.ac.uk`. If your training team has set up a cloud-hosted instance for you, then this might just be the plain IP address, such as `035.177.011.164`. 

!!! Info
    For the purposes of this training material we will use `k1234567` as our example username, and `hpc.university.ac.uk` as our server name. We may, where appropriate, alternatively use `<username>` or `<server>` to indicate where the user or server name should go. In either instance, when following the material below, you should replace these both with the username and server address that your trainers have provided you.

You can connect to the login nodes using the following command (replacing `k1234567` with your guest username, and `hpc.university.ac.uk` with your server name):

```bash
ssh k1234567@hpc.university.ac.uk
```

You will be prompted to enter your password:

```bash
$ ssh k1234567@hpc.university.ac.uk
k1234567@hpc.university.ac.uk's password: 
```

Type your password then press 'Enter'.
You will not see any indication that your password has been typed in - this is normal!
If the password is incorrect you will be prompted to retype it.

!!! Important
    The first time you login you may be asked to change your password. To do this you will reenter your guest password, and then asked twice to type in your new password. You will not see any indication that your password has been typed in. If you do change your password make sure to follow best practice in creating it, such as using [three random words](https://www.ncsc.gov.uk/collection/top-tips-for-staying-secure-online/three-random-words).

Upon successful connection you should see information about the system you have logged on to, perhaps similar to

```text
Welcome to Ubuntu 24.04.4 LTS (GNU/Linux 6.17.0-1015-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Fri Jun 26 15:44:05 UTC 2026

  System load:  0.0                 Temperature:           -273.1 C
  Usage of /:   21.2% of 122.94GB   Processes:             169
  Memory usage: 11%                 Users logged in:       1
  Swap usage:   0%                  IPv4 address for ens5: 10.0.0.248

==================================================================
k1234567@login1:~$
```

## The environment

All HPC systems will be UNIX based, probably a linux system such as Ubuntu. They will likely use a Bash shell.

Each HPC filesystem will be setup according to local requirements. But they generally will include common elements such as:

- Home directory located in `/users/` or `/home/`
- Personal scratch space located in `/scratch/` or `/scratch/users/`
- Group or project scratch space located in `/scratch/group/` or `/scratch/project/`
- Long-term research data storage located in `/rds/` or similar
- Datasets space located in `/datasets/` or similar

Most of these directories will be useful to you at some point over the course of your research, so we'll briefly introduce each in turn.

**Home directory:** `/home/<username>`.
Your home directory is normally the directory you see when you log in.
This is a good place to put any code you write or other custom software, configuration files and small amounts of data.
This directory is only accessible by you by default - other users can't access files in your home directory unless you specifically allow them to.
You will typically be allocated a small amount of disk space, such as 50GiB, for your home directory.

**Personal scratch:** `/scratch/<username>`.
Your personal scratch space is where you should store the input and output data for your active research.
Scratch space is designed to provide much higher performance which is important for many HPC jobs and has a much higher capacity and personal allocation.
Your personal scratch space can only be accessed by you by default - just like your home directory.
You will typically be allocated a larger, but still limited, amount of disk space, such as 200GiB, for your personal scratch, but usually you may request more if necessary.

**Group scratch:** `/scratch/grp/<groupname>` or **Project scratch:** `/scratch/prj/<projectname>`.
Group or project scratch space, where provided, is similar to your personal scratch space but as the name suggests is shared by a group or a project.
Every member of the group or project will be able to access the files stored in this space.

**Research Data Store (RDS):** `/rds/<projectname>`.

Many institutions provide a networked large-scale storage facility for live research data.
These are suitable for the largest datasets used by projects and where it's important that the data be properly backed up.

!!! Important
    Generally the only data storage locations that are backed up are your home directory (`/home/<username>`) and RDS projects (`/rds/<projectname>`). Data stored in scratch is *not* backed up, so make sure you copy any important results or code to RDS or another location that is backed up.
