# Using Custom Templates

## Create templates/ dir under playbooks

```
├── playbooks
│   ├── templates
```

## Create j2 custom template under templates dir

```
├── playbooks
│   ├── templates
│   │   └── anycast-rp.j2
```

```jinja
{% if hostname == "dc1-leaf1a" or hostname == "dc1-leaf1b" %}
router pim sparse-mode
   vrf Prod
      ipv4
         anycast-rp 10.255.245.10 1.1.1.1
         anycast-rp 10.255.245.10 1.1.1.2
{% endif %}
```
## Enable custom template in group_vars

We can use logic in the j2 template to limit which switches get the additional config. So you can put this at the highest level of your fabric variables

```yaml
custom_templates:
  - anycast-rp.j2
```

## Confirm config generated on the correct devices

All template generated config will be appeneded to the bottom of the config file

```
# sample config from dc1-leaf1a
!
router pim sparse-mode
   vrf Prod
      ipv4
         anycast-rp 10.255.245.10 1.1.1.1
         anycast-rp 10.255.245.10 1.1.1.2
!
```
