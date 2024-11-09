# Simulation of Packet Transmission Between Routers in a Network Using Dijkstra's Algorithm to Find the Shortest Path

This project simulates packet transmission between routers in a network. Dijkstra's algorithm is used to find the shortest path between a source router and a destination router, considering the bandwidth of the connections.

The project was implemented in Kotlin, applying the object-oriented programming paradigm.

### Authors:
- **Bottini, Franco Nicolas**
- **Robledo, Valentin**

## How to Clone This Repository

```console
git clone https://github.com/francobottini99/PR-KOTLIN-2023.git
```

> [!NOTE]
> To run the program, you need jdk 20.

## How to Use

Navigate to the main directory of the project and use the following command:

```console
./gradlew run
```

The program will prompt you to enter some parameters for the simulation:

```console
Number of routers and pages
```

```console
Page size
```

```console
Bandwidth
```

```console
Number of cycles
```

Once the parameters are entered, the simulation will begin.

### Example

Here's an example with 3 routers, 3 initial pages of size 512, connection bandwidth of 256, and 12 simulation cycles.

Generated connections:

```console
Router [1] Connections: [1 -> 2, 3 -> 1, 1 -> 3, 2 -> 1]
Router [2] Connections: [1 -> 2, 2 -> 3, 2 -> 1, 3 -> 2]
Router [3] Connections: [2 -> 3, 3 -> 1, 1 -> 3, 3 -> 2]
```

Cycle 1, packet transmission:

```console
--- Cycle 1 - Task: SEND_STORE ---
Router [1] - Packet sent ID[1] from router [1] to router [3] | with destiny router [3]
Router [2] - Packet sent ID[1] from router [2] to router [3] | with destiny router [3]
Router [3] - Packet sent ID[1] from router [3] to router [2] | with destiny router [2]
```

Cycle 2, packet reception:

```console
--- Cycle 2 - Task: RECEPTION ---
Router [1] - Empty buffer.
Router [1] - Empty buffer.
Router [2] - Empty buffer.
Router [2] - Packet stored ID[1] from router [3] with destiny router [2]
Router [2] - Reconstruct Page [1] Size [45]
Router [3] - Packet stored ID[1] from router [2] with destiny router [3]
Router [3] - Reconstruct Page [3] Size [47]
Router [3] - Packet stored ID[1] from router [1] with destiny router [3]
```

We can see that page [1] was generated with a size [45], so it was fragmented into a single packet and reconstructed in the next cycle.

## Class Diagram

![TP2_PARADIGMAS_UML_CLASS.png](img/UML_CLASS.png)

## Functionality

Here is an explanation of how the simulation works.

### Network Generation

The initial phase of the process involves generating a graph that acts as the structural representation of the network. To ensure essential properties, the graph is required to be connected, meaning there is at least one path between any pair of nodes in the network. This design approach ensures consistency in the simulation.

Each node in the graph represents a router in the network topology. The graph's edges represent the connections between routers, and the weights of these edges indicate the bandwidth associated with each connection.

Additionally, pages are initially generated for as many routers as the system has. The size of these pages is randomly set between 1 and the value specified as a parameter.

### Transmission Initialization

Once the network and initial pages are generated, the simulation can begin. For each router in the network, a random destination router and a random page are selected for transmission. The router fragments the page into packets and stores them in an internal buffer.

Then, random paths are generated from each router to each destination. These shortest paths are stored in a network list that each router can consult for sending packets.

### First Cycle - Packet Transmission

In the first cycle, each router selects a packet to send and places it in the connection buffer. If there are no packets, the router waits until the next transmission cycle and checks again if there are packets to send.

### Second Cycle - Packet Reception

In the next cycle, each router retrieves a packet from the connection buffer for each connection it has. The router checks the destination of the packet; if the destination is different from the current router, the packet is placed in the internal transmission buffer to be sent in the next cycle. If the packet's destination is the current router, it is stored in an internal buffer to await the reconstruction of the page. Once all packets are received and the page is reconstructed, the page is discarded to simulate its delivery to a terminal.

### Subsequent Cycles

In the following cycles, packet transmission and reception continue, one by one.

To enrich the simulation, every 12 cycles, new pages are generated for as many routers as the network has, and optimal paths are recalculated. Additionally, optimal paths are recalculated every 6 cycles based on the network's bandwidth.
