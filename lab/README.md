## SuperMicro M11SDV-8CT-LN4F Mini-ITX
- Ubuntu with kvm-libvirt
  - truenas scale (controls sata controll via vfio)
    - dc1 (samba active directory domain controller)
    - ipa (freeipa server)
    - windows 10 client (for testing ad and samba, and rsat)


### Notes

- Running an externally accesible pfsense network in nested virtualization on truenas scale requires bridge at both levels
   - create a bridge on ubuntu host and attach a physical port (eg br1)
   - create libvirt network using the bridge
   ```xml
   <network>
    <name>br1</name>
    <forward mode="bridge"/>
    <bridge name="br1"/>
  </network>
  ```
  - add the network to scale vm
  ```xml
  <interface type="network">
      <mac address="00:00:00:00:00:00"/>
      <source network="br1"/>
      <model type="virtio"/>
      <address type="pci" domain="0x0000" bus="0x0a" slot="0x00" function="0x0"/>
   </interface>
   ```
   - create bridge in truenas scale (requires console access!*)
   ```bash
   cli -c "network interface create name=br1 type=BRIDGE bridge_members=enp2s0"
   ```
   - commit changes
  ```bash
  cli -c "network interface commit"
  ```
  - persist changes
  ```bash
  cli -c "network interface checkin"
  ```
  - reboot system!
  
 
  
  ****due to the horrendous state of netwrking in scale, these changes must be done over direct console or ipmi access and all network connectivity will be lost until reboot!***
 
