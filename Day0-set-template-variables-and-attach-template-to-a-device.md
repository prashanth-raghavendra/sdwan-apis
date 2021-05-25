# Day0 - Set template variables and attach template to device
<br>

<b>1.	Pre-requisites: </b>
<br>

* Device template must already exist in vManage.

* Template must not be already attached to the device (Day0 provisioning).
<br>

<b>2.	Create a YAML file </b>
<br>

  Let’s say variables.yaml

  Sample file below (one host-name block for each device):
  
  <br>

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

<br>

<b>3.	SDK (including installation instructions) </b>
<br>

https://github.com/CiscoDevNet/python-viptela

<br>

<b>4.	Run import command with file </b>
<br>

<i>vmanage import attachments -f variables.yaml</i>

<br>


# (Optional) Annexure - Identify the template variables using API
<br>

<b>a.	GET
<br>
/dataservice/device</b>
<br>

This API will give the UUIDs (device IDs).
<br>

Note the UUID of the device of interest.<br>

Example1: <i>CSR-24A787D5-5C83-4578-900C-57E47F5A34F8 </i>
<br>
Example2: <i>ISR4331/K9-FDO23160NNT </i>
<br><br>

<b>b.	GET
<br>
/dataservice/template/device</b>
<br>

This API will give the Device template IDs.
<br>

Note the template ID of interest.<br>

Example: <i>9a08ead2-2767-427a-b293-a1c6e90463de </i>
<br><br>

<b>c.	POST
<br>
/dataservice/template/device/config/input/ </b>
<br>

(sample) 
<br>
<i>PAYLOAD = {<br>
               "templateId":"9a08ead2-2767-427a-b293-a1c6e90463de",
<br>
               "deviceIds":["CSR-24A787D5-5C83-4578-900C-57E47F5A34F8"],
<br>
               "isEdited":false,
<br>
               "isMasterEdited":false
<br>
             } </i>
<br>
<br>

In the Response, ‘columns’ will have list of variables.
<br>

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
<br>

The variable names can be obtained from the title.
<br>

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
