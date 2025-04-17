# Astrisk_FreePBX

## 1. Setup Asterisk<br> 
### Step 1 – Install Required Dependencies<br> 
* Before starting, install all required dependencies to compile Asterisk.<br>

    * sudo apt update <br> 
    * sudo apt upgrade<br> 
    * sudo apt-get install unzip git sox gnupg2 curl libnewt-dev libssl-dev libncurses5-dev subversion libsqlite3-dev build-essential libjansson-dev libxml2-dev libedit-dev uuid-dev subversion -y<br>

### Step 2 – Install Asterisk
* Download version 21:
    * cd /usr/src <br>
    * sudo wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-21-current.tar.gz
    * sudo tar xvf asterisk-21-current.tar.gz
    * cd asterisk-21*/<br>
* Some require dependencies:
    * sudo contrib/scripts/get_mp3_source.sh
    * sudo contrib/scripts/install_prereq install<br>
* Next, configure Asterisk:<br>
    * sudo ./configure
    * sudo make -j2
    * sudo make install
    * sudo make samples
    * sudo make config
    * sudo ldconfig
### Step 3 – Configure Asterisk
* Create a separate user and group for Asterisk.<br>
    * sudo groupadd asterisk
    * sudo useradd -r -d /var/lib/asterisk -g asterisk asterisk<br>
* Add required users to the Asterisk group:<br>
    * sudo usermod -aG audio,dialout asterisk<br>
* Set permissions:<br>
    * sudo chown -R asterisk.asterisk /etc/asterisk
    * sudo chown -R asterisk.asterisk /var/{lib,log,spool}/asterisk
    * sudo chown -R asterisk.asterisk /usr/lib/asterisk<br>
* Edit /etc/default/asterisk file:<br>
    * sudo nano /etc/default/asterisk    
        *   AST_USER="asterisk"
        *   AST_GROUP="asterisk"
![Screenshot from 2025-04-17 12-14-16](https://github.com/user-attachments/assets/86ac5fa3-feaf-4acb-a67c-fbf38e8ce1d1)
        Save and close the file (press ctrl+x then y)<br>
* Edit etc/asterisk/asterisk.conf file:<br>
    * sudo nano /etc/asterisk/asterisk.conf
        * runuser = asterisk ; The user to run as.
        * rungroup = asterisk ; The group to run as.
![Screenshot from 2025-04-17 13-19-24](https://github.com/user-attachments/assets/60ceeb26-311f-4961-9198-b166a8866b01)
        Save and close the file (press ctrl+x then y)<br>
* Restart and enable the Asterisk service:<br>
    * sudo systemctl restart asterisk
    * sudo systemctl enable asterisk<br>
* Verify status of Asterisk service:<br>
    * sudo systemctl status asterisk
* Open Asterisk command-line interface:
    * sudo asterisk -rvvv
![Screenshot from 2025-04-17 13-25-32](https://github.com/user-attachments/assets/7c7455ea-f874-44db-ac22-c0248e1222a4)
    Varify version 21.7.0
* Exit CLI:
    * exit
![Screenshot from 2025-04-17 13-27-21](https://github.com/user-attachments/assets/d1c2b462-7954-439c-af58-94ae515a5132)

### Step 4 – Install FreePBX
* Install Required Dependencies:
    * sudo apt install software-properties-common
    * sudo add-apt-repository ppa:ondrej/php -y
* Install Apache, MariaDB, PHP
    * sudo apt-get install -y apache2 mariadb-server mariadb-client bison flex \
php8.2 php8.2-curl php8.2-cli php8.2-common php8.2-mysql php8.2-gd \
php8.2-mbstring php8.2-intl php8.2-xml php-pear
* Download version 17:
    * sudo wget http://mirror.freepbx.org/modules/packages/freepbx/freepbx-17.0-latest.tgz
    * sudo tar -xvzf freepbx-16.0-latest.tgz
    * cd freepbx
* Install Node.js package
    * sudo apt-get install nodejs npm -y
    * sudo ./install -n
* Get the following output
    ![Screenshot from 2025-04-17 13-33-53](https://github.com/user-attachments/assets/63590429-4f9e-4471-aede-346e62723718)
* Install the pm2 package 
    * sudo fwconsole ma install pm2
* Enable the Apache
    * sudo a2enmod rewrite
    * sudo systemctl restart apache2
### Step 5 – Access FreePBX
* Now, open your web browser and access the FreePBX web interface using the URL http://your-server-ip/admin
 






  
    
