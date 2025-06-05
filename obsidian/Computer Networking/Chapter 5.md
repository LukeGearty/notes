# Introduction
Forwarding Table/Flow Table links the data and control plane. They specify the local router actions taken.
Per Router Control: The routing algorithm runs in each and every router
Logically Centralized Control: A centralized controller computes and distributes the forwarding tables, and also perform other functions like load sharing, firewalling, NAT. A control agent in each router interacts with the centralized controller.

# Routing Algorithms
Routing algorithms determine good paths from senders to receivers.


Centralized Routing Algorithms compute the path from source to destination using complete knowledge of the network. Also known as link state
Decentralized Routing Algorithms compute the least-cost path in an iterative, distributed manner. Distance Vector algorithm.
Static: routes change slowly
Dynamic: Routes change as network traffic loads or topology changes.

## Link State Routing Algorithm
Dijkstra's Algorithm
D(v) is the cost of the least-cost path from source node to destination v as of the iteraiton of the algorithm
p(v) is the previous node (neighbor of v) along the current least cost path from source to v
N' is the subset of nodes whose least-cost path from source to v is definitively known

Step 0:
initialize N' to only include source node
for all the nodes, if node is a neighbor of the source node, D(v) = C(source, neighbor). Otherwise D(v) = ∞.

Loop
Add a node w to N' such that D(w) is a minimum
update D(v) for every neighbor of w that is not in N':
	D(v) = min(old cost of D(v), D(w) + c(w, v))
Loop through nodes until N' has all nodes

Computational complexity: O(n^2), can be (O n log n)

Pathology: When link costs are not symmetric, that is C(w,v) != C(v, w) can lead to different routes/oscillations being taken. A way to avoid this pathology is to have the Link state algorithm execute in different routers at different times.

## Distance Vector
Iterative, asynchronous, and distributed. Each ode receives infromation from one or more of its directly attached neighbors.
Iterative means the processs continues until no more information is exchanged between neighbors.
Asynchronous in that it does not require all of the nodes to operate simultaneously.

d_x(y) is the cost of the least cost path from node x to node y.
d_x(y) = min_v{ c(x, v) + d_v(y) }
The cost of x to y is the minimum value of the cost from x to v + the cost of v to y.
This is called the Bellman-Ford Equation 

Step 0:

for all destinations y in N:
	D_x(y) = c(x, y) if y is not a neighbor then c(x, y) = ∞
for each neighbor w
	D_w(y) = ? for all destinations y in N
for neighbor w
	send distance v D_x = [D_x(y) : y in N } to w] // send the distance vector to neighbors

loop
	wait until a link cost change to some neighbor w or until receive a distance ector

	for each y in N:
		D_x(y) = min_v{ c(x, v) + D_v(y)})
if D_x(y) changed for any destination y
	send distance vector D_x = [D_x(y) : y in N] to all neighbors

Count-to-infinity Problem:
In a network, good news travels fast. So a link cost going down will propagate quickly.
Bad news however, it will take a while for the actual news to propagate through the entire network. 
### A comparison between LS and DV

|                      | LS                                                                                                                                                                                   | DV                                                                                                                                                       |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Message Complexity   | LS requires each node to know the cost of each link in the network. O(\|N\| \|E\|) messages must be sent. Whenever a link cost changes, the new link cost must be sent to all nodes. | When link costs change, DV algorithm will propagate the results of the changed link cost only if the new link cost results in a changed least cost-path. |
| Speed of Convergence | LS is an O(\|N\|^2) algorithm requiring O(\|N\| \|E\|) messages.                                                                                                                     | Can converge slowly, routing loops while the algorithm is converging. Count to infinity problem.                                                         |
| Robustness           | A router can broadcast an incorrect cost. A node can also corrupt or drop any packets it receives as part of an LS broadcast.                                                        | Routes can advertise incorrect least-cost paths to any or all destinations.                                                                              |

# Intra-AS Routing in the Internet: OSPF
The view of the internet as a homogenous set of routers all executing the same routing algorithm is incorrect for these reasons.
- Scale: The internet is really big, and storing routing info for every single possible destination is really not possible
- Administrative Autonomy: ISPs have networks of routers and each ISP operates independently of each other.
Routers are organized in to Autonomous Systems (ASs). ISPs and their routers can constitute a single AS. It is identified by an Autonomous System number (ASN) and is assigned by ICANN, like an IP address.
Routing within an autonomous system is called an Intra-autonomous system routing protocol.
### Open Shortest Path First
An open source intra-AS routing protocol. A link-state protocol that uses link-state flooding of information and a Dijkstra's least cost path algorithm. Each router runs the algorithm and determines the shortest-path tree to all subnets with itself as the root node.
A router broadcasts information all other routers in the AS. It broadcasts link-state infromation whenever there is a change in a link's state, and broadcasts a link's state periodically (like say 30 minutes), even if nothing changed. 
It is carried directly by IP, with an upper-layer protocol of 89 for OSPF. It implements its own reliable message transfer and link-state broadcast. 

