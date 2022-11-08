# Getting Started

Protecting the "confidentiality, integrity, and availability of data," or the <b>CIA triad</b>.<br><br>

### Setting Up a Pentest Distro

A hypervisor is software that allows us to create and run virtual machines (VMs). 
It will enable us to use our host computer to run multiple VMs by virtually sharing memory and processing resources.
VMs on a hypervisor run isolated from the primary operating system, which offers a layer of isolation and protection between our production network 
and vulnerable networks, such as Hack The Box, or when connecting to client environments from a VM (though VM breakout vulnerabilities do arise from time to time).<br>
#### ISO
```
The ISO (Optical disc image) file is essentially just a CD-ROM that can be mounted within our hypervisor of choice to build the VM by installing the operating system ourselves. An ISO gives us more room for customization, e.g., keyboard layout, locale, desktop environment switch, custom partitioning, etc., and therefore a more granular approach when setting up our attack VM.
```
#### OVA
```
The OVA (Open Virtual Appliance) file is a pre-built virtual appliance that contains an OVF XML file that specifies the VM hardware settings and a VMDK, which is the virtual disk that the operating system is installed on. An OVA is pre-built and therefore can be rapidly deployed to get up and running quicker.
```
<br>

### Folder Structure

Here we have a folder for the client Acme Company with two assessments, Internal Penetration Test (IPT) and External Penetration Test (EPT). Under each folder, we have subfolders for saving scan data, any relevant tools, logging output, scoping information (i.e., lists of IPs/networks to feed to our scanning tools), and an evidence folder that may contain any credentials retrieved during the assessment, any relevant data retrieved as well as screenshots.
