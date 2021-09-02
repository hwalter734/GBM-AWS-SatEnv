# GBM-AWS-SatEnv
This repository includes the necessary files to deploy an environment that supports Red Hat Satellite. It deploys a Satellite main server and an additional Capsule server, either via solely Cloudformation, Ansible, or a combination of both.

# Pre-requisites
- **AWS Account**: If you don't have an account set up, you can click the following [link](https://www.google.com/aclk?sa=L&ai=DChcSEwiGgPC8p8jyAhUIKIYKHVqYD_4YABABGgJ2dQ&ae=2&sig=AOD64_1iTlBXbvjnjNFp_r9eqOe9SXhZvg&q&adurl&ved=2ahUKEwj97-m8p8jyAhXrQzABHePbAP8Q0Qx6BAgDEAE)
- **AWS Credentials**: You need an *aws access key* and an *aws secret key* in order to communicate with AWS. Click the following [link](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-appendix-sign-up.html) for more information.
- **AWS CLI**: You should have aws-cli setup in your system in order to replicate the following repository. Alternatively, you can also specify *aws_access_key* and an *aws_secret_key* variables if you are planning on using Ansible. More info on installing the aws cli [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- **Ansible (Optional)**: More info on installation [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) 
- **Ansible Tower (Optional)**: More info on installation [here](https://docs.ansible.com/ansible-tower/latest/html/quickinstall/index.html) 
- **SSH Key Pair**: You can create the key pair using Ansible and launching the create\_keypair\_playbook.yml. Alternatively, you can use the following command ```ssh-keygen -m PEM -t rsa -b 4096 nameofyourkey```
# Options for Deployment
This repository includes the necessary files to deploy the Cloudformation stack in a few ways.

## Option 1: Cloudformation Exclusively
The user can use the template file **satellite_env.yml** file alongside with **parameters.json** to deploy the Cloudformation stack via the CLI. Note that you can update the **parameters.json** file to the values/names which you prefer.
```
aws cloudformation create-stack \
    --stack-name sat-config \ 
    --template-body file://satellite_env.yml \
    --parameters file://parameters.json \
    --capabilities CAPABILITY_IAM \
    --region us-east-1
```

## Option 2: Cloudformation and Ansible through CLI
First, make sure to include/modify your variables in the vars.yml file. Afterwards you cant deploy the Cloudformation template using the **cloudformation\_satenv.yml**. 

## Option 3: Cloudformation and Ansible through Ansible Tower
Add a survey to your job template to include all the variables necessary to properly deploy the Cloudformation template. Afterwards you can use the **tower\_cloudformation\_satenv.yml** to deploy the Cloudformation template.

## Option 4: Ansible or Ansible Tower 
In case you are using Ansible Tower, make sure to add a survey to your job template to include all the variables necessary for the playbook to work. Afterwards you can launch the **tower\_sat\_env\_playbook.yml**. If you are using Ansible in the CLI, update **vars.yml** file with your desired values and you can use **sat_env_playbook.yml** to deploy the environment in AWS.

In case you are using Ansible Tower, you must add your AWS credentials in the **Credentials** section of your Ansible Tower.
![aws_credentials](screenshots/credentials.png?raw=true) 

# Why use Cloudformation?
Cloudformation works based on stacks, which helps you organize your resources as well as the steps necessary to set up your environment. This also provides the advantage that you can delete your stacks, in case a phase of your development goes wrong, a modification needs to take place or you simply want all items within the stack to be deleted.

# Why use Ansible?
Ansible is a powerful automation tool that can facilitate your manual tasks in an orderly fashion, while mantaining an easy to read file format (.yml). For those with more experience with Ansible, you can combine both Cloudformation with Ansible or keep the environment working solely with Ansible.

# Conclusion
The best method is the one that works best for you and your needs.