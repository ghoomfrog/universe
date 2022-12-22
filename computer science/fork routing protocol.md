# Fork Routing Protocol

![Fork-Routing](https://user-images.githubusercontent.com/35694451/208344240-cae75190-51f8-4d8c-88c7-c317a69d08fa.png)

The Fork Routing Protocol (FRP) is a routing protocol where each router, *fork router*, is configured with one input line and two routes.

This configuration allows routers to be accessed using relative addresses where each bit corresponds to a route.

To enable absolute addressing:
1. a global router is chosen as the router where addresses relative to it are considered absolute
2. addresses have an *origin* bit that specifies whether the address is absolute (0) or relative (1)

Absolute addressing requires fork routers to convert the absolute address to a relative one which requires them to have their own absolute address stored for calculation. To make sure all fork routers can access each other, their second route, their *reline*, points to the router that points to the router that points to them: forming a circular reference.

### Header

Field      |Size      |Description
-----------|----------|-----------
Address    |*variable*|an [intlet-based arbitrary-precision integer](https://github.com/ghoomfrog/universe/blob/main/computer%20science/intlet-based%20arbitrary-precision%20integer.md) where the first bit of the actual integer represents the origin (*0* = the global router; *1* = this router) and the remaining bits represent the routes, relative to the origin
