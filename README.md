# bcl_use_case2

## Action plan:

Create the following roles:

 - RHEL custom ISO creation > we have it
 - Preparation of Windows ISO with WinRM enabled > Ale has something
 - Create VMware templates from custom ISO's > we cannot test it at the moment
 - Create VM from templates > Pablo
 - Configure web server > Amaya

## autounattend.xml

To generate a modified ISO with unattended installation for Windows:

- Download Windows Server 2019 ISO
- Mount the iso on a Linux box
- Copy the mounted content in a writable directory
- Add the autounattend.xml file to the directory root
- Regenerate the ISO

    sudo mkdir {/windows,/windows_iso}
    sudo mount windows-server-2019.iso /windows
    sudo cp -r /windows /windows_iso
    sudo umount /windows
    sudo cp autounattend.xml /windows_iso
    genisoimage   -no-emul-boot -b "boot/etfsboot.com" -boot-load-size 8 -eltorito-alt-boot -no-emul-boot -e "efi/microsoft/boot/efisys.bin" -boot-load-size 1 -iso-level 4 -o "windows-unattended.iso"

The setup creates the admin user **Administrator/redhat** and configures *WinRM* to connect to the machine with Ansible

## deploy_webserver.yml

No Variable is needed for it, just replace the *logo.png* with an actual logo to show.

## deploy_iis.yml

No Variable is needed for it, just replace the *logo.png* with an actual logo to show.

## prepare_iso.yml


| Variable | Subelements | Description | Required | Default | Notes |
| -- | -- | -- | -- | -- | -- |
| ks_lang                 | - | System language                                 | :x: | en_US | |
| ks_keyboard             | - | Keyboard layout                                 | :x: | us | |
| ks_timezone             | - | Timezone                                        | :x: | Europe/Amsterdam | |
| ks_rootpw               | - | Plaintext password for the root user            | :x: | redhat | |
| ks_disk                 | - | Block device to be used for the installation    | :x: | sda | |
| ks_hostname             | - | Guest hostname                                  | :x: | N/A | Mandatory if primary interfaces is static |
| interfaces              | - | List of configured interfaces                   | :heavy_check_mark: | N/A |
| - | network_device    | Device name                                     | :heavy_check_mark: | N/A | |
|| ip                | IP of the interface                             | :x: | N/A | If set, other fields are mandatory for static configuration. If not set, DHCP will be used
|| netmask           | Netmask of the interface                        | :x: | N/A | Mandatory if *ip* is static
|| gateway           | Gateway for the interface                       | :x: | N/A | Mandatory if *ip* is static
|| nameserver        | Nameserver for the interface                    | :x: | N/A | Mandatory if *ip* is static
|| primary           | Boolean to check if interace is primary         | :x: | N/A | Mandatory if *ip* is static. If true, *ks_hostname* must be set
|| vlanid            | Vlan ID for the device                          | :x: | N/A | Mandatory if *ip* is static


