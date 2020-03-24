# terraform-extensive-kitchen-test

This repo is example of terraform ec2 extensive kitchen test. The inspiration comes from this [wonderful guide](https://newcontext-oss.github.io/kitchen-terraform/tutorials/extensive_kitchen_terraform.html)

You can try the original project for more detailed infomation, or you can try this one.

## Requirements
- Terraform installed (version between 0.11.4 and 0.11.14)
- Ruby version >= 2.4, < 2.7
- AWS account
- AWS CLI installed and configured
- jq installed
- [Bundler installed](https://bundler.io/index.html#getting-started)
- Prepare your environment for **kitchen**
  - Type: `sudo apt-get install rbenv ruby-dev ruby-bundler`
  - add to your ~/.bash_profile: 
  ```
  eval "$(rbenv init -)"
  true
  export PATH="$HOME/.rbenv/bin:$PATH"
  ```
  - do `. ~/.bash_profile` in order to apply the changes made in .bash_profile 
  
## Running the extensive kitchen test
- clone the repo locally
```
git clone https://github.com/berchev/terraform-extensive-kitchen-test.git
```
- change to repo directory
```
cd terraform-extensive-kitchen-test
```
- generate SSH key used by inspect when authenticatig to EC2 instances. Run this command:
```
ssh-keygen \
-b 4096 \
-C "Kitchen-Terraform AWS provider tutorial" \
-f test/assets/key_pair \
-N "" \
-t rsa
```
- install bundler if not installed
```
gem install bundler
```
- Install the gem, according to Gemfile
```
bundle install
```
- Run the extensive kitchen test, using [test_bash.sh](test_bash.sh)
```
bash test_bash.sh
```
- If everything is fine, kitchen will do following:
  - provision centos machine in `us-east-1"`
  - run tests on it
  - destroy centos machine
  - provision ubuntu machine in `us-west-2`
  - runt tests on it
  - destroy ubuntu machine

## Test results:
Example tests result will looks like this:
- centos machine
```
$$$$$$ Verifying the 'local' system...

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  local://

  ✔  state_file: 0.11.14
     ✔  0.11.14 is expected to match /\d+\.\d+\.\d+/
  ✔  inspec_attributes: static terraform output
     ✔  static terraform output is expected to eq "static terraform output"
     ✔  static terraform output is expected to eq "static terraform output"


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 3 successful, 0 failures, 0 skipped
$$$$$$ Finished verifying the 'local' system.
$$$$$$ Verifying the 'remote' system...

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  ssh://centos@ec2-3-230-125-26.compute-1.amazonaws.com:22

  ✔  operating_system: centos
     ✔  centos is expected to eq "centos"
  ✔  reachable_other_host: Host 35.170.52.73

     ✔  Host 35.170.52.73
      is expected to be reachable


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 2 successful, 0 failures, 0 skipped

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  ssh://centos@ec2-34-239-227-160.compute-1.amazonaws.com:22

  ✔  operating_system: centos
     ✔  centos is expected to eq "centos"
  ✔  reachable_other_host: Host 35.170.52.73

     ✔  Host 35.170.52.73
      is expected to be reachable


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 2 successful, 0 failures, 0 skipped

$$$$$$ Finished verifying the 'remote' system.
```
- ubuntu machine
```
$$$$$$ Verifying the 'local' system...

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  local://

  ✔  state_file: 0.11.14
     ✔  0.11.14 is expected to match /\d+\.\d+\.\d+/
  ✔  inspec_attributes: static terraform output
     ✔  static terraform output is expected to eq "static terraform output"
     ✔  static terraform output is expected to eq "static terraform output"


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 3 successful, 0 failures, 0 skipped
$$$$$$ Finished verifying the 'local' system.
$$$$$$ Verifying the 'remote' system...

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  ssh://ubuntu@ec2-52-39-211-20.us-west-2.compute.amazonaws.com:22

  ✔  operating_system: ubuntu
     ✔  ubuntu is expected to eq "ubuntu"
  ✔  reachable_other_host: Host 52.43.191.116

     ✔  Host 52.43.191.116
      is expected to be reachable


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 2 successful, 0 failures, 0 skipped

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  ssh://ubuntu@ec2-52-39-180-194.us-west-2.compute.amazonaws.com:22

  ✔  operating_system: ubuntu
     ✔  ubuntu is expected to eq "ubuntu"
  ✔  reachable_other_host: Host 52.43.191.116

     ✔  Host 52.43.191.116
      is expected to be reachable


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 2 successful, 0 failures, 0 skipped
$$$$$$ Finished verifying the 'remote' system.
```
