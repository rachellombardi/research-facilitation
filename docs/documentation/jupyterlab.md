[title]: - "Launching a JupyterLab Instance"

[TOC]


*Disclaimer: JupyterLab is a new feature. We appreciate thoughts and feedback sent to support@opensciencegrid.org*


# Objective

This guide describes how to request an account to access JupyterLab, launch a JupyterLab instance, and how to send jobs from JupyterLab to HTCondor. 


# What is JupyterLab?
JupyterLab is a web-based interface for [Project Jupyter](https://jupyter.org). It is an interactive environment that supports a variety of computational activities and workflows through features such as notebooks, terminals, text editors, etc.

More information about the JupyterLab interface can be found in the [JupyterLab manual](https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html). 

JupyterLab can be a helpful resource for teaching and for running small batches of HTCondor jobs for testing and/or troubleshooting purposes.  


# Steps to Accessing and Working in JupyterLab
The steps necessary for working in JupyterLab are: 
- request an account
- meet with a Research Computing Facilitator for a short introduction to working with JupyterLab and to activate your account
- launch a JupyterLab instance


# Request Access to a JupyterLab Access Point

## Request an Account 
To request access to JupyterLab, submit an application using the following steps:

(1) Go to the [registration website](https://registry.cilogon.org/registry/co_petitions/start/coef:261). You will be redirected to the CILogon sign in page. Select your institution and use your institutional credentials to login. 

If you have issues signing in using your institutional credentials, contact us at support@opensciencegrid.org.

(2) Once you sign in, you will be redirected to the "JupyterLab User Enrollment for New Users" page. Click "Begin" and enter your name and email address on the following page. In many cases, this information will be automatically populated. If desired, it is possible to manually edit any information automatically filled in. Once you have entered your information, click "SUBMIT". 

(3) After submitting your application, you will receive an email from registry@cilogon.org to verify your email address. Click the link listed in the email to be redirected to a page confirm your invitation details. Click the "ACCEPT" button to complete this step. 


## Meet with a Research Computing Facilitator
Once OSG staff receive your email verification, a Research Computing Facilitator will contact you within one business day to arrange a short consultation and introduction to JupyterLab resources. During this meeting, our staff will provide personalized start-up guidance per your specific computational research goals and activate your account.
You will be notified when your account is approved via email.

Additionally, you will be assigned a "ProjectName" that you can use later in an HTCondor submit file to submit jobs through your JupyterLab interface to HTCondor. 


## Upload an SSH Key
Adding an SSH public key is optional. Contact us to discuss alternative ways to authenticate when logging in.

After your account has been approved following a meeting with a Research Computing Facilitator, the last step of account creation is to add an SSH key. To do this:

(1) Return to the [registration website](https://registry.cilogon.org/registry/co_petitions/start/coef:261) and login using CILogon if prompted.

(2) Click your name at the top right. In the dropdown box, click "My Profile (OSG)" button. 

(3) On the right hand side of your profile, click "Authenticators" link. 

(4) On the authenticators page, click the "Manage" button. 

(5) On the new SSH Keys page, click "Add SSH Key" and browse your computer to upload your public SSH key.


# How to launch a JupyterLab Instance
To launch a JupyterLab instance, go to [https://jupyterhub.osgdev.chtc.io/](https://jupyterhub.osgdev.chtc.io/) using an internet browser. 

You will be prompted to "Sign in" using your institution CILogin credentials.

Once logged in, you will be automatically redirected to the "Server Options" page. Several server options are listed, including:

| Notebook Server Options      | Description |
| ----------- | ----------- |
| Minimal      | Includes basic command-line tools.      |
| R    | Includes the R interpreter and base environment.   |
| SciPy      | Includes popular packages from the scientific Python ecosystem.       |
| TensorFlow    | Includes popular Python deep learning libraries.  |
| Data Science      | Includes libraries for data analysis from the Julia, Python, and R communities.       |
| PySpark    | Includes Python support for Apache Spark.  |
| All Spark      | Includes Python, R, and Scala support for Apache Spark.      |


To request a specific server configuration, [contact a Research Computing Facilitator](support@osgconnect.net). 

Select your desired server option and click "Start" to launch your instance. This process may take several minutes to complete. You will be redirected automatically to JupyterLab when your instance is ready.


# Working with your JupyterLab Instance
Working in JupyterLab, you will be able to interact with files in your `/home` directory, execute code, and save files. 

Each user has a *total* of 8 CPUs and 16 GB memory available to their JupyterLab instance and HTCondor jobs submitted from the notebook. 


# Sending jobs to HTCondor from JupyterLab
To send jobs from JupyterLab to HTCondor, the following information must be added to the HTCondor submit file with one modification: 

```
# The `requirements =` and `+FromJupyterLab` lines tell HTCondor to assign all jobs to run on the dedicated execute point server assigned to your instance upon launch. It is not necessary to edit these lines. 
requirements = Machine == "CHTC-Jupyter-User-EP-$ENV(HOSTNAME)"
+FromJupyterLab = true

# Sets a Project Name for this job submission 
+ProjectName = "ProjectNameinCOManage"
```

The only modification that needs to be made to these submit file attributes is to replace "ProjectNameinCOManage" with the project name you were assigned when you met with a Research Computing Facilitator. 

Therefore, an example HTCondor submit file for a JupyterLab job may look like: 

```
# hello-world.sub

executable = hello-world.sh
# arguments = 

log = hello-world.log
output = hello-world.out
error = hello-world.err

# transfer_input_files = file1, /path/to/file2

request_cpus = 1
request_memory = 1 GB
request_disk = 2 GB

requirements = Machine == "CHTC-Jupyter-User-EP-$ENV(HOSTNAME)"
+ProjectName = "ProjectNameinCOManage"
+FromJupyterLab = true

queue
```


# Log Out of JupyterLab Session
To log out of your session, go to the top left corner of the JupterLab interface and click the "File" tab. Under this tab, click "Log Out". 

# Get Help
For questions or to receive help with launching JupyterLab, [contact a Research Computing Facilitator](support@osgconnect.net) 



