# 🔗 Link Failure Detection and Recovery using SDN

## 📌 Overview

This project demonstrates how **Software Defined Networking (SDN)** enables dynamic routing and self-healing networks.
Using a centralized controller (POX) and Mininet, the network detects link failures and automatically reroutes traffic through an alternate path, restoring connectivity without manual intervention.

---

## 🎯 Objective

* Detect link failures in a network
* Dynamically update routing using an SDN controller
* Restore connectivity using an alternate path
* Observe flow rule changes and network performance

---

## 🧰 Tools & Technologies

* **Mininet** – Network emulator
* **POX Controller** – SDN controller (learning switch)
* **Open vSwitch (OVS)** – Software switch
* **iperf** – Throughput measurement
* **ping** – Latency and connectivity testing

---

## 🧱 Network Topology

```
h1 ─── s1 ─── s2 ─── h2
         │
         s3
```

### 🔹 Paths

* **Primary Path:** h1 → s1 → s2 → h2
* **Backup Path:** h1 → s1 → s3 → s2 → h2

---

## ⚙️ Implementation Details

### 🔹 Controller Logic

* Handles **packet_in events** for unknown traffic
* Learns MAC addresses dynamically
* Installs **flow rules** in switches
* Uses **match–action mechanism** for forwarding

### 🔹 Flow Rule Example

* **Match:** Destination MAC address
* **Action:** Forward packet to appropriate port

---

## 🚀 Execution Steps

### 1. Start POX Controller

```bash
cd pox
./pox.py forwarding.l2_learning
```

### 2. Run Mininet Topology

```bash
sudo mn --custom topo.py --topo mytopo --controller=remote
```

### 3. Verify Connectivity

```bash
pingall
```

### 4. Measure Performance

```bash
h1 ping h2
iperf h1 h2
```

### 5. Simulate Link Failure

```bash
link s1 s2 down
```

### 6. Observe Recovery

```bash
h1 ping h2
```

### 7. Restore Link

```bash
link s1 s2 up
```

---

## 📊 Observations

* Initial packets are dropped due to **controller learning phase**
* Controller installs flow rules after receiving packet_in events
* When the primary link fails:

  * Packets are temporarily dropped
  * Controller recalculates path
  * Traffic is rerouted via backup path (s3)
* Connectivity is restored automatically
* When the link is restored, normal routing resumes

---

## 🔁 Failure Handling

When the link between s1 and s2 fails, the controller detects the failure and reroutes traffic via switch s3 by installing new flow rules dynamically.

## 📸 Results & Proof

The following results were observed:

* ✔ Successful communication in normal conditions
* ✔ Temporary packet loss during link failure
* ✔ Automatic recovery via alternate path
* ✔ Stable performance after recovery

### Included Screenshots

* Normal connectivity (`pingall`)
* Flow table entries (`ovs-ofctl dump-flows`)
* Latency measurement (`ping`)
* Throughput (`iperf`)
* Link failure scenario
* Recovery behavior
* Final restored network

---

## 🧠 Key Concepts Demonstrated

* Software Defined Networking (SDN)
* Controller–Switch interaction
* Flow rule installation
* Match–Action forwarding
* Dynamic routing
* Self-healing networks

---

## 🎯 Conclusion

This project successfully demonstrates how SDN enables **intelligent and adaptive networking**.
By separating the control plane from the data plane, the controller dynamically manages traffic, ensuring **fault tolerance and efficient routing**.

---

## 🔥 Future Enhancements

* Implement custom controller logic for faster failure detection
* Add shortest path routing algorithm (Dijkstra)
* Integrate real-time monitoring and visualization
* Extend topology to larger networks

---
This project demonstrates self-healing networks using SDN.
