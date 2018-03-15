# Raspbian-Ansible-Secure-Config
Ansible Hardened Configurations for Raspbian PI3

# Goal
The goal of this project is to use Ansible for managing the initial security configs of freshly deployed Raspberry Pi3s using Pi-Bakery recipies. Security, Security, Security. Do you know the number of botnets looking for default and nonsecure raspberry Pis? Please learn something from these settings, and update your servers accordingly. "What good is knowledge, if not passed on?"
# OS Considerations
This repo applies to Rasbian Stretch full distro deployments.
# Process Workflow
We will have Pi-Bakery recipes to create the SD Card images with the following on first boot:
Change (pi)'s default password to a non-dictionary 18 character password.
Setup new default admin account (PADM.insertusername)other than root and pi, but mirroring (pi)'s permissions and group memberships.
Provide Wifi network security config for Ansible to connect to [NewConfig] hosts.
bring wlan down
Change hostname to a standard name that ansible will pick up under [NewConfig] Role, to further harden the system.
bring wlan up
rebuild resolv.conf to find private DNS servers
rebuild hosts to include new hostname
enable ssh for (PADM.insertusername)
reboot

Ansible takes over applying Role [NewConfig].
After [NewConfig] processes, reboots, Ansible uses Role [CommonConfig] to ensure the security config is applied to all servers.

# Caveats
This is a work in progress as we implement this workflow. Please test and assist as we develop this repo.
If this project interests you, please get involved.
Thank You,
-=HeroesInExile=-
