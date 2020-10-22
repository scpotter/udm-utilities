# Run AdguardHome on your UDM

## Features

1. Run AdguardHome on your UDM with a completely isolated network stack.  This will not port conflict or be influenced by any changes on by Ubiquiti
2. Persists through reboots and firmware updates.

## Requirements

1. You have setup the on boot script described [here](https://github.com/boostchicken/udm-utilities/tree/master/on-boot-script)
1. AdguardHome persists through firmware updates as it will store the configuration in a folder (you need to create this). It needs 2 folders, a Work and Configuration folder. Please create the 2 folders in "/mnt/data/". In my example I created "AdguardHome-Confdir" and "AdguardHome-Workdir"

## Customization

* Tip: Create a branch with customizations
    * Update [20-dns.conflist](../cni-plugins/20-dns.conflist) if you want to use an alternate IP address for the container
    * Update [10-dns.sh](../dns-common/on_boot.d/10-dns.sh) with your own values
    * Readme updates as appropriate
* If you want IPv6 support use [20-dnsipv6.conflist](../cni-plugins/20-dnsipv6.conflist) and update [10-dns.sh](../dns-common/on_boot.d/10-dns.sh) with the IPv6 addresses. Also, please provide IPv6 servers to podman using --dns arguments.

## Steps

1. Fork or clone and make Customizations
1. On your controller, make a Corporate network with no DHCP server and give it a VLAN. For this example we are using VLAN 5.
1. Copy customized [10-dns.sh](../dns-common/on_boot.d/10-dns.sh) to /mnt/data/on_boot.d 

   ```bash
    curl -L https://raw.githubusercontent.com/scpotter/udm-utilities/master/dns-common/on_boot.d/10-dns.sh -o /mnt/data/on_boot.d/10-dns.sh
    ```
1. Execute /mnt/data/on_boot.d/10-dns.sh

   Update permissions if needed:

   `
   chmod +x /mnt/data/on_boot.d/10-dns.sh
   `

1. Copy customized [20-dns.conflist](../cni-plugins/20-dns.conflist) to /mnt/data/podman/cni.  This will create your podman macvlan network
    ```bash
    curl -L https://raw.githubusercontent.com/scpotter/udm-utilities/scpotter-custom-config/cni-plugins/20-dns.conflist -o /mnt/data/podman/cni/20-dns.conflist
    ```
1. Run the AdguardHome docker container, be sure to make the directories for your persistent AdguardHome configuration.  They are mounted as volumes in the command below.

    ```shell script
    mkdir /mnt/data/AdguardHome-Confdir
    mkdir /mnt/data/AdguardHome-Workdir
    
    podman run -d --network dns --restart always  \
        --name adguardhome \
        -v "/mnt/data/AdguardHome-Confdir/:/opt/adguardhome/conf/" \
        -v "/mnt/data/AdguardHome-Workdir/:/opt/adguardhome/work/" \
        --dns=127.0.0.1 --dns=9.9.9.9 \
        --hostname adguardhome \
        adguard/adguardhome:latest
    ```

1. Browse to 10.0.5.3:3000 (or your custom ip) and follow the setup wizard
1. Update your DNS Servers to 10.0.5.3 (or your custom ip) in all your DHCP configs.
1. Access the AdguardHome like you would normally.
