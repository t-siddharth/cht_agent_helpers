<h3 align="center">CloudHealth Linux Agent Installation Helpers</h3>
<hr/>

The CloudHealth Linux Agent allows us to collect instance-level metrics. 

The configuration details and [default settings can be viewed here](https://apps.cloudhealthtech.com/agent_settings/edit): 

```
wget https://s3.amazonaws.com/remote-collector/agent/v14/install_cht_perfmon.sh -O install_cht_perfmon.sh;
sudo sh install_cht_perfmon.sh 14 #{unique registration code} aws;
```



We have created CloudHealth Linux agent installation helpers for the commonly used provisioning automation tools. We have helpers for:
  - [Cloud Init](https://gist.github.com/siddtewari/c850ec5c10d8efdbd3bb#cloud-init) 
  - [Chef Recipe](https://gist.github.com/siddtewari/c850ec5c10d8efdbd3bb/#chef-recipe)

<hr/>
##### Cloud Init

User Data that you can use with AWS as a bootstrap script

Replace unique-registration-code with your unique code. You can get the code from the agent configuration page https://apps.cloudhealthtech.com/agent_settings/edit

```
#!/bin/bash
# echo Defaults:root \!requiretty >> /etc/sudoers

#Install CHT linux agent for CHT-Sidd-Test account
echo Defaults:root \!requiretty >> /etc/sudoers
wget https://s3.amazonaws.com/remote-collector/agent/v14/install_cht_perfmon.sh -O install_cht_perfmon.sh
sudo sh install_cht_perfmon.sh 14 UNIQUE_REGISTRATION_CODE aws

# SAMPLE CLOUD-INIT COMMANDS THAT THE CLIENT MIGHT HAVE
# yum update -y
# yum install httpd -y
# service httpd start
# yum install php56 php-mysql git wget -y


```
<hr/>
##### Chef Recipe

You can install the CloudHealth Agent using the [Chef Recipe ](https://gist.github.com/siddtewari/1d406064ecacc52dfd0d) documented in this secret gist. Also listed below for convenience.

Replace unique-registration-code with your unique code. You can get the code from the agent configuration page https://apps.cloudhealthtech.com/agent_settings/edit

```
registration_code = "unique-registration-code" 

execute "Install Agent" do
  command "wget https://s3.amazonaws.com/remote-collector/agent/install_cht_perfmon.sh -O install_cht_perfmon.sh; sh install_cht_perfmon.sh 14 #{registration_code} aws; exit 0"
  ignore_failure true
  not_if "test -d /opt/cht_perfmon"
  only_if { node.attribute?(:ec2) }
end
```
<hr/>
