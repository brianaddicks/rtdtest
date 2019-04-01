.. rtdtest documentation master file, created by
   sphinx-quickstart on Mon Apr  1 09:10:33 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Todd
===================================

Todd is a repository of scripts used to gather data for Lockstep Analytics. Installation and management of these scripts is facilitated by the Dirk_ module.

Installation
------------

.. code-block:: powershell

   Install-Module Dirk
   Import-Module Dirk
   Install-Dirk -Path 'c:\lockstep' -GithubCredential (Get-Credential)

The above command will install Dirk to ``c:\lockstep``, prompting for credentials. Optionally you can supply ``-Branch`` to install a specific branch during development.

.. code-block:: powershell

   Install-Dirk -Path 'c:\lockstep' -GithubCredential (Get-Credential) -Branch 'MyTestBranch'

Installation
------------

The root directory for the Todd (``C:\lockstep\Todd`` in the example above) requires a `config.json` file. This will be loaded into Todd as ``$ToddConfig`` to be used by all subsequent scripts. The file is a json file with the following available settings.

.. csv-table:: Main Config File
   :header: Setting, IsRequired, Type, Requires, Description

   "SyslogServer", "no", "string", "SyslogPort SyslogApplication", "fqdn/ip of the desired syslog server"
   "SyslogPort", "no", "int", "SyslogServer SyslogApplication", "udp port for desired syslog server"
   "SyslogApplication", "no", "string", "SyslogServer SyslogPort", "application identifier to use for syslog messages"
   "LogDnaApiKey", "no", "int", "SyslogApplication LogDnaEnviroment", "apikey for logdna messages"
   "LogDnaEnvironment", "no", "int", "SyslogApplication LogDnaApiKey", "enviroment identifier to use for logdna messages (set this to the shortname of the customer)"
   "LogThreshold", "no", "int",, "verbosity level for logging (higher is more verbose)"
   "MaxLogFiles", "no", "int",, "number of local log files to keep before rolling them"
   "AesKey", "yes", "array",, "byte array used to encrypt senstive configuration info, created with New-EncryptionKey_ from CorkScrew_ PowerShell module"

Scheduled Task Setup
--------------------

The Dirk_ comes with a handy cmdlet (Register-DirkScheduledTask_) for setting up scheduled tasks to run Todd. It currently supports the following schedules.

* Daily
* Hourly
* Every 5 Minutes

Optionally you can supply an arbitrary ``-JobName`` parameter which is passed to Todd on runtime and used to determine what checks should run. More info on this at Inputs_. You must provide a credential to run the scheduled task.

.. code-block:: powershell

   Import-Module Dirk

   # Daily Task with job name of everyday
   $TaskCred = Get-Credential
   Register-DirkScheduledTask -Daily -JobName everyday -ScheduledTaskCredential $TaskCred

   # Hourly Task with job name of onthehour
   $TaskCred = Get-Credential
   Register-DirkScheduledTask -Hourly -JobName onthehour -ScheduledTaskCredential $TaskCred

   # Every 5 minutes task with name of allthetime
   $TaskCred = Get-Credential
   Register-DirkScheduledTask -Every5Minutes -JobName allthetime -ScheduledTaskCredential $TaskCred

If you need some other schedule, you can still use this cmdlet, just update the schedule as needed.

.. Link refs

.. _Dirk: https://lockstep-technology-group-dirk.readthedocs-hosted.com
.. _New-EncryptionKey: https://corkscrew.readthedocs.io/en/latest/cmdlets/New-EncryptionKey/
.. _Register-DirkScheduledTask: https://corkscrew.readthedocs.io/en/latest/cmdlets/Register-DirkScheduledTask/
.. _CorkScrew: https://corkscrew.readthedocs.io
.. _Inputs: inputs/index.html