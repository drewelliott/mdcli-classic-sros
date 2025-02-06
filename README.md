
# /admin display-config | match "interface \"classic-to-md\"" context-all

## classic
```
*A:classic# /admin display-config | match "interface \"classic-to-md\"" context all
#--------------------------------------------------
    router Base
        interface "classic-to-md"
            address 10.20.1.2/24
            port 1/1/c1/1
            no shutdown
*A:classic#
```

## md-cli 

### pySROS (NETCONF)

```python
>>> from helper import get_connection as gc
>>>
>>> c = gc("clab-md-mdcli", "admin", "admin", 830)
>>>
>>> path = "/configure/router[router-name=Base]/interface[interface-name=md-to-classic]"
>>>
>>> c.running.get(path)
Container({'interface-name': Leaf('md-to-classic'), 'admin-state': Leaf('enable'), 'port': Leaf('1/1/c1/1'), 'ipv4': Container({'primary': Container({'address': Leaf('10.20.1.1'), 'prefix-length': Leaf(24)})})})
>>>
>>>
>>> c.running.get(path, defaults=True)
Container({'interface-name': Leaf('md-to-classic'), 'admin-state': Leaf('enable'), 'flavor': Leaf('regular'), 'port': Leaf('1/1/c1/1'), 'strip-label': Leaf(False), 'tos-marking-state': Leaf('trusted'), 'ingress-stats': Leaf(False), 'mac-accounting': Leaf(False), 'gre-termination': Leaf(False), 'urpf-selected-vprns': Leaf(False), 'ingress': Container({'destination-class-lookup': Leaf(False)}), 'ldp-sync-timer': Container({'end-of-lib': Leaf(False)}), 'load-balancing': Container({'ip-load-balancing': Leaf('both'), 'spi-load-balancing': Leaf(False), 'teid-load-balancing': Leaf(False), 'flow-label-load-balancing': Leaf(False)}), 'lag': Container({'per-link-hash': Container({'class': Leaf(1), 'weight': Leaf(1)})}), 'ptp-hw-assist': Container({'admin-state': Leaf('disable')}), 'hold-time': Container({'ipv4': Container({'down': Container({'init-only': Leaf(False)})}), 'ipv6': Container({'down': Container({'init-only': Leaf(False)})})}), 'ipv4': Container({'allow-directed-broadcasts': Leaf(False), 'icmp': Container({'mask-reply': Leaf(True), 'redirects': Container({'admin-state': Leaf('enable'), 'number': Leaf(100), 'seconds': Leaf(10)}), 'ttl-expired': Container({'admin-state': Leaf('enable'), 'number': Leaf(100), 'seconds': Leaf(10)}), 'unreachables': Container({'admin-state': Leaf('enable'), 'number': Leaf(100), 'seconds': Leaf(10)}), 'param-problem': Container({'admin-state': Leaf('enable'), 'number': Leaf(100), 'seconds': Leaf(10)})}), 'dhcp': Container({'admin-state': Leaf('disable'), 'trusted': Leaf(False), 'release-include-gi-address': Leaf(False), 'src-ip-addr': Leaf('auto'), 'relay-plain-bootp': Leaf(False), 'option-82': Container({'action': Leaf('keep'), 'vendor-specific-option': Container({'system-id': Leaf(False), 'client-mac-address': Leaf(False), 'pool-name': Leaf(False), 'port-id': Leaf(False), 'service-id': Leaf(False)})})}), 'bfd': Container({'admin-state': Leaf('disable'), 'transmit-interval': Leaf(100), 'receive': Leaf(100), 'multiplier': Leaf(3), 'type': Leaf('auto')}), 'primary': Container({'address': Leaf('10.20.1.1'), 'prefix-length': Leaf(24), 'broadcast': Leaf('host-ones')}), 'neighbor-discovery': Container({'timeout': Leaf(14400), 'retry-timer': Leaf(50), 'learn-unsolicited': Leaf(False), 'proactive-refresh': Leaf(False), 'local-proxy-arp': Leaf(False), 'remote-proxy-arp': Leaf(False), 'limit': Container({'log-only': Leaf(False), 'threshold': Leaf(90)})})}), 'qos': Container({'network-policy': Leaf('default')}), 'if-attribute': Container({'delay': Container({'delay-selection': Leaf('static-preferred'), 'dynamic': Container({'twamp-light': Container({'ipv4': Container({'admin-state': Leaf('disable')}), 'ipv6': Container({'admin-state': Leaf('disable')})})})})}), 'network-domains': Container({'network-domain': {'default': Container({'domain-name': Leaf('default')})}})})
>>>

### gNMIc (gNMI)
>>>
╭─drew at snoopy in ~/git/mdcli-classic-sros/scripts
╰─○ gnmic -a clab-md-mdcli:57400  --username admin --password admin get --path "/configure/router[router-name=Base]/interface[interface-name=md-to-classic]" --insecure

[
  {
    "source": "clab-md-mdcli:57400",
    "timestamp": 1738873523268771216,
    "time": "2025-02-06T20:25:23.268771216Z",
    "updates": [
      {
        "Path": "configure/router[router-name=Base]/interface[interface-name=md-to-classic]",
        "values": {
          "configure/router/interface": {
            "admin-state": "enable",
            "interface-name": "md-to-classic",
            "ipv4": {
              "primary": {
                "address": "10.20.1.1",
                "prefix-length": 24
              }
            },
            "port": "1/1/c1/1"
          }
        }
      }
    ]
  }
]
╭─drew at snoopy in ~/git/mdcli-classic-sros/scripts
╰─○
```

