# UniFi Simple [Traffic] Rules Demystified

Simple Rules [formerly Traffic Rules] provide a higher level interface for configuring firewall policy that is applied to traffic between VLANs and from VLANs to the Internet.

## Rule Syntax
Traffic Rules have four main components that define:

- What the rule does:
    - Action
- What the rule effects:
    - Source
- What the rule applies to:
		- Destination
- When the rule is active:
    - Schedule



## Rule Ordering
Traffic Rules are automatically ordered and the ordering is updated as new rules are added.

The top level grouping is based on the selection in the **Device/Network** field.

**At** this level Rules are grouped by what they apply to, ordered from specific to general.

1. Clients
2. Networks
3. All Devices  / All Networks

Within each top level group rules are ordered by the *type of filtering* used by each **Category**.

1. DPI based - Apps, App Groups
2. IP based - Domain Name, IP Address, Internet, Local Network
3. GeoIP based - Region

And finally rules within each *type of filtering* are evaluated.

1. Allow
2. Speed Limit [see note “Speed Limiting Rules”]
2. Block

Both Allow and Block short circuit evaluation of Rules. As soon as a rule matches a decision is made on the basis of the matching rule.

### Speed Limiting Rules
Current behaviour of Speed Limit rules is to add an Allow rule at the same Device/Network > Category the speed limit applies to.

### Domain Names
The Domain Names target uses an option in dnsmasq that updates specified ipsets with resolved IP addresses when a domain name lookup matches a user defined list. The iptables firewall rules created by Traffic Rules then use the ip addresses stored in ipsets to block client traffic from accessing the domain names.


#### Important Note
The ipsets populated from DNS lookups are cleared when Traffic Rules are added or updated. If you are testing Rules that use the Domain Name category the operating system DNS cache will prevent the ipsets from updating correctly until the cached record expires.

To avoid misleading test results use ```dig``` or ```nslookup``` to request the domain name. This bypasses the local cache and will tigger an update of the ipsets.
