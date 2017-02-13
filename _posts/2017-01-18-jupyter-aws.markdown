---
layout: post
comments: true
title:  "Setting up Jupyter on AWS to start automatically when launching or starting EC2 instance"
date:   2017-01-18 01:00:00
mathjax: true
---

This guide shows the steps to set up Jupyter on AWS and set it up so that it starts automatically when an instance in launched or re-started. I have tested these steps with the Ubuntu 16.04 LTS AMI ami-b7a114d7 that AWS offers.

* TOC
{:toc}

## AMI setup
Start a brand new Ubuntu 16.04 LTS AMI. When launching an AMI, use the following custom security groups:

- Type: Custom TCP Rule
  - Protocol: TCP
  - Port Range: 8888
  - Source: Custom 0.0.0.0/0
- Type: SSH
  - Protocol: TCP
  - Port Range: 22
  - Source: Custom 0.0.0.0/0

## Jupyter setup
After ssh-ing into the machine ran the following commands:

~~~ bash
sudo apt-get update
wget https://repo.continuum.io/archive/Anaconda2-4.2.0-Linux-x86_64.sh #update with the latest one from https://www.continuum.io/downloads#linux
bash Anaconda2-4.2.0-Linux-x86_64.sh
~~~

Follow the installation steps then:

~~~ bash
source .bashrc
jupyter notebook --generate-config
key=$(python -c "from notebook.auth import passwd; print(passwd())")
~~~

Type in a password and continue

~~~ bash
cd ~/.ssh
certdir=$(pwd)
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.key -out mycert.pem
~~~

Press Enter several times leaving all the fields blank. Then

~~~ bash
cd ~
sed -i "1 a\
c.NotebookApp.ip = '0.0.0.0'\\
c.NotebookApp.open_browser = False\\
c.NotebookApp.password = u'$key'" .jupyter/jupyter_notebook_config.py
jupyter notebook
~~~

I observed that by having all the lines in `.jupyter/jupyter_notebook_config.py` commented out except for the three lines above, I am able to access the Jupyter notebook even behind a firewall.

## Set Jupyter to start when instance boots
Add the following line right before the `exit 0` line in `\etc\rc.local`

~~~ bash
su ubuntu -c 'bash /home/ubuntu/start_ipynb.sh'
~~~

and create the file `/home/ubuntu/start_ipynb.sh` with the following lines

~~~ bash
export PATH="$PATH:/home/ubuntu/anaconda2/bin"
jupyter notebook --notebook-dir=/home/ubuntu/ --profile=nbserver > /tmp/ipynb.out 2>&1 &
~~~

The `--notebook-dir=` indicates the location to launch the jupyter notebook.

## Acknowledgements
- [Run Jupyter Notebook Server on AWS EC2](http://yangjie.me/2015/08/26/Run-Jupyter-Notebook-Server-on-AWS-EC2/)
- [Auto-start ipython notebook server](http://www.gallamine.com/2014/05/auto-start-ipython-notebook-server-on.html)
