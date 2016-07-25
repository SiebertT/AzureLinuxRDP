# Using XRDP with LXQT desktop environment on Ubuntu 14.x Azure Server #

----------

In this tutorial you will be guided through the steps to be able to RDP to your Ubuntu 14.x Azure Servers GUI. 

After doing this you will be able to access your server anywhere through Windows RDP and configure your server through the GUI or use it as a workstation as you please.

I found it annoying I could only use my Azure Linux VM's through the CLI so I figured this out for others as well. 

## Prerequisites ##
1. You have created an Ubuntu Server 14.x Virtual Machine on Azure (preferably a clean one to avoid problems).
2. You have a telnet or SSH client installed such as [PuTTY](http://www.putty.org/ "PuTTY download") or [MobaXTerm](http://mobaxterm.mobatek.net/download.html "MobaXterm download") to connect to your console for the configuration.
3. You have installed a Remote Desktop Client. In our case we use the standard Windows Remote Desktop Connection application.

## The mandatory sudo apt-get installs ##

The LXQT is not yet present in the repositories, so we add it manually with the following command

    sudo add-apt-repository ppa:lubuntu-dev/lubuntu-daily

Now we want to update our apt-get repositories.

    sudo apt-get update
    sudo apt-get upgrade

> By default the XRDP login will use an en-us keyboard. If this doesn't match yours and you want to switch it, we need a package and another command.

    sudo apt-get install x11-xkb-utils
    setxkbmap -be
the '-be' is for a Belgian keyboard. change it with one of [these](http://pastebin.com/v2vCPHjs "Keyboard Layout Values") keywords for your personal keyboard setup.

## Install your alternative desktop environment ##
We need to install a desktop environment on top of our Ubuntu that is compatible with XRDP. In this tutorial it's the LXQT desktop alternative. 

    sudo apt-get install lxqt-metapackage lxqt-session lxsession
    
> Make sure you have updated your apt-get repository!

## Install the XRDP package + configuration ##
Now we need the XRDP package in order to be able to RDP to our new desktop environment!

    sudo apt-get install xrdp 

We need to make clear to our Linux system that we want to connect to the LXQT environment through RDP.

    echo startlxqt >~/.xsession

Now we restart the service to make our configuration work. Restarting the server does the trick as well.

    sudo service xrdp restart

## Open the RDP port on Azure ##

In order for the RDP to work on an Azure server, we need to enable this kind of traffic with an inbound rule in the Security Group. The port we need is 3389.

> note that the VM in the screenshots is using the environment of MATE, don't worry about this. It's exactly the same for every environment.

1. Go to your Azure portal and navigate to your VM. Look for Network interfaces.
![](https://i.gyazo.com/fc9428da7ee7ed8bc9672d46830d0da0.png)
2. Click on the name of your Security Group, through the settings, click Network Security group once more.
![](https://i.gyazo.com/3afd29410fa1c6575a35869cb04ead6c.png)
3. Click on the security groups name, through the settings, click Inbound security rules
![](https://i.gyazo.com/936eac2a354c6e2414ff4822013688a8.png)
4. Click on Add, give your rule a name and make sure to type in the port 3389 with the Action Allow. Press OK.
![](https://i.gyazo.com/68915695214be0cd5fa2ef1e6077b025.png)
5. Wait for the rule to be made by Azure.

## Open up your RDP client, connect to the public IP or the DNS of your server ##
![](https://i.gyazo.com/82ff8d59b848977596b3195b91d47a69.png)

## Connect with your Azure VM user and PW to the XRDP session ##
![](https://i.gyazo.com/81654b2d5ff31cb61f0465eb229d62d0.png)

## Enjoy your LXQT desktop ##
![](https://i.gyazo.com/9a8953bf8754675f0c0dd51dde3a9c5e.png)

### Credits ###
Sources for parts of this walk through were found at [c-nergy](http://c-nergy.be/), check them out if you're looking for more Linux guides.


## Author
Siebert Timmermans, gitHub user: [SiebertT](https://github.com/SiebertT)

7/25/2016 5:47:03 PM 