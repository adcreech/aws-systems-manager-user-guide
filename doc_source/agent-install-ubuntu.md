# Manually install SSM Agent on Ubuntu Server instances<a name="agent-install-ubuntu"></a>

Connect to your Ubuntu Server instance and perform the steps in one of following procedures to install SSM Agent on each instance that will run commands using Systems Manager\.

## About SSM Agent installations on 64\-bit Ubuntu Server 16\.04 instances<a name="agent-install-ubuntu-about-v16"></a>

Beginning with instances created from Ubuntu Server 16\.04 AMIs identified with `20180627`, SSM Agent is pre\-installed using Snap packages\. For example: `ubuntu/images/hvm-ssd/ubuntu-xenial-16.04-amd64-server-20180627`\. On instances created from earlier AMIs, you should continue using deb installer packages\. 

**Important**  
Be aware that if an instance has more than one installation of the SSM Agent \(for example, one installed using a Snap, one installed using a deb installer\), your agent operations will not work correctly\.

You can check the source AMI ID for an instance following these steps:

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation, choose **Instances**\.

1. Select an instance\.

1. On the **Description** tab, locate the value in the **AMI ID** field\.

For instances created from a 64\-bit Ubuntu Server 16\.04 AMI, be sure to follow the correct procedure for your SSM Agent installation type:
+ **Instances created from AMIs with identifier `20180627` or later**: [Install SSM Agent on Ubuntu Server instances](#agent-install-ubuntu-tabs)
+ **Instances created from AMIs earlier than `20180627`**: [Install SSM Agent on Ubuntu Server instances](#agent-install-ubuntu-tabs)

## Install SSM Agent on Ubuntu Server instances<a name="agent-install-ubuntu-tabs"></a>

------
#### [ Ubuntu Server 18\.04 and 16\.04 LTS 64\-bit \(Snap\) ]

**To install SSM Agent on Ubuntu Server 18\.04 and 16\.04 LTS 64\-bit instances \(with Snap package\)**

1. SSM Agent is installed, by default, on Ubuntu Server 18\.04 and on 16\.04 LTS 64\-bit AMIs with an identifier of `20180627` or later\. For more information about version 16\.04 AMIs, see [Manually install SSM Agent on Ubuntu Server instances](#agent-install-ubuntu)\.

   You can use the following script if you need to install SSM Agent on an on\-premises server or if you need to reinstall the agent\. You don't need to specify a URL for the download, because the `snap` command automatically downloads the agent from the [Snap app store](https://snapcraft.io/amazon-ssm-agent) at [https://snapcraft\.io](https://snapcraft.io)\.

   ```
   sudo snap install amazon-ssm-agent --classic
   ```
**Note**  
Note the following details about SSM Agent on Ubuntu Server 18\.04 and 16\.04:  
Because of a known issue with Snap, you might see a `Maximum timeout exceeded` error with `snap` commands\. If you get this error, run the following commands one at a time to start the agent, stop it, and check its status:   

     ```
     sudo systemctl start snap.amazon-ssm-agent.amazon-ssm-agent.service
     ```

     ```
     sudo systemctl stop snap.amazon-ssm-agent.amazon-ssm-agent.service
     ```

     ```
     sudo systemctl status snap.amazon-ssm-agent.amazon-ssm-agent.service
     ```
On Ubuntu Server 18\.04 and 16\.04, SSM Agent installer files, including agent binaries and config files, are stored in the following directory: `/snap/amazon-ssm-agent/current/`\. If you make changes to  any configuration files in this directory, then you must copy these files from the `/snap` folder to the `/etc/amazon/ssm/` folder\. Log and library files have not changed \(`/var/lib/amazon/ssm`, `/var/log/amazon/ssm`\)\.
On Ubuntu Server 18\.04, use Snaps only\. Don't install deb packages\. Also verify that only one instance of the agent is installed and running on your instances\.
On Ubuntu Server 18\.04 and 16\.04, SSM Agent provides support for the arm64 processor architecture\.
On Ubuntu Server 16\.04, SSM Agent is installed using either Snaps or deb installation packages, depending on the version of the 16\.04 AMI\. For more information, see [Manually install SSM Agent on Ubuntu Server instances](#agent-install-ubuntu)\.

1. Run the following command to determine if SSM Agent is running\. 

   ```
   sudo snap list amazon-ssm-agent
   ```

1. Run the following command to start the service if the previous command returned amazon\-ssm\-agent is stopped, inactive, or disabled\.

   ```
   sudo snap start amazon-ssm-agent
   ```

1. Check the status of the agent\.

   ```
   sudo snap services amazon-ssm-agent
   ```

------
#### [ Ubuntu Server 16\.04 and 14\.04 64\-bit \(deb\) ]

**To install SSM Agent on Ubuntu Server 16\.04 and 14\.04 64\-bit instances \(with deb installer package\)**

1. You can use the following script if you need to install SSM Agent on an on\-premises server or if you need to reinstall the agent\.
**Important**  
SSM Agent is installed by default on instances created from Ubuntu Server 16\.04 LTS 64\-bit AMIs with an identifier of `20180627` or later\. Instances created from AMIs with earlier identifiers, for example `20171121.1` and `20180522`, should continue to use deb installers\.   
If SSM Agent is installed on your instance in conjunction with a Snap and you install or update SSM Agent using a deb installer package, the installation or SSM Agent operations may fail\. For more information, see [Manually install SSM Agent on Ubuntu Server instances](#agent-install-ubuntu)\.

   Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

   Change to the temporary directory\.

   ```
   cd /tmp/ssm
   ```

   Run the following commands\.
**Note**  
Even though the following download URL shows 'ec2\-downloads\-windows', this is the correct URL\.

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
   ```

   ```
   sudo dpkg -i amazon-ssm-agent.deb
   ```

1. Run one of the following commands to determine if SSM Agent is running\. 

   Ubuntu Server 16\.04:

   ```
   sudo systemctl status amazon-ssm-agent
   ```

   Ubuntu Server 14\.04:

   ```
   sudo status amazon-ssm-agent
   ```

1. Run one of the following commands to start the service if the previous command returned amazon\-ssm\-agent is stopped, inactive, or disabled\.

   Ubuntu Server 16\.04:

   ```
   sudo systemctl enable amazon-ssm-agent
   ```

   Ubuntu Server 14\.04:

   ```
   sudo start amazon-ssm-agent
   ```

1. Run one of the following commands to check the status of the agent\.

   Ubuntu Server 16\.04:

   ```
   sudo systemctl status amazon-ssm-agent
   ```

   Ubuntu Server 14\.04:

   ```
   sudo status amazon-ssm-agent
   ```

------
#### [ Ubuntu Server 16\.04 and 14\.04 32\-bit ]

**To install SSM Agent on Ubuntu Server 16\.04 and 14\.04 32\-bit instances**

1. Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

   Change to the temporary directory\.

   ```
   cd /tmp/ssm
   ```

   Run the following commands\.
**Note**  
Even though the following download URL shows 'ec2\-downloads\-windows', this is the correct URL\.

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_386/amazon-ssm-agent.deb
   ```

   ```
   sudo dpkg -i amazon-ssm-agent.deb
   ```

1. Run the following command to determine if SSM Agent is running:

   ```
   sudo status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returned amazon\-ssm\-agent is stopped, inactive, or disabled\.

   1. Start the agent:

      ```
      sudo start amazon-ssm-agent
      ```

   1. Check the status of the agent:

      ```
      sudo status amazon-ssm-agent
      ```

------

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.