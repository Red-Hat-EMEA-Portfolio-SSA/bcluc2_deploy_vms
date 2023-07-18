
## autounattend.xml

To generate a modified ISO with unattended installation for Windows:

- Download Windows Server 2019 ISO
- Mount the iso on a Linux box
- Copy the mounted content in a writable directory
- Add the autounattend.xml file to the directory root
- Remove files to skip the initial prompt in setup
- Regenerate the ISO

    sudo mkdir {/windows,/windows_iso}
    sudo mount windows-server-2019.iso /windows
    sudo cp -r /windows /windows_iso
    sudo umount /windows
    sudo rm -rf /windows_iso/efi/microsoft/boot/{efisys.bin,cdboot.efi}
    sudo mv /windows_iso/efi/microsoft/boot/efisys_noprompt.bin /windows_iso/efi/microsoft/boot/efisys.bin && mv /windows_iso/efi/microsoft/boot/cdboot_noprompt.efi /windows_iso/efi/microsoft/boot/cdboot.efi
    sudo cp autounattend.xml /windows_iso
    genisoimage   -no-emul-boot -b "boot/etfsboot.com" -boot-load-size 8 -eltorito-alt-boot -no-emul-boot -e "efi/microsoft/boot/efisys.bin" -boot-load-size 1 -iso-level 4 -o "windows-unattended.iso"

The setup creates the admin user **Administrator/redhat** and configures *WinRM* to connect to the machine with Ansible

## deploy_webserver.yml

No Variable is needed for it, just replace the *logo.png* with an actual logo to show.

## deploy_iis.yml

No Variable is needed for it, just replace the *logo.png* with an actual logo to show.

## prepare_iso.yml

podman push ghcr.io/pablo-preciado/bcl_demo:vmware_rest_ee

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
