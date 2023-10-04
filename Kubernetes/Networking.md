[DNS on kubernetes](https://github.com/kubernetes/dns/blob/master/docs/specification.md)

### Pod networking
Every pod should have its own ip address and should be able to reach another pods, on same and different nodes(without NAT).

Node are part of an external LAN.
- Bridge network for each node
- Each bridge on its own subnet
- Every time a container is create, creates a link to its bridge, attach and create ip addresess