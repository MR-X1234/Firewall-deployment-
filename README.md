# Firewall-deployment-
Palo Alto Layar 3 deployment 
1. Security Policy Rules


2. NAT (Network Address Translation) Rules


3. Routing


4. Interface Configuration



Below is a step-by-step explanation and arrangement of the images in logical order, based on typical firewall setup workflow:

![IMG20250715165157](https://github.com/user-attachments/assets/d7bb32d1-27e3-4902-b395-05c6912368e6)


Interface Overview

Interfaces ethernet1/1 and ethernet1/2 are configured with:

ethernet1/1: IP 192.168.1.100/24 in INSIDE-Zone

ethernet1/2: IP 192.168.3.1/24 in OUTSIDE-ZONE


Both interfaces are part of the default virtual router.

![IMG20250715165013](https://github.com/user-attachments/assets/0e63805d-2ce4-417b-9805-2fa1e6bc0174)


ethernet1/1 Settings

Layer3 interface

IP Address: 192.168.1.100/24

Assigned to:

Virtual Router: default

Security Zone: INSIDE-Zone

![IMG20250715164951](https://github.com/user-attachments/assets/e1dd695d-e420-47ca-94e2-1904877fd7a0)

ethernet1/1 Interface Config Details

Same interface setup confirmation:

Layer3, Static IP, part of INSIDE-Zone, default virtual router


2. Set Static Route (Default Route)
   
![IMG20250715165339](https://github.com/user-attachments/assets/2097426f-9210-46d6-81f5-1c59b561c4cd)

![IMG_20250715_165511](https://github.com/user-attachments/assets/296dca47-0135-4fcd-bf06-584659170af0)

Name: routing

Destination: 0.0.0.0/0 (default route)

Next Hop IP: 10.11.81.239 (outside gateway)

Interface: ethernet1/1

However, this may be incorrect. The next hop for ethernet1/1 should be within the 192.168.1.0/24 network. If this IP belongs to the ISP or router on the OUTSIDE, it should instead use ethernet1/2. Check physical/logical connection details.

3. Configure NAT Rule

NAT Rule - Original Packet

Source Zone: INSIDE-Zone

Destination Zone: OUTSIDE-ZONE

Destination Interface: ethernet1/1 (again may be incorrect; should likely be ethernet1/2)

Service: any

![IMG20250715170103](https://github.com/user-attachments/assets/e2a35d22-8a23-4e19-a748-4b942938bf94)

NAT Rule - Translated Packet

Translation Type: Dynamic IP and Port

Address Type: Interface Address

Interface: ethernet1/1

IP Address: 192.168.1.100/24 — NAT will use this IP as the source

![IMG20250715170126](https://github.com/user-attachments/assets/93af7a34-5e18-4181-8c31-1460946efb1b)

Possible error: NAT should typically translate to the OUTSIDE interface IP, i.e., 192.168.3.1/24 (from ethernet1/2). Review your interface assignments.


4.Create Security Policy Rule

Source in Security Rule

Source Zone: INSIDE-Zone

Source Address: Any

![IMG20250715170211](https://github.com/user-attachments/assets/bf111725-e6c0-4641-88fe-5cdc5540a0ff)


Destination in Security Rule

Destination Zone: OUTSIDE-ZONE

Destination Address: Any

![IMG20250715170219](https://github.com/user-attachments/assets/4e6f9863-e9ad-4686-8189-652ea70094e3)

1	Assign IPs and zones: <br> - ethernet1/1 (192.168.1.100/24) → INSIDE-Zone <br> - ethernet1/2 (192.168.3.1/24) → OUTSIDE-ZONE
2	Create NAT rule to translate inside IPs to outside IP/interface (likely ethernet1/2 IP)
3	Add a default route (0.0.0.0/0) with next hop pointing to ISP/router on ethernet1/2 network
4	Add security policy: from INSIDE-Zone to OUTSIDE-ZONE, allow any application/service


