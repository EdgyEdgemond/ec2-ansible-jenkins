# Spin up Jenkins on AWS EC2 using Ansible

## Introduction

This is an [Ansible](http://ansibleworks.com) playbook to install and run the
[Jenkins CI](http://jenkins-ci.org/) system on an 
[AWS EC2](http://aws.amazon.com/ec2) instance.

Jenkins is started as its own service, and not proxied via a web server. 

## Running

Naturally, you need [Ansible](http://ansibleworks.com) installed on your
host system before using this.

You will also need to export AWS_ACCESS_KEY and AWS_SECRET_KEY as environment
varibles before running the playbook. These are your AWS access values.

Then simply run ``ansible-playbook main.yaml``.

## Details

Note this play is written to run on both deb/rpm based distro and it was tested on: `Ubuntu 12.04, Debian 6, Debian 7, CebtOS 6.3, CentOS 5.5, RedHat 6.4`. Additionally, it also makes some assumptions based on the AMI used - the Amazon Linux AMI already has a
usable Java installed, so we don't need to handle that here.

The first part of the play, the ec2.yaml file, will spin up a single, 
[t1.micro](http://aws.amazon.com/ec2/instance-types/#instance-details),
[Amazon Linux](http://aws.amazon.com/amazon-linux-ami/) instance in eu-west-1.
Variables to control these elements are defined at the top of the playbook, in
the vars section. You can change these as required, either in the file or on
the command line with --extra-vars passed to ansible-playbook.

If you have DNS in [Route53](http://aws.amazon.com/route53/) the play can also
create a DNS entry. By default this is not set to run, but specify
``--extra-vars="set_dns=True dns_zone=ZONENAME"`` at runtime and an entry
``ci.ZONENAME`` will also be created.

*NOTE*: The first part of the play, 'ec2.yaml', is not idempotent. Every time
it is run, it will create a new EC2 instance.


