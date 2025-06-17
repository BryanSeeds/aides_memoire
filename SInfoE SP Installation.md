## A capture of my efforts to install an SInfoE AIS Producer SP.

# Plan A - Vagrant Install

As the SInfoE repo is private, we need to use a GitHub personal access token (CLASSIC) with the appropriate scope (repo) to clone a local copy.
(https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

git clone https://GITHUB_USERNAME:GITHUB_PAT@github.com/SCP-Development/mdis-vagrant.git

The SP is provided as a complete demonstration environment within a Vagrant image, and therefore Vagrant is a prerequisite. 
(https://developer.hashicorp.com/vagrant/install)

There are several quirks with the poor HashiCorp documentation, but that's not a battle to document here. It can be done. Even the VMware plug-in.

The end of this adventure however is a virtualisation host error. Our corporate Dev laptops have secure boot enabled, and no access to the UEFI is permitted. It is not possible to sign drivers (https://knowledge.broadcom.com/external/article?legacyId=2146460) and so vmmon and vmnet cannot load.
A similar issue occurs with VirtualBox.


# Plan B - Docker Install

TBC.../
