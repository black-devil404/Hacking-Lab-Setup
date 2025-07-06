#for driver setup refer foloowing link
https://github.com/nlkguy/archer-t2u-plus-linux

wpa_passphrase "SSID" "password" | sudo tee /etc/wpa_supplicant.conf
sudo wpa_supplicant -B -i wlxb4b0240915e3 -c /etc/wpa_supplicant.conf
dhclient wlxb4b0240915e3

✅ Step-by-Step: Auto Connect Wi-Fi on Boot
🛠️ 1. Create a WPA Supplicant Config (already done)

Your file: /etc/wpa_supplicant.conf

Make sure it exists and is readable:

sudo cat /etc/wpa_supplicant.conf

🧠 2. Create a systemd service to start Wi-Fi on boot

Create a new systemd service file:

sudo nano /etc/systemd/system/wifi-connect.service

Paste this:

[Unit]
Description=Connect to Wi-Fi using wpa_supplicant
After=network.target

[Service]
Type=simple
ExecStart=/sbin/wpa_supplicant -i wlxb4b0240915e3 -c /etc/wpa_supplicant.conf
Restart=always

[Install]
WantedBy=multi-user.target

Save and exit (Ctrl + O, Enter, Ctrl + X).
🌐 3. Enable the service

sudo systemctl enable wifi-connect.service
sudo systemctl start wifi-connect.service

Check its status:

sudo systemctl status wifi-connect.service

🌐 4. Add DHCP Client Auto-start

Edit or create a file:

sudo nano /etc/systemd/system/dhclient-wifi.service

Paste:

[Unit]
Description=DHCP Client for Wi-Fi
After=wifi-connect.service

[Service]
Type=simple
ExecStart=/sbin/dhclient wlxb4b0240915e3
Restart=always

[Install]
WantedBy=multi-user.target

Save and exit.

Then:

sudo systemctl enable dhclient-wifi.service
sudo systemctl start dhclient-wifi.service

🔁 Now Reboot and Test

sudo reboot
