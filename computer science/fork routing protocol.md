# Fork Routing Protocol

![Fork-Routing](https://user-images.githubusercontent.com/35694451/208344240-cae75190-51f8-4d8c-88c7-c317a69d08fa.png)

> **Note** All [chains](https://github.com/ghoomfrog/universe/blob/main/computer%20science/chain.md) described in this document have 8-bit chainlets.

The Fork Routing Protocol (FRP) is a routing protocol where each router is configured with one input line, two routes, and an output port.

This configuration allows routers to be accessed using relative addresses where each bit corresponds to a route. The destination router sends the payload through its output port.

Absolute addressing is enabled as follows:
1. A global router is chosen as the router where addresses relative to it are considered absolute.
2. Addresses have an *origin* bit that specifies whether the address is absolute (0) or relative (1).

Absolute addressing requires fork routers to convert the absolute address to a relative one which requires them to have their own absolute address stored for calculation. To make sure all fork routers can access each other, their second route—their reline—points to the router that points to the router that points to them: forming a circular reference.

### Header

Field       |Size            |Description
------------|----------------|-----------
Route count |8b min.         |This is the number of routes in the address, as a chain. 0 means that the destination is this router.
Address     |optional 8b min.|If the address size is not 0, this field is a chain where the first bit of its content represents the origin (0 refers to the global router, and 1 refers to this router) and the remaining bits represent the routes, relative to the origin.

Each router decrements the route count if not 0, and discards the first route bit before forwarding.
