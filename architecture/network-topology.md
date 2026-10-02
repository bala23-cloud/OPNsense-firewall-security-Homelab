
## `network-topology`

```text
                    OPNsense Network Security Lab
                    =============================

                              INTERNET
                                  |
                                  |
                           VirtualBox NAT
                                  |
                                  |
                                  v
                     +----------------------+
                     |       OPNsense       |
                     |       FIREWALL       |
                     |                      |
                     | WAN: 10.0.x.x        |
                     | LAN: 192.168.x.x     |
                     +----------+-----------+
                                |
                         OPNSENSE-LAN
                                |
                                |
                    +-----------+-----------+
                    |                       |
                    |                       |
                    v                       v
             +-------------+         +-------------+
             |   Ubuntu    |         |    Kali     |
             |             |         |             |
             | 192.168.x.x |         |     DMZ     |
             |             |         |  Security   |
             | Management  |         |   Testing   |
             |     /SOC    |         |             |
             +-------------+         +-------------+

                    CURRENT              PLANNED
                    =======              =======
                    LAN                  DMZ
                    Ubuntu               Kali
