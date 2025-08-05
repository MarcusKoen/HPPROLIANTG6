# Step 1: Completely remove any existing VirtualBox
sudo apt remove --purge -y virtualbox virtualbox-dkms virtualbox-ext-pack
sudo apt autoremove -y

# Step 2: Add the correct repository for Ubuntu 22.04 (jammy)
sudo rm /etc/apt/sources.list.d/virtualbox.list  # Remove old focal entry
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo gpg --dearmor --yes --output /usr/share/keyrings/oracle-virtualbox-2016.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] https://download.virtualbox.org/virtualbox/debian jammy contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list

# Step 3: Install dependencies
sudo apt update
sudo apt install -y libssl3 libvpx7

# Step 4: Install VirtualBox 7.0
sudo apt install -y virtualbox-7.0

# Step 5: Install Extension Pack
VBOX_VER=$(vboxmanage -v | cut -d'r' -f1)
wget https://download.virtualbox.org/virtualbox/$VBOX_VER/Oracle_VM_VirtualBox_Extension_Pack-$VBOX_VER.vbox-extpack
sudo vboxmanage extpack install Oracle_VM_VirtualBox_Extension_Pack-$VBOX_VER.vbox-extpack --replace

# Step 6: Add your user to the vboxusers group
sudo usermod -aG vboxusers $USER

# Step 7: Verify installation
vboxmanage -v
