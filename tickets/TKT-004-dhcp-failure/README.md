TKT-003: All Users Have No Internet, Router Unreachable
FieldValueReported byMultiple users (PC1, PC2)PriorityHighAffectedEntire siteLayer1–2 (router interface down)Status✅ Resolved💡 Based on a real incident. I experienced a network where every device lost internet access, the router's web GUI could not be reached, and pinging the gateway failed. The root cause there was not confirmed, so this lab recreates those symptoms in a controlled environment to practise the diagnosis.
1. Reported Issue
"Nobody in the office has internet, and the router's configuration page won't open."
2. Initial Observations
• Every client is affected, not just one, which points to a shared component (switch uplink, router, or internet link) rather than individual PCs.
• The router's management address is the same as the default gateway (192.168.1.1), so losing the GUI and losing the gateway are likely the same problem.
3. Troubleshooting Steps
#Action / CommandResultConclusion1ipconfig on PC1Still has 192.168.1.x (old DHCP lease)Addressing looks normal — the problem is reachability2ping 192.168.1.1 from PC1<!-- exact message you got -->Gateway not reachable3Same test from PC2Same failureAll users affected — focus on shared devices4arp -a on PC1No entry for 192.168.1.1Gateway never answered ARP — Layer 2 problem toward R15show interfaces status on SW1Gi0/1 (uplink to R1) notconnectProblem is on the link between SW1 and R16show ip interface brief on R1 (via console)G0/0 administratively down / downR1's LAN interface was manually shut downPC1 – ping 192.168.1.1:
<!-- Paste your real ping output here -->
PC1 – arp -a:
<!-- Paste your real arp -a output here -->
R1 – show ip interface brief:
<!-- Paste your real R1 output here -->
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     192.168.1.1     YES manual administratively down down
GigabitEthernet0/1     203.0.113.2     YES manual up                    up
Symptom Diagnosis
4. Root Cause
R1's LAN interface GigabitEthernet0/0 (the default gateway, 192.168.1.1) was administratively shut down. With the gateway interface down:
• No client could resolve the gateway's MAC address (ARP), so all traffic leaving the LAN failed.
• The DHCP server on R1 was unreachable, so any client renewing its address would receive none.
• The router's management interface on the same IP was unreachable.
5. Resolution
conf t
interface g0/0
 no shutdown
end
write memory
6. Verification
TestResultshow ip interface brief on R1G0/0 up / up ✅show interfaces status on SW1Gi0/1 connected ✅ping 192.168.1.1 from PC1 and PC2Successful ✅ping 198.51.100.10 (server)Successful ✅Browser → http://www.lab.localPage loads ✅arp -a on PC1Entry for 192.168.1.1 present ✅Verification
7. Lessons Learned
• Scope first: When all users fail at once, check shared devices (uplink, router) before troubleshooting individual PCs.
• administratively down vs down: administratively down means someone ran shutdown (a configuration issue); down alone means no physical signal (cable, port or device failure). This tells you whether to fix config or hardware.
• ARP is the clue: An empty ARP entry for the gateway shows the problem is at Layer 2 or below, before routing is even involved.
• Out-of-band access matters: When the gateway is down, you can't manage the router over the network (no GUI, no SSH). A console connection is the way in. In the real incident, losing the GUI was a symptom, not a separate problem.
• Real-world note: On home/small-office routers, the same symptoms are often caused by a frozen router, a factory reset that changed its IP, or a failed switch between clients and the router — a power cycle and checking ipconfig for the actual gateway are good first steps.
Files
• lab-broken.pkt – lab with R1 G0/0 shut down
• lab-fixed.pkt – lab after the fix
• screenshots/ – symptom, diagnosis and verification evidence

