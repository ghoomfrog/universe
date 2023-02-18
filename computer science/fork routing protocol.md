# Fork Routing Protocol

![Fork-Routing](https://user-images.githubusercontent.com/35694451/208344240-cae75190-51f8-4d8c-88c7-c317a69d08fa.png)

The Fork Routing Protocol (FRP) is a routing protocol where each router is configured with one input line, two routes, some output ports and a hotspot.

This configuration allows routers to be accessed using relative addresses where each bit corresponds to a route. The destination router sends the payload through all its output ports and its hotspot.

Absolute addressing is enabled as follows:
1. A global router is chosen as the router where addresses relative to it are considered absolute
2. Addresses have an <ins>origin</ins> bit that specifies whether the address is absolute (0) or relative (1)

Absolute addressing requires fork routers to convert the absolute address to a relative one which requires them to have their own absolute address stored for calculation. To make sure all fork routers can access each other, their second route, their *reline*, points to the router that points to the router that points to them: forming a circular reference.

### Header

Field       |Size      |Description
------------|----------|-----------
Address Size|64b       |This is the size of the address in bits. 0 means that the destination is this router
Address     |*variable*|If the address size is not 0, this field is a 7-bit-fragment [chain](https://github.com/ghoomfrog/universe/blob/main/computer%20science/chain.md) where the first bit of its content represents the origin (0 refers to the global router, and 1 refers to this router) and the remaining bits represent the routes, relative to the origin

Each router decrements the address size (if not 0) and arithmetically left-shifts the address (if not empty) before forwarding.
