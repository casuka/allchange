# allchange

import os
import subprocess

def get_current_ip(interface):
    result = subprocess.run(["ifconfig", interface], capture_output=True, text=True)
    for line in result.stdout.split('\n'):
        if 'inet ' in line:
            return line.split()[1]

def get_current_mac(interface):
    result = subprocess.run(["ifconfig", interface], capture_output=True, text=True)
    for line in result.stdout.split('\n'):
        if 'ether ' in line:
            return line.split()[1]

def change_ip_address(interface, new_ip, new_netmask, new_gateway):
    current_ip = get_current_ip(interface)
    print(f"Mevcut IP adresi: {current_ip}")
    subprocess.run(["sudo", "ifconfig", interface, new_ip, "netmask", new_netmask])
    subprocess.run(["sudo", "route", "add", "default", "gw", new_gateway, interface])
    print(f"Yeni IP adresi: {new_ip}")

def change_mac_address(interface, new_mac):
    current_mac = get_current_mac(interface)
    print(f"Mevcut MAC adresi: {current_mac}")
    subprocess.run(["sudo", "ifconfig", interface, "down"])
    subprocess.run(["sudo", "ifconfig", interface, "hw", "ether", new_mac])
    subprocess.run(["sudo", "ifconfig", interface, "up"])
    print(f"Yeni MAC adresi: {new_mac}")

# Örnek kullanım
interface = "eth0"  # Ağ arayüzü ismi
new_ip = "192.168.1.100"
new_netmask = "255.255.255.0"
new_gateway = "192.168.1.1"
new_mac = "00:11:22:33:44:55"

change_ip_address(interface, new_ip, new_netmask, new_gateway)
change_mac_address(interface, new_mac)

print(f"{interface} için IP adresi ve MAC adresi başarıyla değiştirildi.")
