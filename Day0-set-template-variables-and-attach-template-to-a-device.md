# Day0 - Set template variables and attach template to device


1.	Pre-requisites:

*Device template must already exist in vManage.
*Template must not be already attached to the device (Day0 provisioning).


2.	Create a YAML file 

*Let’s say variables.yaml

*Sample file below (one host-name block for each device):

    vmanage_attachments:
    -   host_name: site1-cEdge1
        device_type: vedge
        uuid: CSR-82DEC3C6-3A28-B866-6F4A-40BEA274CA00
        system_ip: 1.1.1.4
        site_id: '100'
        template: site1_cedge_template
        variables:
            test_banner_login: 'Hello Login site100'
            test_banner_motd: 'Hello MOTD site100'
            vpn_512_if_name: GigabitEthernet8
            vpn_512_if_description: VPN512-interface
            vpn_512_if_ipv4_address: 192.168.1.8/24
            vpn_0_if_name: GigabitEthernet1
            vpn_0_if_description: VPN0-interface
      system/system-ip: 1.1.1.4
      system/site-id: ‘100’
    -   host_name: site1-cEdge2
        device_type: vedge
        uuid: CSR-82DEC3C6-3A28-B866-6F4A-40BEA274CA20
        system_ip: 1.1.1.5
        site_id: '120'
        template: site1_cedge_template
        variables:
            test_banner_login: 'Hello Login site120'
            test_banner_motd: 'Hello MOTD site120'
            vpn_512_if_name: GigabitEthernet6
            vpn_512_if_description: VPN512-interface
            vpn_512_if_ipv4_address: 192.168.2.8/24
            vpn_0_if_name: GigabitEthernet2
            vpn_0_if_description: VPN0-interface
      system/system-ip: 1.1.1.5
      system/site-id: ‘120’



3.	SDK (including installation instructions)

*https://github.com/CiscoDevNet/python-viptela


4.	Run import command with file 

*vmanage import attachments -f variables.yaml



# (Optional) Annexure - Identify the template variables using API


a.	GET
/dataservice/device

*This API will give the UUIDs (device IDs).

*Note the UUID of the device of interest.

*Example1: CSR-24A787D5-5C83-4578-900C-57E47F5A34F8
*Example2: ISR4331/K9-FDO23160NNT


b.	GET
/dataservice/template/device

*This API will give the Device template IDs.

*Note the template ID of interest.

*Example: 9a08ead2-2767-427a-b293-a1c6e90463de


c.	POST
/dataservice/template/device/config/input/

(sample) 
PAYLOAD = 
{"templateId":"9a08ead2-2767-427a-b293-a1c6e90463de",
  "deviceIds":["CSR-24A787D5-5C83-4578-900C-57E47F5A34F8"],
  "isEdited":false,
  "isMasterEdited":false}


*In the Response, ‘columns’ will have list of variables.


    property	"//banner/login"
    title	"Login Banner(test_banner_login)"

    property	"//banner/motd"
    title	"MOTD Banner(test_banner_motd)"

    property	"/512/vpn_512_if_name/interface/if-name"
    title	"Interface Name(vpn_512_if_name)"

    property	"/512/vpn_512_if_name/interface/description"
    title	"Description(vpn_512_if_description)"

    property	"/512/vpn_512_if_name/interface/ip/address"
    title	"IPv4 Address/ prefix-length(vpn_512_if_ipv4_address)"

    property	"/0/vpn_0_if_name/interface/if-name"
    title	"Interface Name(vpn_0_if_name)"

    property	"/0/vpn_0_if_name/interface/description"
    title	"Description(vpn_0_if_description)"

    property	"//system/host-name"
    title	"Hostname(host-name)"

    property	"//system/system-ip"
    title	"System IP(system-ip)"

    property	"//system/site-id"
    title	"Site ID(site-id)"


*The variable names can be obtained from the title.

    test_banner_login
    test_banner_motd
    vpn_512_if_name
    vpn_512_if_description
    vpn_512_if_ipv4_address
    vpn_0_if_name
    vpn_0_if_description
    host-name
    system-ip
    site-id