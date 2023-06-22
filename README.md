# bcl_use_case2

## Action plan:

Create the following roles:

 - RHEL custom ISO creation > we have it
 - Preparation of Windows ISO with WinRM enabled > Ale has something
 - Create VMware templates from custom ISO's > we cannot test it at the moment
 - Create VM from templates > Pablo
 - Configure web server > Amaya

## deploy_webserver.yml

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
