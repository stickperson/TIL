# OpenWRT Custom DNS
## Use Case
I want to route all traffic for a specific interface through pi-hole.

## Instructions
OpenWRT should be using `dnsmasq`. To set the DNS, we need touse custom DHCP-Options.

Navigate to the interfaces page.

![Interfaces](./select_interfaces.png)

Select the interface you would like to edit

![Edit interface](./edit_interface.png)

Scroll down to the DHCP section and select "Advanced"

![DHCP](./advanced_dhcp.png)

Set the custom DNS. We want to use option 6, so the syntax is `6,<address1>,<address2>` etc.

![Custom Options](./set_dns.png)

Click "Save and Apply." All done!
