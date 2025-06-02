## CORE RJ45 within Docker

Using the RJ45 Link Layer Node within the CORE Network Emulator to connect to the outside world is straightforward enough, unless you are running CORE within a Docker container. That needs a couple of extra steps, for which the documentation is sparse and unnecessarily complicated. Hence this guide.

CORE should already be installed in a Docker container (if not, follow a tutorial such as this one: https://coreemu.github.io/core/install_docker.html).

To check that the CORE container is available, we can list all installed Docker containers with:

*sudo docker ps -a*

Assuming it is, we first need to start the CORE Emulator within Docker:

*sudo docker start core*

then enable xhost access, so we can run core-gui from the core container:

*xhost +local:root*

and finally start the core-gui:

*sudo docker exec -it core core-gui*

Done. Except if you connect a PC Container Node to an RJ45 Link Layer Node and try to ping the real world it's unlikely to work. There are two things to be aware of:
1. Although Docker has bridge networking enabled by default, it uses the private IP address range of 172.17.0.0/16. The PC Container Node must be in this same subnet, or connected to the RJ45 Link Layer Node via a router.
2. For security, Ubuntu has IP Forwarding disabled by default, and this needs to be enabled:
   
*sudo su*

*echo 1 > /proc/sys/net/ipv4/ip_forward*

Note that this is a temporary change until reboot, to enable permanent IP Forwarding, edit the */etc/sysctl.conf* file instead (not recommended).

And that's it. The PC Container Node should now be able to successfully reach a real world location via the RJ45 Link Layer Node.

![Alt text](https://github.com/BryanSeeds/aides_memoire/blob/CORE-Emulator-RJ45-in-Docker/core_rj45_ping.jpg "CORE GUI Screenshot")
