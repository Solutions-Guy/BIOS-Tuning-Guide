# BIOS-Tuning-Guide
BIOS Tuning a BigTwin SuperServer

Supermicro Update Manager (SUM) is a command line interface utility for scripting the deployment of BIOS options from a centralized management server. For more details on how to use the SUM tool,
please refer to the SUM User Guide on the Supermicro website. This section will provide basic examples of how to use SUM to tune the X11 Dual Socket Motherboards.

Pre-requisites:
- Installation of SUM.
- Setting Up OOB Managed Systems.
- Activated License Key for Managed Systems on all server nodes. 

*It is common practice to manually configure the BIOS, then export the settings into an XML file by using
the GetCurrentBiosCfg command.

** Once a “golden image” has been exported into an XML format, ChangeBiosCfg is the recommended
command to change the BIOS configuration.

# BIOS Management Command Example
Below is an example of how to get and change the BIOS settings with the SUM utility.

1. Get Current BIOS Settings
The following show OOB and In-Band syntax for getting the current BIOS settings.

```yml
OOB Syntax:
$ ./sum -i <IP or hostname> -u <username> -p <password> -c GetCurrentBiosCfg --file <file_name>

In-Band Syntax:
$ ./sum -c GetCurrentBiosCfg --file <file_name>
```

2. Change BIOS Settings

The following show OOB and In-Band syntax for changing the BIOS settings.

```yml
OOB Syntax:
$ ./sum -i <IP or hostname> -u <username> -p <password> -c ChangeBiosCfg --file <file_name> --reboot

In-Band Syntax:
$ ./sum -c ChangeBiosCfg --file <file_name> --reboot
```

# BIOS Settings Dependencies

Some BIOS settings are dependent on other BIOS settings. In the BIOS menu, the settings will be highlighted or grayed out to indicate whether they are configurable. However, the SUM BIOS
management commands do not show highlighted or grayed out settings. Therefore, it is important to know the BIOS setting dependencies when using the SUM BIOS management commands.

Note:
It is critical to understand dependencies when using the SUM BIOS management commands. For example, the following settings cannot be changed unless Power Technology is set to Custom.
• CPU P State Control
• Hardware PM State Control
• CPU C State Control
• Package C State Control
• CPU T State Control
• Energy and Performance Bias

By default, Power Technology is set to Energy Efficient Mode. Using the SUM Tool to execute a BIOS change that does not align with the dependencies will result in an error. 

BIOS Configuration Fail Message

This section shows the error message when dependencies are not met. For example, using ChangeBiosCfg to set the CPU C6 Report value to “Disabled,” will return the following output if Power Technology is set to “Disable.” Power Technology should be set to “Custom” in order to change the
value of the CPU C6 Report via SUM Tool.

```yml
OOB Example with a missing dependency:
$ ./sum -i 172.31.xx.xx -u <ADMIN> -p <PASS> -c ChangeBiosCfg --file MissingDependency.xml --reboot
```

![](https://github.com/Solutions-Guy/BIOS-Tuning-Guide/blob/master/Sample%20error%20message%20message%20from%20SUM%20Utility%20when%20dependencies%20are%20not%20met.png)
<p align="center">Sample error message from SUM Utility when dependencies are not met</p>

# BIOS Configuration Success Message

If all dependencies are met and the ChangeBiosCfg command executes successfully, the message “The BIOS configuration is updated for <IP address>” will appear.


```yml
OOB Example with recommended latency-sensitive BIOS settings covered in section 3.3:
$ ./sum -i 172.31.xx.xx -u <ADMIN> -p <PASS> -c ChangeBiosCfg --file LowLatency.xml --reboot
```

![](https://github.com/Solutions-Guy/BIOS-Tuning-Guide/blob/master/Sample%20success%20message%20from%20SUM%20Utility.png)
<p align="center">Sample success message from SUM Utility</p>
