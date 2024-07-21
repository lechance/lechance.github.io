---
title: Linux Iptables
date: 2011-03-11
categories: Linux
tags:
- linux
- iptables
---

## Iptables glossary:

**TARGET**: When using iptables, a target is an action you want Iptables to apply when a packet matches a rule.

**CHAIN**: A chain is a list of rules; available built-in chains are: **INPUT, OUTPUT, FORWARD, PREROUTING, and POSTROUTING**.

**TABLE**: Tables are iptables features for each purpose. For example, there is a table for routing tasks and another table for filtering tasks; each table contains rule chains.

Available tables are filter, nat, raw, security, and mangle. Each table contains built-in (rule) chains. The following list shows what chains include each table:

| Tables |            |      -      |    -    |   -   |    -    |
| :----: | :--------: | :---------: | :-----: | :---: | :-----: |
| FILTER |    INPUT   |    OUTPUT   | FORWARD |       |         |
|   NAT  | PREROUTING | POSTROUTING |  OUTPUT |       |         |
|   RAW  | PREROUTING |    OUTPUT   |         |       |         |
| MANGLE | PREROUTING | POSTROUTING |  OUTPUT | INPUT | FORWARD |

Depending on the action you want iptables to do, you need to specify a table using the option -t followed by the table name. In this tutorial, the option -t isn’t used. This tutorial focuses on filtering purposes using the filter table applied by default when the option -t isn’t passed. While reading this tutorial, you will learn some of the concepts mentioned above.

## Getting started with Iptables:

Before starting, check for previous rules by instructing iptables to list existing policies and rules using the -L (–list) parameter.

    sudo iptables -L

The output above shows 3 rows: **Chain INPUT, Chain FORWARD, and Chain OUTPUT**. Where **INPUT** refers to policies regarding incoming traffic, **OUTPUT** refers to policies applied to outgoing traffic, and **FORWARD** refers to routing policies.

The output also shows there are no defined rules, and all defined policies are accepted.

There are 3 types of policies: **ACCEPT, REJECT and DROP**.

The policy **ACCEPT** allows connections; the policy **REJECT** refuses connections returning an error; the policy **DROP** refuses connections without producing errors.
When using **DROP**, **UDP** packets are dropped, and the behavior will be the same as connecting to a port with no service. **TCP** packets will return an **ACK/RST**, which is the same response that an open port with no service on it will respond with. When using **REJECT**, an ICMP packet returns destination-unreachable to the source host.

When you deal with Iptables, you need first to define the three policies for each chain; after that, you can add exceptions and specifications. Adding policies looks like this:

    sudo iptables -P INPUT     <ACCEPT/DROP/REJECT>
    sudo iptables -P OUTPUT   <ACCEPT/DROP/REJECT>
    sudo iptables -P FORWARD <ACCEPT/DROP/REJECT>

**Iptables permissive and restrictive policies:**

You can apply Iptables with a permissive policy by accepting all incoming connections except for these you specifically drop or reject. In this case, every connection is allowed unless you define a rule to refuse it specifically.

On the contrary, restrictive policies refuse all connections except for these you specifically accept. In this case, every connection is refused unless you define a rule to accept it.

**Applying a restrictive policy with Iptables:**

The following example shows how to apply a restrictive policy with Iptables by dropping all incoming traffic except for the allowed.

**Blocking incoming traffic.**

