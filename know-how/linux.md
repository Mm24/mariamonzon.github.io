## User accounts 

### Avoiding a password for sudo in
    echo "`whoami` ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/`whoami` && sudo chmod 0440 /etc/sudoers.d/`whoami`
    
### Set up the Desktop Environment

Now, for the exciting bit. We’ll set up a desktop environment and use a remote access tool to interact with it. Remote Desktop is included with Windows. If you can install WSL you already meet the minimum version requirements (I think Windows Pro or higher is needed).

We’ll use XFCE4, as it’s the simplest.

    Run sudo apt install -y xrdp xfce4 xfce4-goodies

Then, run the following:

    sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak
    sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
    sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini
    sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
    echo xfce4-session > ~/.xsession
