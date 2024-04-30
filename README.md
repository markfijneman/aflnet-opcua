# New AFLNet version

This repo is a fork of the stateful Fuzzer [AFLNet](https://github.com/aflnet/aflnet), developed to cope efficiently with stateful systems.

# Upgrades

## OPC UA support
Meaning of state numbers:
* 1: ACK (Acknowledge)
* 3: OPN (Open Secure Channel)
* \>3: Secure Channel NodeID (different message types that can be sent over secure channels, see https://github.com/wireshark/wireshark/blob/dff1a7996125edb3aa437a4eaab0f31c43d8161d/plugins/epan/opcua/opcua_serviceids.h for a list)

Negative numbers represent errors:
* -1 = unknown message type (FreeOpcUa seems to respond with garbage data after having received a close request; but this might also be a fault caused by the abstraction function)
* -2 = OPC UA Error (https://github.com/OPCFoundation/UA-Nodeset/blob/UA-1.05.03-2023-12-15/Schema/StatusCode.csv)

## Outputs a complete state model

This version of AFLNet allows the user to export the state model of the system by integrating the labels in the edges.

**The old graph:**

![image info](./images/old.png)

**The new graph:**

![image info](./images/new.png)

# Allows testing the fuzzer on custom code

AFLNet only supports known protocols. This version implements a **TEST** version that allows processing the *requests* and *responses* of custom software.

For example: 

    ./afl-fuzz -d -i ./input -o output -N tcp://127.0.0.1/port -P TEST -D 10000 -q 3 -s 3 -E -R ./my_program

allows to read from the input folder the messages to send to *my_program* and interprets the messages received from the program as *response codes*

# Important Notes

The extension saves the labels into the "key" field. If you want to show the label, you need to change "key" to "label". Also, .dot files don't like spaces (" ") at the end of a label, so try to get rid of them.

# Licences

AFLNet is licensed under [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0).

AFLNet is an extension of [American Fuzzy Lop](http://lcamtuf.coredump.cx/afl/) written and maintained by Michał Zalewski <<lcamtuf@google.com>>. For details on American Fuzzy Lop, we refer to [README-AFL.md](README-AFL.md).

* **AFL**: [Copyright](https://github.com/aflsmart/aflsmart/blob/master/docs/README) 2013, 2014, 2015, 2016 Google Inc. All rights reserved. Released under terms and conditions of [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0).
