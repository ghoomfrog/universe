# Fork Routing Protocol

![Fork-Routing](https://user-images.githubusercontent.com/35694451/208344240-cae75190-51f8-4d8c-88c7-c317a69d08fa.png)

The Fork Routing Protocol (FRP) is a routing protocol where each router, *fork router*, is essentially configured with one input line and two routes.

This configuration deprecates routing tables and allows routers to be accessed using relative addresses where each bit corresponds to a route.

To enable absolute addressing:
1. a global router is chosen as the router where addresses relative to it are considered absolute
2. the header has an *origin* bit that specifies whether the address is absolute (0) or relative (1)

Absolute addressing requires fork routers to convert the absolute address to a relative one which requires them to store their own absolute address for calculation. To make sure all fork routers can access each other, their second route, *their reline*, points to the router that points to their input router: forming a circular reference.

### Header

Field       |Size      |Description
------------|----------|-----------
Address Size|64b       |address size in bits
Origin      |1b        |*0* = global router; *1* = this router
Address     |*variable*|address relative to origin
