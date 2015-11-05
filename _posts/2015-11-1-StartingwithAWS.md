---
layout: post
title: Getting Started with AWS and the Survox Platform
---

Ok, we made it back from another successful users' summit. It was a great opportunity to meet with many of our users and share what we've been up to over the past year. One of the topics that generated quite a bit of interest was the talk given by Ken Keyes on our use of a variety cloud based services and automation tools. We spoke with several users after the presentation and there were many questions about how to get started investigating this on their own. So, we decided to start sharing what we've learned with out development community by posting to this site. We hope you find it useful in your own exploration of the various technologies available to assist you in building a world class data collection and analysis platform. 

Enough pre-amble, we're going to talk a little about Amazon Web Services (AWS) in this post. AWS is one of many providers of 'on-demand' programable infrastructure. We selected AWS based largely on the breadth of services they offer, the amount of time they've been in the space, and the availability of user facing interfaces for their services. There are many other great providers out there but many of them relied more heavily on programming interfaces for almost all operations and the ability to interact with the services through a user interface was high on our list. 

Our first goal was working out how we could get the Survox platform running 'in the cloud.' To do this, we started working with the AWS service known as Elastic Compute Cloud (EC2). EC2 is the service AWS offers for quickly obtaining and configuring servers in the cloud. We would recommend you take a quick look at this page: [AWS Docs - Launching an Instance](https://aws.amazon.com/ec2/getting-started/)  there are lots of quick-starts and tutorials you can find there. AWS also offers a free tier for the first 12 months, check out the following link: [AWS Free Usage Information](http://aws.amazon.com/free/) - this is a great way to try out many of Amazon's services.

###Some key concepts
- ***Amazon Machine Image (AMI -pronounced ahh-me)*** - is the base image you’ll use to create your server. Whatever is on your AMI when you created it, will be available on each instance you create from that AMI. 
- ***Instance*** - This is a server that you have provisioned based on an AMI. 
- ***Instance State*** - An instance state determines whether the server is running, stopped, rebooting, or being retired. 
Virtual Private Cloud (VPC) - This is a virtual network you can create and configure. This is where you’ll launch AWS resources and control whether your resources can access the internet or not. 
- ***Security Group*** - This is a critical concept to make certain you pay attention to. Think of a security group as a firewall for your server. The security group determine what traffic is allowed to access your server. The default security group created for an instance is open to all, so you’ll want to be careful and make sure you take some time to configure a more restrictive security group. 
- ***Private & public IP addresses*** - You’ll always get a private IP address for each instance you create. A private address is not accessible over the internet and is only reachable over the same network. You can find out more about networks here: [AWS VPC](https://aws.amazon.com/vpc/)
- ***Availability Zone*** - This is where you’ll spin up your instance. Amazon has many availability zones available for you to use. This 
- ***Tags***s - These are user defineable tags that can be assigned to many AWS resources. They are very useful once you've started building up multiple instances and you're trying to figure out what everything is being used for. We'd recomend thinking about a standard tagging scheme that makes sense for your needs and using it from day 1. 

So, let's try getting an instance spun up and install the Survox platform on it. You remember how we mentioned that you don't want t use the default Amazon images (there issn’t a 6.6 version of either Redhat or CentOS available at the time we’re writing this), so what should you do? 

There are several options, but we’re going to create our AMI from a virtual machine (VM) that we created using VMWare fusion. If you’re reading this, you probably already know a bunch about creating a virtual machine. However, if you’re looking for more information, you can find it here [VMWare Education](http://mylearn.vmware.com/mgrreg/index.cfm)

You’re going to want a VM that contains the operating system you intended to use (since we’re going to install the 8.8.6 version of the software, you’ll want to install RedHat 6.6or CentOS 6.6). You’ll also want follow whatever company practices you have for 'hardening' the security of a server (disable root access, )

You’ll need to install a few tools on your local system:
The AWS command line interface (CLI) tools. You can find the instuctions here: [AWS Command Line Tools](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) 
The  Open Virtualization Format (OVF) Tool. You can download the tool and find the documentation here. [Ovf Tool](https://www.vmware.com/support/developer/ovf/) 

Once these tools are both installed you’re ready to move forward 

This gets a little odd depending how you install the OVF tool and how you name your VM. First thing to realize, when you see a file named somefile.vmwarevm this is actually a directory not a file (the OS makes it look like a file in some cases.) So you end up with a fairly long command to reference the files you need correctly. 

{% highlight bash %}
ovftool "/users/usname/Documents/Virtual Machines.localized/CentOS 64-bit.vmwarevm/CentOS 64-bit.vmx" "/users/usname/VMs/devops-cos66.ovf"
{% endhighlight %}

Now that you have the OVF file you can upload the image to Amazon using the cli tools. The '-o' option is used for your access key ID. If AWS_ACCESS_KEY isn't set, you must specify this option. The '-w' option is used for your secret access key. Default: The value of the AWS_SECRET_KEY environment variable. If AWS_SECRET_KEY isn't set, you must specify this option.

{% highlight bash %}
ec2-import-instance /users/usname/VMs/devops-cos66-disk1.vmdk -f vmdk -a x86_64 -t t2.small -b devops-base-images -o AAAAAAAA -w SSSSSSSSS -region us-west-2 -p Linux
{% endhighlight %}

It's worth noting that many of the command line options can be set as environment variables if you’re going to be doing this frequently. If you're interested in trying to set the environment variables as opposed to using the command line switches, you can find information about doing so here: [AWS CLI Environment Variables](http://) 

The ec2-import-instance command will copy the VM to the S3 bucket you specified. Once completed, you can check on the task by using ec2-describe-conversion-tasks with the task ID provided by the ec2-iport-instance command. 

{% highlight bash %}
ec2-describe-conversion-tasks import-i-fh3vidy9
{% endhighlight %}

Once the process finishes, go ahead and log into the instance. You'll want to make sure you've updated the security group to limit access to the system. If everything looks ok, go the the AWS EC2 console and select the instance you created. From the 'Actions' button, select 'Image', 'Create Image' and follow the on screen prompts. Once the process completes, you should have a new AMI you can re-use. 

Once you have an AMI created go ahead an launch it - [AWS Docs - Launching an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html)

Ok, now you can access that instance like you would any other machine. Let's go ahead and login via ssh <include steps>

Look at that - you have your first machine in the cloud, congratulations!

Now you're ready to install the survox platform. This is really no different than installing on any other server. You can find the steps for installing the software right here: 

ok, let's see if it worked? Try <url here>
blah blah - then explain what they have to do with the survoxconsole.conf

ok, let's try again

viola!

You now have a study server and survox console up and running and ready to start deploying surveys for you. A couple of important things to remeber: 

1 - If you restart the machine, you will end up with a different IP address. 
2 - Mac Address

So, what could you do with this? 
reliability
creating additional AMI (don't forget about the validate string)

What's next..
