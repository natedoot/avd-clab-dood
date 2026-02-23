## No Working

### Play Log

```
/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd main                                                                                                           19:15:33
❯ ansible-playbook playbooks/anta-validate.yml -i inventory.yml

PLAY [Build Switch configuration] ****************************************************************************************************************************************************

TASK [Preflight | Run a lightweight command via eAPI] ********************************************************************************************************************************
ok: [dc1-leaf1a]

TASK [Preflight | Show basic success details] ****************************************************************************************************************************************
ok: [dc1-leaf1a] => {
    "msg": {
        "command": "show version",
        "host": "dc1-leaf1a",
        "ok": true,
        "result_preview": "Arista cEOSLab\nHardware version: \nSerial number: \nHardware MAC address: 001c.7316.c879\nSystem MAC address: 001c.7316.c879\n\nSoftware image version: 4.33.1F-39308076.liffeyceoslab..."
    }
}

TASK [arista.avd.anta_runner : Verify Requirements] **********************************************************************************************************************************
AVD version 5.5.1
Use -v for details.
ok: [dc1-leaf1a -> localhost]

TASK [arista.avd.anta_runner : Create required output directories if not present] ****************************************************************************************************
ok: [dc1-leaf1a -> localhost] => (item=/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd/anta)
changed: [dc1-leaf1a -> localhost] => (item=/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd/anta/avd_catalogs)
ok: [dc1-leaf1a -> localhost] => (item=/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd/anta/user_catalogs)
ok: [dc1-leaf1a -> localhost] => (item=/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd/anta/reports)

TASK [arista.avd.anta_runner : Run ANTA] *********************************************************************************************************************************************
[WARNING]: [anta-workflow] No tests found in the user-defined ANTA catalogs, exiting
ok: [dc1-leaf1a -> localhost]

PLAY RECAP ***************************************************************************************************************************************************************************
dc1-leaf1a                 : ok=5    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```

### Tree

```
├── anta
│   ├── avd_catalogs
│   ├── reports
│   │   ├── anta_report.csv
│   │   ├── anta_report.json
│   │   └── anta_report.md
│   └── user_catalogs
│       └── interfaces
│           └── interfaces-test.yml
```

## Working

### Play log

```
/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd main                                                                                                           19:15:55
❯ ansible-playbook playbooks/anta-validate.yml -i inventory.yml

PLAY [Build Switch configuration] ****************************************************************************************************************************************************

TASK [Preflight | Run a lightweight command via eAPI] ********************************************************************************************************************************
ok: [dc1-leaf1a]

TASK [Preflight | Show basic success details] ****************************************************************************************************************************************
ok: [dc1-leaf1a] => {
    "msg": {
        "command": "show version",
        "host": "dc1-leaf1a",
        "ok": true,
        "result_preview": "Arista cEOSLab\nHardware version: \nSerial number: \nHardware MAC address: 001c.7316.c879\nSystem MAC address: 001c.7316.c879\n\nSoftware image version: 4.33.1F-39308076.liffeyceoslab..."
    }
}

TASK [arista.avd.anta_runner : Verify Requirements] **********************************************************************************************************************************
AVD version 5.5.1
Use -v for details.
ok: [dc1-leaf1a -> localhost]

TASK [arista.avd.anta_runner : Create required output directories if not present] ****************************************************************************************************
ok: [dc1-leaf1a -> localhost] => (item=/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd/anta)
ok: [dc1-leaf1a -> localhost] => (item=/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd/anta/avd_catalogs)
ok: [dc1-leaf1a -> localhost] => (item=/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd/anta/user_catalogs)
ok: [dc1-leaf1a -> localhost] => (item=/Users/nate.shaw/git_projects/avd-clab-dood/single-dc-lab/avd/anta/reports)

TASK [arista.avd.anta_runner : Run ANTA] *********************************************************************************************************************************************
fatal: [dc1-leaf1a -> localhost]: FAILED! => {"anta_tests_summary": {"devices_with_test_errors": [], "devices_with_test_failures": ["dc1-leaf1a"], "tests_error": 0, "tests_failed": 1, "tests_passed": 0, "tests_skipped": 0, "tests_unset": 0, "total_tests": 1}, "changed": false, "msg": "Task failed due to ANTA test failures/errors."}

PLAY RECAP ***************************************************************************************************************************************************************************
dc1-leaf1a                 : ok=4    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

### Tree

```
├── anta
│   ├── avd_catalogs
│   ├── reports
│   │   ├── anta_report.csv
│   │   ├── anta_report.json
│   │   └── anta_report.md
│   └── user_catalogs
│       └── interfaces-test.yml
```
