# About

Here are my notes for building and EC2 image on Amazon Web Services (AWS). These notes are created specifically for creating a enviroment that uses a Jupyter Notebook Server to work with deep learning algorithms in Python with Tensorflow. There is also information for installing bioinformatic tools to handle sequencing data. 

## Volumes

After setting up the EC2 image on AWS, you need to attach volume so you have disk space.  The volume size is super flexable, whatever you put on the volume is there and can be attached and detached from any image. Think of as a flash drive.  You pay for the size of the volume as long as it is active. If you only want to pay for the storage you are using, you can take a snapshot, then shut down your volume. You pay for snapshot size. The snapshot can then be restarted on a new volume.

**Steps for attaching volume**

1. Start old snapshot as a new volume
1. Attach volume to instance in gui
2. **ONLY FIRST TIME USING IT**: Create file system on your volume: `sudo mkfs -t ext4 device_name`.  I ran into a bug in which this doesn't work, but this does: `mkfs -t ext4 -L rootfs device_name`
3. `sudo mount device_name mount_point` (example `sudo mount /dev/xvdf data`
4. change permission for directory: `sudo chmod 777 -R mount_point`

**Useful commands**
- `lsblk` - look at your volumes
- `sudo file -s /dev/xvdf` ask if your volume is formatted. If output is data then it is *not* formatted.
- `df` - ask how much space
- `df -Th` 

\* There are ways to assign a volume to an instance and automatically attach at every bootup. But currently not using these, but def useful in future.

**Steps for detaching volume**

1. take snapshot of your volume
2. unmount: `umount -d /dev/sdh`
3. detach volume
4. delete volume

If you change the size of the volume you attach, you have to tell your image to resize the image or it only sees the previous size.

`resize2fs /dev/xvda1`

### More info on Volume
[attaching Volumnes](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)

[Volume Costs](https://forums.aws.amazon.com/thread.jspa?threadID=245389)

## Connecting to your image

ssh

Example:
`ssh -i key.pem ubuntu@ec2-54-67-80-121.us-west-1.compute.amazonaws.com`

* don't forget the `ubuntu@`

## Connecting to Rstudio

Just go to server address. Make sure you change password right away.

## Connecting to Ijupyter Notebook  (Python and Julia)

`<server IP>/julia`

## Shiny

`<server IP>/shiny/rstudio`

## Transfering files

Make sure your final directory has the right permissions.

Example: `sudo scp -r -i ~/Downloads/aws2.pem final_assemblies_10_15_17/ ubuntu@54.183.227.160:/home/data`

# Image Add ons

## Bedtools install

`apt-get install bedtools`

## BLAST

[Installization](https://www.ncbi.nlm.nih.gov/books/NBK52640/)

but then I just did this: `sudo apt install ncbi-blast+`

## Bedtools

Followed these directions: 
`wget https://github.com/arq5x/bedtools2/releases/download/v2.25.0/bedtools-2.25.0.tar.gz`

## seqmagick for file 
pip install seqmagick

## BioAwk

Install:

`apt-get install bison`
`git clone https://github.com/lh3/bioawk`
`cd bioawk `
`sudo make`
`sudo cp bioawk /usr/local/bin/`

## Seqtk

Install:

`sudo git clone https://github.com/lh3/seqtk.git`
`cd seqtk` 
`sudo make`
`sudo cp seqtk /usr/local/bin/`

## gnome-system-monitor

`sudo apt install gnome-system-monitor`

## fastx-toolkit

`sudo apt insall fastx-toolkit`

## TrimAL

`git clone https://github.com/scapella/trimal.git`
`cd trimal /usr/local/bin`

## Samtools

`samtools faidx test.fa two`

## BBmap

`sudo wget https://sourceforge.net/projects/bbmap/files/latest/download`
`tar -xvzf download`

## Homebrew

`sudo apt install linuxbrew-wrapper`

## Parsimonator

`sudo wget https://sco.h-its.org/exelixis/resource/download/software/Parsimonator-1.0.1.tar.bz2`
`sudo tar -vxjf Parsimonator-1.0.1.tar.bz2`
`cd Parsimonator-1.0.1`
 `make -f Makefile.gcc`

## Fasta Utilities
(pretty useless)

`sudo git clone https://github.com/jimhester/fasta_utilities.git`
`sudo perl Makefile.PL PREFIX=$HOME` (there was some prereqs missing
`sudo make`
`sudo make install`

## Dat

`sudo npm install -g dat`

## Questions

What is the difference between stopping and terminating the instance?

> When you stop an instance, we shut it down. We don't charge usage for a stopped instance, or data transfer fees, but we do charge for the storage for any Amazon EBS volumes. [ref](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html)

## Contacting and Research at UC Berkeley

### With BearBuy

Called Bearbuy: Phone: (510) 664-9000 option 1, then option 2

Contact?: 	aws-uc-procurement@amazon.com

Storage:
[block Storage for EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)

## Comparing Plans:

https://aws.amazon.com/premiumsupport/compare-plans/

keycode: https://aws.amazon.com/premiumsupport/compare-plans/

## Resources

Basically, I followed this [great medium article](http://strimas.com/r/rstudio-cloud-1/) and [This dude](http://www.louisaslett.com/RStudio_AMI/) created and maintains a sweet Image with everything you need.

- [H20 on AWS](http://amunategui.github.io/h2o-on-aws/)
- [Installing Anaconda](https://medium.com/@GalarnykMichael/aws-ec2-part-3-installing-anaconda-on-ec2-linux-ubuntu-dbef0835818a)
- [setting up and using jupyter on aws](https://medium.com/towards-data-science/setting-up-and-using-jupyter-notebooks-on-aws-61a9648db6c5)
- [Lazy girls guide](https://kenophobio.github.io/2017-01-10/deep-learning-jupyter-ec2/)
- [Blog on Setting up Rstudio in AWS](http://strimas.com/r/rstudio-cloud-1/  )
- [Building Genomics Workflows on Amazon Cloud](https://aws.amazon.com/blogs/compute/building-high-throughput-genomics-batch-workflows-on-aws-introduction-part-1-of-4/)
- [Intro to cloud computing](https://github.com/griffithlab/rnaseq_tutorial/wiki/Intro-to-AWS-Cloud-Computing)
- [Create Rstudio from scratch](http://randyzwitch.com/r-amazon-ec2/)



