<p align="center">


<h2>Video Demonstration</h2>

- ### [YouTube: Day 28 Lab: OSPF Pt. 3](https://www.youtube.com/watch?v=xQDTEoT1K8I)

<h2>Environments and Technologies Used</h2>

- Jeremy's IT Lab Youtube Channel
- Cisco Packet Tracer
  

  
<h2>Operating Systems Used </h2>

- Cisco IOS


<h2>Step-by-Step</h2>

<b>🔹 Step 1 – Configure Serial Link & Enable OSPF (R1 ↔ R2)</b>

1. Configure R1

enable

conf t

interface s0/0/0

ip address 192.168.12.1 255.255.255.252

clock rate 128000     # only if DCE

no shutdown

2. Verify DCE side (R1)

do show controllers s0/0/0

3. Configure R2

enable

conf t

interface s0/0/0

ip address 192.168.12.2 255.255.255.252

no shutdown

4. Enable OSPF on Interfaces

On R1

interface s0/0/0

ip ospf 1 area 0

interface g0/0

ip ospf 1 area 0

On R2

interface s0/0/0

ip ospf 1 area 0

5. Verify OSPF

show ip ospf interface

show ip route

<b>🔹 Step 2 – Fix Missing Route (10.0.2.0/24)</b>

1. Check OSPF neighbors

show ip ospf neighbor

2. Check routing table

show ip route

3. Check OSPF network type mismatch

On R4

show ip ospf interface g0/1

On R3

show ip ospf interface g0/1

🚨 Problem:

R3 = point-to-point

R4 = broadcast

4. Fix on R3

conf t

interface g0/1

no ip ospf network point-to-point

5. Verify fix

show ip route

ping 10.0.2.1

<b>🔹 Step 3 – Fix OSPF Neighbor Issue with R5</b>

1. Check neighbors on R2/R4

show ip ospf neighbor

2. Check interface settings (R5)

show ip ospf interface g0/0

🚨 Problem:

Hello timer = 5

Dead timer = 20

👉 Must match neighbors (10 / 40)

3. Fix timers on R5

conf t

interface g0/0

no ip ospf hello-interval

no ip ospf dead-interval

4. Verify adjacency

show ip ospf neighbor

<b>🔹 Step 4 – Fix Default Route to Internet</b>

1. Test connectivity

ping 8.8.8.8

2. Check OSPF config (R5)

show running-config | section ospf

3. Check routing table (R5)

show ip route

🚨 Problem:

No default route exists → nothing to advertise

4. Configure default route on R5

conf t

ip route 0.0.0.0 0.0.0.0 203.0.113.2

5. Verify on R1

show ip route

6. Test again

ping 8.8.8.8

<b>🔹 Step 5 – Verify LSDB (OSPF Database)</b>

show ip ospf database
