If your Linux system has a /etc/sysctl.d directory, use:


echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf
sudo sysctl -p /etc/sysctl.d/99-tailscale.conf
Otherwise, use:


echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.conf



If your Linux node uses firewalld, you might need to allow masquerading due to a known issue. As a workaround, you can allow masquerading with this command:



firewall-cmd --permanent --add-masquerade


then: 


sudo tailscale set --advertise-routes=192.0.2.0/24,198.51.100.0/24

and allow your sybnets in Tilscale admin panel