**IMPORTANT**: applying the following 3 rules may leave you without an internet connection. Using the rules mentioned in “[Iptables Appending rules and Iptables states](https://docs.google.com/document/d/1hvVg2PYZxPYNfvwL3ILDzJRXaW7mZk9fb8z0PgyisCLp8ZKzzU20LyrqvbjZfIJodgl7Xw/edit?_gfid=docs_editor_frame\&fileUrl=https://cloud-docs-gdd.dropbox.com/fsip/files/id\:L05vcXvkRoIAAAAAAADPsw\&isInboundFsipRequest=true\&jsh=m;/_/scs/apps-static/_/js/k%3Doz.gapi.en.vQiXRrxCe40.O/am%3DAQ/d%3D1/rs%3DAGLTcCMBxIGVyXSdvvcs43a64yHt_P7dfg/m%3D__features__\&organizationType=CONSUMER\&parent=https://gdd.dropbox.com\&pfname\&previewUrl=https://www.dropbox.com/scl/fi/zyb6sg69kmq0k7xbbps3w/Iptables-tutorial-IvanVanney-haniwriter.docx?cloud_editor%3Dpreview%26dl%3D0%26web_open_id%3Dweb_open_id-b1e2faa055cf5b79\&rpctoken=20221183\&tpat=cd.ACUtFvBe3lv9EuSGBWPHp4_XfyPro3IWkVgd4J8F0kU8XbrRXF4qVwmfIYq_Du_Da7jLzId3IdEjNaGyXF5QyopAJBRSUKHiZm0zHAXpkUObO3P9BGI86oq5AuYeJoYUSw-QBQgdOYLd4lhS-JDpaR5OeXVzElYa-XPvnTMNgjg7UM2LYpgdyzFZMpZri7BVtssRwg9zXdiyOj25v7XUyg0-Tg_HQ9lZfO6xmsN2HIHXm_cfdOZpYEpQ_gSnItevoHMb99gqDt8mYwPEhyJ_k6AMMnxMQHrW2s3E_uDozH9vy6p78_VjcSNZkuqu0Db7eGQdedFDJ6jvX0qELKF6ONOeFGZJAMUXTnjk_c689kAp5FBqEQTYsNnz0aIWsncQJVwRidz0s_AMMHxItU4kFKXYB-nsu6NzJ50a-54vaEJUbC-_n1CaX6sSNZ4OT6hN6aURRGZAqreEW2jIk05PwuW8tMZwR2qE1f67jNFSBcw9txZRdaqYWAHwVLzdWzp-S1k_gnVI9LISAJNa_S7kqC1fsTutscbFkmKnblsekz6IS8fIkoTLcwJRxPlai3ktxllSvoG_H7XIb5t4zVWy4J2I_v2OpP38xagrtUcxFxSBHnyqq47kgVHpXRYAC0UJJXYInTZSiYMfDPjBh59VMdVto--fl8DldKamMubZxkcw2U63tXRxkiaVrvrqX0q4d1EfaSTeiE7PUvqsOHJQ1OEo_05-TkneRgRohprTyHJOAfcqRyGW-vS4ShTpJdKNcSuhw_WlhBSQGXfyK8NQTUCXPPep7-JfPZs-D-mFWYnzfQ%3D%3D\&tpatExpirationTime=1623019860000\&usegapi=1#bookmark=id.gjdgxs),” you add the necessary exceptions to restore your access to the internet. You can consistently execute sudo iptables -F to flush rules.

You can block all incoming traffic, allowing only outgoing traffic to browse the web and for applications you need.

sudo iptables -P INPUT DROP

sudo iptables -P OUTPUT ACCEPT

sudo iptables -P FORWARD DROP

Where:

**-P = Policy**

**sudo iptables -P INPUT DROP**: instruct iptables to refuse all incoming traffic without replying to the source.

**sudo iptables -P OUTPUT ACCEPT**: defines an ACCEPT policy for outgoing traffic.

**sudo iptables -P FORWARD DROP**: instructs iptables not to execute routing tasks dropping all packets destined to a different host (trying to pass through the firewalled device) without reply.

The example above allows browsing the web and connections started by the local device (**-P OUTPUT ACCEPT**) but will prevent connections initiated by another host (**-P INPUT DROP**) like ssh attempts to access your device not return error messages.

When you enable Iptables with a restrictive policy like in the previous example, you need to append rules to adjust your configuration. For example, if you keep the configuration mentioned above without adding a reasonable exception for the loopback (lo) interface, some applications may not work correctly. You will also need to allow incoming traffic belonging to or related to a connection started by your device.

## Iptables Appending rules and Iptables states

It is essential to understand that Iptables applies rules by order. When you define a rule after a previous rule, the second rule will rewrite the last if a packet matches the same rule.

I like the previous example; you blocked all incoming traffic, you need to append exceptions for the loopback interface; this can be achieved by adding the -A (Append) parameter.

    sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
    
    sudo iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED -j ACCEPT

The module (-m) conntrack –ctstate **ESTABLISHED,RELATED** instructs Iptables to confirm if the connection state is **ESTABLISHED or RELATED** to an existing connection before applying the defined rule policy.

There are 4 possible states Iptables can check:

**Iptables state NEW**: The packet or traffic you allow or block tries to start a new connection.

**Iptables state ESTABLISHED**: The packet or traffic you allow or block is part of an established connection.

**Iptables state RELATED**: The packet or traffic starts a new connection but is related to an existing connection.

**Iptables state INVALID**: The packet or traffic is unknown without the state.

The first line of the example above instructs Iptables to accept incoming packets from the traffic coming from or related to connections started by your device. The second line instructs Iptables to accept only outgoing traffic from already established connections.

## Iptables Append to accept loopback traffic and defining interfaces:

The loopback interface is used by programs that need to interact with the localhost. If you don’t allow loopback traffic, some applications may not work.

The following command allows loopback connections:

sudo iptables -A INPUT -i lo -j ACCEPT

sudo iptables -A OUTPUT -o lo -j ACCEPT

Where -i and -o are used to specify the network device for incoming traffic (-i) and outgoing traffic (-o).

## Applying a permissive policy with Iptables:

You can also define a permissive policy allowing all traffic except for the specified dropped or rejected. You can enable everything except a specific IP or IP range, or you can refuse packets based on their headers, among more possibilities.

The following example shows how to apply a permissive policy allowing all traffic except for an IP range blocked for the ssh service.

    sudo iptables -P INPUT ACCEPT
    
    sudo iptables -P OUTPUT ACCEPT
    
    sudo iptables -P FORWARD DROP
    
    sudo iptables -A INPUT -p tcp --dport 22 -m iprange --src-range 192.168.1.100-192.168.1.110 -j REJECT

The example above applies a permissive policy but blocks ssh access to all IPs belonging to the range 192.168.1.100 and 192.168.1.110.

Where -p specifies the protocol, –dport (or –destination-port) the destination port (22,ssh), and the module iprange with the argument –src-range (source range) allows defining the IP range. The option -j (–jump) instructs iptables what to do with the packet; in this case, we represent REJECT.

## Blocking ports with Iptables

The following example shows how to block a specific port for all connections, the ssh port.

sudo iptables -A INPUT -p tcp --destination-port 22 -j DROP

![img](https://linuxhint.com/wp-content/uploads/2021/05/image6-37.png)

## Saving Iptables changes

Iptables rules are not persistent; after rebooting, the rules will not be restored. To make your rules persistent, run the following commands where the first line saves the rules in the file /etc/iptables.up.rules, and the second line is to create the file for iptables to start after reboot.

sudo iptables-save > /etc/iptables.up.rules

nano /etc/network/if-pre-up.d/iptables

Add the following to the file and close saving changes (CTRL+X).

![img](https://linuxhint.com/wp-content/uploads/2021/05/image9-31.png)

*#!/bin/sh*
/sbin/iptables-restore < /etc/iptables.up.rules

![img](https://linuxhint.com/wp-content/uploads/2021/05/image8-35.png)

Finally, give the file execution permissions by running:

chmod +x /etc/network/if-pre-up.d/iptables

![img](https://linuxhint.com/wp-content/uploads/2021/05/image12-19.png)

## Flushing or removing Iptables rules:

You can remove all your Iptables rules by running the following command:

sudo iptables -F

![img](https://linuxhint.com/wp-content/uploads/2021/05/image10-27.png)

To remove a specific chain like INPUT, you can run:

sudo iptables -F

## ![img](https://linuxhint.com/wp-content/uploads/2021/05/image11-24.png)

## Conclusion:

Iptables are among the most sophisticated and flexible firewalls in the market. Despite being replaced, it remains as one of the most spread defensive and routing software.

Its implementation can be quickly learned by new Linux users with basic knowledge of TCP/IP. Once users understand the syntax, defining rules becomes an easy task.

There are a lot more additional modules and options that weren’t covered by this introductive tutorial. You can see more iptables examples at [Iptables for beginners](https://linuxhint.com/iptables_for_beginners/).

I hope this Iptables tutorial was helpful. Keep following Linux Hint for more Linux tips and tutorials.

----
### View

    iptables -nvL –line-number

### Add/Insert

添加规则有两个参数：-A和-I。其中-A是添加到规则的末尾；-I可以插入到指定位置，没有指定位置的话默认插入到规则的首部。

    iptables -A INPUT -s 192.168.1.5 -j DROP
    //or
    iptables -I INPUT 3 -s 192.168.1.3 -j DROP

### Delete

    iptables -D INPUT -s 192.168.1.5 -j DROP
    //or with --line-number to delete
    iptables -D INPUT 2

### Modify

    iptables -R INPUT 3 -j ACCEPT

<div style="margin:150px;"></div>

### Iptables

<https://www.linode.com/docs/security/firewalls/control-network-traffic-with-iptables/>

#### General format

the following command adds a rule to the beginning of the chain that will drop all packets from the address `198.51.100.0`:

    iptables -I INPUT -s 198.51.100.0 -j DROP

The sample command above:

1.  Calls the iptables program
2.  Uses the -I option for insertion. Using a rule with the insertion option will add it to the beginning of a chain and will be applied first. To indicate a specific placement in the chain, you may also use a number with the -I option.
3.  The `-s` parameter, along with the IP address (198.51.100.0), indicates the source.
4.  Finally, the `-j` parameter stands for jump. It specifies the target of the rule and what action will be performed if the packet is a match.

| Parameter             | Description                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------- |
| `-p, --protocol`      | The protocol, such as TCP, UDP, etc.                                                        |
| `-s, --source`        | Can be an address, network name, hostname, etc.                                             |
| `-d, --destination`   | An address, hostname, network name, etc.                                                    |
| `-j, --jump`          | Specifies the target of the rule; i.e. what to do if the packet matches.                    |
| `-g, --goto chain`    | Specifies that the processing will continue in a user-specified chain.                      |
| `-i, --in-interface`  | Names the interface from where packets are received.                                        |
| `-o, --out-interface` | Name of the interface by which a packet is being sent.                                      |
| `-f, --fragment`      | The rule will only be applied to the second and subsequent fragments of fragmented packets. |
| `-c, --set-counters`  | Enables the admin to initialize the packet and byte counters of a rule.                     |

#### Default Tables

Tables are made up of built-in chains and may also contain user-defined chains. The built-in tables will depend on the kernel configuration and the installed modules.

The default tables are as follows:

**Filter** - This is the default table. Its built-in chains are:

*   Input: packets going to local sockets
*   Forward: packets routed through the server
*   Output: locally generated packets

**Nat** - When a packet creates a new connection, this table is used. Its built-in chains are:

*   Prerouting: designating packets when they come in
*   Output: locally generated packets before routing takes place
*   Postrouting: altering packets on the way out

**Mangle** - Used for special altering of packets. Its chains are:

*   Prerouting: incoming packets
*   Postrouting: outgoing packets
*   Output: locally generated packets that are being altered
*   Input: packets coming directly into the server
*   Forward: packets being routed through the server

**Raw** - Primarily used for configuring exemptions from connection tracking. The built-in chains are:

*   Prerouting: packets that arrive by the network interface
*   Output: processes that are locally generated

**Security** - Used for Mandatory Access Control (MAC) rules. After the filter table, the security table is accessed next. The built-in chains are:

*   Input: packets entering the server
*   Output: locally generated packets
*   Forward: packets passing through the server

#### Basic iptables Options

There are many options that may be used with the `iptables` command:

| Option                     | Description                                                                |
| -------------------------- | -------------------------------------------------------------------------- |
| `-A --append`              | Add one or more rules to the end of the selected chain.                    |
| `-C --check`               | Check for a rule matching the specifications in the selected chain.        |
| `-D --delete`              | Delete one or more rules from the selected chain.                          |
| `-F --flush`               | Delete all the rules one-by-one.                                           |
| `-I --insert`              | Insert one or more rules into the selected chain as the given rule number. |
| `-L --list`                | Display the rules in the selected chain.                                   |
| `-n --numeric`             | Display the IP address or hostname and post number in numeric format.      |
| `-N --new-chain <name>`    | Create a new user-defined chain.                                           |
| `-v --verbose`             | Provide more information when used with the list option.                   |
| `-X --delete-chain <name>` | Delete the user-defined chain.                                             |

#### Insert, Replace or Delete iptables Rules

**iptables** rules are enforced top down, so the first rule in the ruleset is applied to traffic in the chain, then the second, third and so on. This means that rules cannot necessarily be added to a ruleset with `iptables -A` or `ip6tables -A`. Instead, rules must be inserted with `iptables -I` or `ip6tables -I`.

#### Insert

Inserted rules need to be placed in the correct order with respect to other rules in the chain. To get a numerical list of your iptables rules:

    sudo iptables -L -nv --line-numbers

For example, let's say you want to insert a rule into the [basic ruleset](https://www.linode.com/docs/security/firewalls/control-network-traffic-with-iptables/#basic-iptables-rulesets-for-ipv4-and-ipv6) provided in this guide, that will accept incoming connections to port 8080 over the TCP protocol. We'll add it as rule 7 to the INPUT chain, following the web traffic rules:

    sudo iptables -I INPUT 7 -p tcp --dport 8080 -m state --state NEW -j ACCEPT

If you now run `sudo iptables -L -nv` again, you'll see the rule in the output.

#### Replace

Replacing a rule is similar to inserting, but instead uses `iptables -R`. For example, let's say you want to reduce the logging of denied entries to only 3 per minute, down from 5 in the original ruleset. The LOG rule is ninth in the INPUT chain:

    sudo iptables -R INPUT 9 -m limit --limit 3/min -j LOG --log-prefix "iptables_INPUT_denied:" --log-level 7

### Delete

Deleting a rule is also done using the rule number. For example, to delete the rule we just inserted for port 8080:

    sudo iptables -D INPUT 7

> **Caution:**
> Editing rules does not automatically save them. See our section on deploying rulesets for the specific instructions for your distribution.

#### View Your Current iptables Rules

    sudo iptables -L -nv
    
    sudo ip6tables -L nv

### Configure iptables

iptables can be configured and used in a variety of ways. The following sections will outline how to configure rules by port and IP, as well as how to ++blacklist++ (block) or ++whitelist++ (allow) addresses.

#### Block Traffic by Port

You may use a port to block all traffic coming in on a specific interface. For example:

    sudo iptables -A INPUT -j DROP -p tcp --destination-port 110 -i eth0

Let's examine what each part of this command does:

*   `-A` will add or append the rule to the end of the chain.
*   `INPUT` will add the rule to the table.
*   `DROP` means the packets are discarded.
*   `-p tcp` means the rule will only drop TCP packets.
*   `--destination-port 110` filters packets targeted to port 110.
*   `-i eth0` means this rule will impact only packets arriving on the eth0 interface.

It is important to understand that iptables do not recognize aliases on the network interface. Therefore, if you have several virtual IP interfaces, you will have to specify the destination address to filter the traffic. A sample command is provided below:

    sudo iptables -A INPUT -j DROP -p tcp --destination-port 110 -i eth0 -d 195.51.100.0

You may also use `-D` or `--delete` to remove rules. For example, these commands are equivalent:

    iptables --delete INPUT -j DROP -p tcp --destination-port 110 -i eth0 -d 198.51.100.0
    iptables -D INPUT -j DROP -p tcp --destination-port 110 -i eth0 -d 198.51.100.0

#### Drop Traffic from an IP

In order to drop all incoming traffic from a specific IP address, use the iptables command with the following options:

    iptables -I INPUT -s 198.51.100.0 -j DROP

To remove these rules, use the `--delete` or `-D` option:

    iptables -D INPUT -s 198.51.100.0 -j DROP

#### Block or Allow Traffic by Port Number to Create an iptables Firewall

One way to create a firewall is to block all traffic to the system and then allow traffic on certain ports. Below is a sample sequence of commands to illustrate the process:

    iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    iptables -A INPUT -i lo -m comment --comment "Allow loopback connections" -j ACCEPT
    iptables -A INPUT -p icmp -m comment --comment "Allow Ping to work as expected" -j ACCEPT
    iptables -A INPUT -p tcp -m multiport --destination-ports 22,25,53,80,443,465,5222,5269,5280,8999:9003 -j ACCEPT
    iptables -A INPUT -p udp -m multiport --destination-ports 53 -j ACCEPT
    iptables -P INPUT DROP
    iptables -P FORWARD DROP

Let’s break down the example above. The first two commands add or append rules to the `INPUT` chain in order to allow access on specific ports. The `-p tcp` and `-p udp` options specify either UDP or TCP packet types. The `-m multiport` function matches packets on the basis of their source or destination ports, and can accept the specification of up to 15 ports. Multiport also accepts ranges such as `8999:9003` which counts as 2 of the 15 possible ports, but matches ports `8999`, `9000`, `9001`, `9002`, and `9003`. The next command allows all incoming and outgoing packets that are associated with existing connections so that they will not be inadvertently blocked by the firewall. The final two commands use the -P option to describe the default policy for these chains. As a result, all packets processed by `INPUT` and `FORWARD` will be dropped by default.

Note that the rules described above only control incoming packets, and do not limit outgoing connections.

#### Whitelist/Blacklist Traffic by Address

You can use iptables to block all traffic and then only allow traffic from certain IP addresses. These firewall rules limit access to specific resources at the network layer. Below is an example sequence of commands:

```sh
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -i lo -m comment --comment "Allow loopback connections" -j ACCEPT
iptables -A INPUT -p icmp -m comment --comment "Allow Ping to work as expected" -j ACCEPT
iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT
iptables -A INPUT -s 198.51.100.0 -j ACCEPT
iptables -P INPUT DROP
iptables -P FORWARD DROP
```

<div style="margin:320px;" />
---

Each chain is a list of rules; each rule is a set of conditions and an action to execute when the conditions are met. When processing a packet, the firewall scans the appropriate chain, one rule after another; when the conditions for one rule are met, it “jumps” (hence the -j option in the commands) to the specified action to continue processing. The most common behaviors are standardized, and dedicated actions exist for them. Taking one of these standard actions interrupts the processing of the chain, since the packet's fate is already sealed (barring an exception mentioned below):

BACK TO BASICS ICMP

> ICMP (Internet Control Message Protocol) is the protocol used to transmit complementary information on communications. It allows testing network connectivity with the ping command (which sends an ICMP echo request message, which the recipient is meant to answer with an ICMP echo reply message). It signals a firewall rejecting a packet, indicates an overflow in a receive buffer, proposes a better route for the next packets in the connection, and so on. This protocol is defined by several RFC documents; the initial RFC777 and RFC792 were soon completed and extended.
> → <http://www.faqs.org/rfcs/rfc777.html>
> → <http://www.faqs.org/rfcs/rfc792.html>
> For reference, a receive buffer is a small memory zone storing data between the time it arrives from the network and the time the kernel handles it. If this zone is full, new data cannot be received, and ICMP signals the problem, so that the emitter can slow down its transfer rate (which should ideally reach an equilibrium after some time).
> Note that although an IPv4 network can work without ICMP, ICMPv6 is strictly required for an IPv6 network, since it combines several functions that were, in the IPv4 world, spread across ICMPv4, IGMP (Internet Group Membership Protocol) and ARP (Address Resolution Protocol). ICMPv6 is defined in RFC4443.
> → <http://www.faqs.org/rfcs/rfc4443.html>

**ACCEPT**: allow the packet to go on its way;

**REJECT**: reject the packet with an ICMP error packet (the --reject-with type option to iptables allows selecting the type of error);

**DROP**: delete (ignore) the packet;

**LOG**: log (via syslogd) a message with a description of the packet; note that this action does not interrupt processing, and the execution of the chain continues at the next rule, which is why logging refused packets requires both a LOG and a REJECT/DROP rule;

**ULOG**: log a message via ulogd, which can be better adapted and more efficient than syslogd for handling large numbers of messages; note that this action, like LOG, also returns processing to the next rule in the calling chain;
chain\_name: jump to the given chain and evaluate its rules;

**RETURN**: interrupt processing of the current chain, and return to the calling chain; in case the current chain is a standard one, there's no calling chain, so the default action (defined with the -P option to iptables) is executed instead;

**SNAT** (only in the nat table): apply Source NAT (extra options describe the exact changes to apply);

**DNAT** (only in the nat table): apply Destination NAT (extra options describe the exact changes to apply);

**MASQUERADE** (only in the nat table): apply masquerading (a special case of Source NAT);

**REDIRECT** (only in the nat table): redirect a packet to a given port of the firewall itself; this can be used to set up a transparent web proxy that works with no configuration on the client side, since the client thinks it connects to the recipient whereas the communications actually go through the proxy.
Other actions, particularly those concerning the mangle table, are outside the scope of this text. The iptables(8) and ip6tables(8) have a comprehensive list.
