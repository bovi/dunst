# Dunst

Simple cloud management tool using SSH and VMWare Workstation. Supporting clone, start and stop of VMs.

## Usage

Call the ```dunst``` command for one server like this:

```
dunst 192.168.0.1 user password cmd
```

### List available machines

To get a list of all available machines:

```
dunst 192.168.0.1 user password list
```

Returns:

```
Count	3
ID                      VMX                                                                     STATE   MAC                     IP
35986813758351367565    /home/dabo/vms/vmx/35986813758351367565/35986813758351367565.vmx        halt    00:50:56:2e:c8:ad
46478391399632621556    /home/dabo/vms/vmx/46478391399632621556/46478391399632621556.vmx        run     00:50:50:2e:c8:ac       172.23.0.170
94165679480162074298    /home/dabo/vms/vmx/94165679480162074298/94165679480162074298.vmx        halt    00:50:51:2e:c8:ad
```

### Create new machine

To create a new machine:

```
dunst 192.168.0.1 user password new
```

Returns:

```
OK	98680029932821660268
```

### Boot a machine

To start a machine:

```
dunst 192.168.0.1 user password start 98680029932821660268
```

Returns:

```
OK      98680029932821660268
```

### Shutdown

To shutdown a machine (soft shutdown):

```
dunst 192.168.0.1 user password halt 98680029932821660268
```

Returns:

```
OK      98680029932821660268
```

### Power Off

To power off a machine (hard shutdown):

```
dunst 192.168.0.1 user password stop 98680029932821660268
```

Returns:

```
OK      98680029932821660268
```

### Reboot

To reboot a machine (soft reboot):

```
dunst 192.168.0.1 user password reboot 98680029932821660268
```

Returns:

```
OK      98680029932821660268
```

### Reset

To reset a machine (hard reboot):

```
dunst 192.168.0.1 user password reset 98680029932821660268
```

Returns:

```
OK      98680029932821660268
```
