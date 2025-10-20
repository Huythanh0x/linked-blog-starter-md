Okay, here's the reformatted content of [[Security - Block out going connection (To domain, IP)]] in proper Markdown:

# Block Outgoing Connections to Certain IPs on macOS Using pf

## 1. Find IP addresses of the domain

- In Terminal run:

```bash
dig {{example.com}} A
```

- Note the IP addresses in the ANSWER SECTION.

## 2. Determine your active network interface

- Run:

```bash
scutil --nwi
```

- Note active interface name (e.g., `en0`).

## 3. Edit pf configuration file

- Open pf config:

```bash
sudo nano /etc/pf.conf
```

## 4. Add a table and blocking rule

- Add at the end (replace IPs and interface):

```text
table <blocked_ips> persist { 104.26.6.58, 104.26.7.58, 172.67.69.91 }
block drop out quick on en0 to <blocked_ips>
```

## 5. Test pf config syntax (no output means OK)

```bash
sudo pfctl -nf /etc/pf.conf
```

## 6. Load and enable pf firewall with the rules

```bash
sudo pfctl -f /etc/pf.conf
sudo pfctl -e
```

## 7. Verify rules are loaded

```bash
sudo pfctl -sr           # List rules
sudo pfctl -t blocked_ips -T show  # Show blocked IPs
```

---

# Temporarily Disable Your pf Block

- Disable pf firewall to suspend all pf rules:

```bash
sudo pfctl -d
```

- Confirm pf is disabled:

```bash
sudo pfctl -s info
```

- Look for `Status: Disabled`

- Reactivate pf and reload rules when ready:

```bash
sudo pfctl -f /etc/pf.conf
sudo pfctl -e
```

---

# Completely Remove the Block

1. Open pf config:

```bash
sudo nano /etc/pf.conf
```

2. Remove or comment out the block table and rule:

```text
#table <blocked_ips> persist { ... }
#block drop out quick on en0 to <blocked_ips>
```

3. Save and quit.

4. Reload pf config without block rules:

```bash
sudo pfctl -f /etc/pf.conf
```

5. Optionally disable pf completely:

```bash
sudo pfctl -d
```

---

## Notes:

- Replace `en0` with your actual active interface.
- IP addresses must be updated if domain IPs change.
- pf blocks system-wide, affects all apps.
- Commands require admin privileges (sudo).
- This process uses native macOS command-line tools and pf firewall.

This fully addresses blocking, disabling, and removal of outgoing IP blocks on macOS natively.

If customization or help on interface detection is needed, feel free to ask!

1. [https://www.digitalocean.com/community/tutorials/how-to-configure-packet-filter-pf-on-freebsd-12-1](https://www.digitalocean.com/community/tutorials/how-to-configure-packet-filter-pf-on-freebsd-12-1)
2. [https://www.siberoloji.com/how-to-configure-the-pf-firewall-basic-rules-on-freebsd-operating-system/](https://www.siberoloji.com/how-to-configure-the-pf-firewall-basic-rules-on-freebsd-operating-system/)
3. [https://srobb.net/pf.html](https://srobb.net/pf.html)
4. [https://stackoverflow.com/questions/23715269/how-can-i-programmatically-manage-pf-rules-on-the-fly](https://stackoverflow.com/questions/23715269/how-can-i-programmatically-manage-pf-rules-on-the-fly)
5. [https://docs.netgate.com/pfsense/en/latest/firewall/pf-ruleset.html](https://docs.netgate.com/pfsense/en/latest/firewall/pf-ruleset.html)
6. [https://baltig.infn.it/ocp-tools/os-controller-ha/-/blob/e9c5a806ded0f0f83be1f2b2641515ee75c6310c/modules/firewall/README.markdown](https://baltig.infn.it/ocp-tools/os-controller-ha/-/blob/e9c5a806ded0f0f83be1f2b2641515ee75c6310c/modules/firewall/README.markdown)
7. [https://forums.lawrencesystems.com/t/pffocus-tutorial-howto-want-to-try-opnsense/10931](https://forums.lawrencesystems.com/t/pffocus-tutorial-howto-want-to-try-opnsense/10931)
8. [https://forums.freebsd.org/threads/firewall-sample-rules.51526/](https://forums.freebsd.org/threads/firewall-sample-rules.51526/)