- Security: OSPF messages can be authenticated 
- Multiple same-cost paths: when the cost a path has the same cost as another path, there can be multiple paths chosen
- Integrated support for unicast and multicast
- Hierarchy:
	- An OSPF AS can be configured to run hierarchically. Each run ins its own OSPF link-state routing algorithm. One or more area border routers are responsible for routing pacekts outside the area. One OSPF area in the AS is configured to be the backbone where traffic is routed to other areas in the AS.

# Routing Among the ISPs: BGP
BGP is an inter-autonomous system routing protocol - between different ASs.
## The Role of BGP
When a router gets a packet destined within the same AS, the forwarding table is determined by the AS's intra-AS routing protocol. If it is destined outside the AS, BGP determines the forwarding behavior.
Packets are specified for CIDRized prefixes. The entries in the forwarding table can take the form of (131.213.34/20, I), where I = the router interface to be forwarded.
BGP provides each router a means to:
1. Obtain prefix reachability info from neighboring ASs. BGP allows each subnet to advertise "I exist and am here" to the rest of the Internet. 
2. Determine the "best routes" to the prefixes: BGP route selection procedures are run to determine the best route to the prefix.

## Advertising BGP Route Information
For each AS, each router is either a gateway router (connected to another router in another AS) or an internal router (connected to hosts within the same AS). Every router exchanges information over semi-permanent TCP connections using port 179. If it spans two ASs, it is called an external BGP (between two gateway routers). If it is in the same AS, it is called an internal BGP connection.

## Determining Best Routes
When routers advertise prefixes across the BGP connection, it includes with the prefix several attributes.
AS-PATH: contains the list of ASs that the advertisement passed through.
NEXT-HOP: The IP address of the router interface that begins the AS-PATH.

### Hot Potato Routing
Hot Potato Routing is when the route chosen from all possible routes is the route with the least cost path to the NEXT-HOP router beginning that route.

### Route-Selection Algorithm
Route selection in BGP, if there is more than one route to the same prefix, invokes these rules to find the best possible route:

1. First, a route is assigned a local preference that can be set by the router or learned from another router. The value is set by the network administrator.
2. The route with the shortest AS-PATH is selected. If this was the only rule, BGP would use a DV algorithm to determine the path, where the distance metric is number of AS hops rather than number of router hops.
3. Hot potato routing.
4. BGP identifiers are used.

## IP-Anycast
BGP can implement IP-Anycast, where multiple servers share the same IP addresses and incoming requests are routeed to the nearest server. CDNs don't really use IP-Anycast, but it is used by DNS. 

## Routing Policy
AS Routing policy can trump all considerations. Routes are first selected according to local-preference.

# The SDN Control Plane
Four key characteristics of an SDN architecture:
1. Flow Based forwarding
2. Separation of data plane and control plane: 
3. Network control functions are external to the data plane switches
4. Programmable Network

## The SDN Control Plane: SDN Controller and SDN Network-control applications
From a bottom up perspective:
There is a communication layer between the SDN controller and network devices
There is a network-wide state-management layer, that provides information about the state of how the network is going.
The interface to the network-control application layer


# Questions
## Section 5.1
**R1.  What is meant by a control plane that is based on per-router control? In such cases, when we say the network control and data planes are implemented “monolithically,” what do we mean?**
Per-router control: A routing algorithm runs in each and every router. Both forwarding and routing are controlled in the router. 

**R2.  What is meant by a control plane that is based on logically centralized control? In such cases, are the data plane and the control plane implemented within the same device or in separate devices? Explain.**
A logically centralized controller implements the control plane - computes routing tables and communicates with the routers, while the routers themselves just implement the data plane functions.

## Section 5.2
**R3.  Compare and contrast the properties of a centralized and a distributed routing algorithm. Give an example of a routing protocol that takes a centralized and a decentralized approach.**
Centralized: Has global knowledge: Dijkstra's Link State, OSPF
Decentralized: done iteratively.  Distance Vector, BGP

**R4.  Compare and contrast link-state and distance-vector routing algorithms.**
Link-State: centralized, nodes use global knowledge of network to compute the least-cost path. 
Distance-Vector: decentralized, nodes send DVs to neighbors to compute least-cost path.

**R5.  What is the “count to infinity” problem in distance vector routing?**
When a links cost goes up, the news about the links can propagate slowly because the nodes don't have complete knowledge of the network. News can go back and forth between nodes before the correct least-cost path of a link is calculated.