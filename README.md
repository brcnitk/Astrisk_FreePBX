

# VoIP Setup using Asterisk & FreePBX

Voice over Internet Protocol (VoIP) is a technology that allows you to make voice calls using a broadband Internet connection instead of a regular (or analog) phone line. Here’s a practical overview of how VoIP works and its key components:

## 1. Basic Components of VoIP

- **IP Phones**: These are specialized phones that connect directly to your Internet connection. They can be hardware-based (similar to traditional phones) or software-based (softphones that run on computers or mobile devices).
- **SIP (Session Initiation Protocol)**: This is a signaling protocol used to initiate, maintain, and terminate real-time sessions that include voice, video, and messaging applications.
- **Codecs**: These are used to compress and decompress voice signals to ensure efficient transmission over the Internet. Common codecs include G.711, G.729, and Opus.
- **VoIP Gateway**: This device converts traditional analog voice signals into digital data that can be transmitted over the Internet.
- **PBX (Private Branch Exchange)**: A VoIP-enabled PBX can manage internal calls within an organization and connect to external lines via VoIP.
- **SIP Trunking**: This allows you to connect your PBX to the Internet, enabling VoIP calls without the need for traditional phone lines.

## 2. How VoIP Works

- **Call Initiation**: When you make a call using a VoIP phone, the device converts your voice into digital data packets.
- **Packet Transmission**: These data packets are transmitted over the Internet to the recipient.
- **Call Routing**: The VoIP service provider routes the call to the recipient’s phone, whether it’s another VoIP phone, a traditional landline, or a mobile phone.
- **Packet Reception**: The recipient’s device receives the data packets and converts them back into voice signals.
- **Call Termination**: The call ends when either party hangs up, and the connection is terminated.

## 3. Practical Setup

- **Internet Connection**: A stable and high-speed Internet connection is crucial for VoIP. A minimum of 100 kbps per line is recommended.
- **VoIP Service Provider**: Choose a reliable VoIP service provider that offers the features you need, such as call forwarding, voicemail, and conferencing.
- **Network Configuration**: Ensure your network is configured to prioritize VoIP traffic (Quality of Service or QoS) to minimize latency, jitter, and packet loss.
- **Hardware/Software**: Depending on your needs, you can use IP phones, softphones, or even adapters to connect traditional phones to your VoIP system.

## 4. Advantages of VoIP

- **Cost-Effective**: VoIP typically costs less than traditional phone services, especially for long-distance and international calls.
- **Flexibility**: You can make and receive calls from anywhere with an Internet connection.
- **Scalability**: It’s easy to add or remove lines as your business grows or changes.
- **Advanced Features**: VoIP systems often include features like call forwarding, voicemail-to-email, auto-attendant, and more.

## 5. Challenges and Considerations

- **Internet Dependency**: VoIP relies on your Internet connection, so any issues with your connection can affect call quality.
- **Power Outages**: Unlike traditional phones, VoIP phones may not work during a power outage unless you have a backup power source.
- **Security**: VoIP systems can be vulnerable to cyber threats, so it’s important to implement security measures like encryption and firewalls.

## 6. Practical Applications

- **Business Communication**: VoIP is widely used in businesses for internal and external communication, often integrated with other business applications.
- **Remote Work**: VoIP enables remote workers to stay connected with the office as if they were on-site.
- **Customer Service**: Call centers use VoIP to manage large volumes of calls efficiently, often integrating with CRM systems.

## 7. Future Trends

- **5G Integration**: The rollout of 5G networks is expected to enhance VoIP call quality and reliability.
- **AI and Automation**: AI-driven features like voice recognition, automated call routing, and virtual assistants are becoming more common in VoIP systems.
- **Unified Communications**: VoIP is increasingly being integrated with other communication tools like video conferencing, instant messaging, and collaboration platforms.

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
        *   AST_GROUP="asterisk"<br>
![Screenshot from 2025-04-17 12-14-16](https://github.com/user-attachments/assets/86ac5fa3-feaf-4acb-a67c-fbf38e8ce1d1)<br>
        Save and close the file (press ctrl+x then y)<br>
* Edit etc/asterisk/asterisk.conf file:<br>
    * sudo nano /etc/asterisk/asterisk.conf
        * runuser = asterisk ; The user to run as.
        * rungroup = asterisk ; The group to run as.<br>
![Screenshot from 2025-04-17 13-19-24](https://github.com/user-attachments/assets/60ceeb26-311f-4961-9198-b166a8866b01)<br>
        Save and close the file (press ctrl+x then y)<br>
* Restart and enable the Asterisk service:<br>
    * sudo systemctl restart asterisk
    * sudo systemctl enable asterisk<br>
* Verify status of Asterisk service:<br>
    * sudo systemctl status asterisk
* Open Asterisk command-line interface:
    * sudo asterisk -rvvv<br>
![Screenshot from 2025-04-17 13-25-32](https://github.com/user-attachments/assets/7c7455ea-f874-44db-ac22-c0248e1222a4)<br>
    Varify version 21.7.0
* Exit CLI:
    * exit<br>
![Screenshot from 2025-04-17 13-27-21](https://github.com/user-attachments/assets/d1c2b462-7954-439c-af58-94ae515a5132)<br>




## 2. Setup FreePBX
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
* Get the following output<br>
    ![Screenshot from 2025-04-17 13-33-53](https://github.com/user-attachments/assets/63590429-4f9e-4471-aede-346e62723718)<br>
* Install the pm2 package 
    * sudo fwconsole ma install pm2
* Enable the Apache
    * sudo a2enmod rewrite
    * sudo systemctl restart apache2
### Access FreePBX
* Now, open your web browser and access the FreePBX web interface using the URL http://your-server-ip/admin<br>
![image](https://github.com/user-attachments/assets/ddce947f-9612-403a-a61e-c93c560fbd15)<br>
Provide your Admin user details and click on the Setup System button.
* Should see the following page:<br>
![image](https://github.com/user-attachments/assets/dea9c6f8-44da-45f7-8ec5-ff0e36d9c99a)<br>
  Click on the FreePBX Administration button.<br> 
![image](https://github.com/user-attachments/assets/470b28c7-eca6-4110-83db-de68276facc3)<br>
  Enter your admin username and password, and click on the Continue button.<br>
![image](https://github.com/user-attachments/assets/bf3fc8d2-db93-4db7-a4b9-33cfc09d0012)<br>




## 3. Basic calling set-up with Asterisk
* Some Asterisk Firewall Rules
   * sudo iptables -A INPUT -p udp -m udp --dport 5060 -j ACCEPT
   * sudo   iptables -A INPUT -p udp -m udp --dport 5036 -j ACCEPT
   * sudo   iptables -A INPUT -p udp -m udp --dport 10000:20000 -j ACCEPT
* set our configs
   * cd /etc/asterisk
   * sudo nano pjsip_custom.conf(This file should be empty, enter this code.)
 
           ;================================ TRANSPORTS ==
           ; Our primary transport definition for UDP communication behind NAT.
           [transport-udp-nat]
           type = transport
           protocol = udp
           bind = 0.0.0.0
           ;================================ CONFIG FOR SIP ITSP ==
         
           [calling](!)                    ; template
           type=endpoint                   ; specify the below are the configurations related to an endpoint (a device)
           context=interaction             ; *** it refers to the context set in the dialplan (extensions.conf) ***
           allow = !all, ulaw, alaw  ; do not allow all codecs and only allow audio codecs - ulaw and alaw
           direct_media=no                   ; do not allow two devices directly talking to each other
           trust_id_outbound=yes
           rtp_symmetric=yes
           force_rport=yes
           rewrite_contact=yes
           device_state_busy_at=1
           dtmf_mode=rfc4733

           [auth-userpass](!)            ; template 
           type = auth                       ; specify the below are the configurations related to authentication
           auth_type = userpass      ; specify the type of the authentication is based on username and password
         
           [aor-single-reg](!)           ; template
           type = aor
           max_contacts = 1            ; specify the maximum sip addresses allocated. Here, that means each sip address could only connect to one device.
         
           [7000](calling)                 ; endpoint 7000 inherits the settings in calling template
           auth=7000                       ; authentication = 7000
           aors=7000                       ; address of record = 7000
           callerid = 7000 <7000>      ; caller's id = 7000
         
           [7000](auth-userpass)     ; account creation for endpoint 7000
           password = 7000
           username = 7000
         
           [7000](aor-single-reg)
         
           [7100](calling)
           auth=7100
           aors=7100
           callerid = 7100 <7100>
         
           [7100](auth-userpass)
           password = 7100
           username = 7100
         
           [7100](aor-single-reg)
     ![Screenshot from 2025-04-17 14-15-36](https://github.com/user-attachments/assets/4c3d5767-e928-46ed-b541-03bf9b97baae)<br>

* Open extensions_custom.conf(This file should be empty, enter this code.)
   * sudo nano extensions_custom.conf

              [globals]
           INTERNAL_DIAL_OPT=,30
         
           [interaction]                       ; refers to the `context=interaction` in calling template in pjsip.conf
           exten = 7000,1,Answer()               ; the first argument 7000 refers to the number you dial on your softphone. 
           same = n,Dial(PJSIP/7000,60)      ; PJSIP/7000 refers to the endpoint 7000 , 60 means waiting for 60 seconds
           same = n,Playback(vm-nobodyavail)
           same = n,Voicemail(7000@main)
           same = n,Hangup()
         
           exten = 7100,1,Answer()
           same = n,Dial(PJSIP/7100,60)
           same = n,Playback(vm-nobodyavail)
           same = n,Voicemail(7000@main)
           same = n,Hangup()
         
           ; same means following the same extension
           ; n means the next action. 
     
      ![Screenshot from 2025-04-17 14-15-54](https://github.com/user-attachments/assets/a758dd1d-ddc2-4b72-9677-2016cd3e3fc9)<br>

      Now we have 2 Account(7000,7100). We need to set up Softphones.




## 4. set up Softphones.

* Download Zoiper5(Android Windows Linux)<br>
![Screenshot from 2025-04-17 14-23-15](https://github.com/user-attachments/assets/17fcb2cc-a862-4a9b-ae33-108ec4ee7117)<br>
Click Continue as a Free user.<br>
![Screenshot from 2025-04-17 14-29-41](https://github.com/user-attachments/assets/9d4979a0-c67f-4874-8428-931eb8dc02e3)<br>
Enter your 7000@<Your_ip> and Password, and Login.<br>
![image](https://github.com/user-attachments/assets/ba5cc7e5-7e42-4542-80eb-5ae014b2f5dc)<br>
Click Next.<br>
![Screenshot from 2025-04-17 14-32-10](https://github.com/user-attachments/assets/baac754d-cac3-4e97-aabc-eae16f96b7f9)<br>
Click Skip.<br>
![image](https://github.com/user-attachments/assets/a3b487b0-e8d8-40a8-9456-e953ae2ba147)<br>
Will see green in SIP UDP, and click next.<br>
![image](https://github.com/user-attachments/assets/1fb090c3-a4f5-496c-ad3d-1e89ddd7b786)<br>

### Do similar for 7100 in another system (Android Windows Linux).<br>
![WhatsApp Image 2025-04-17 at 14 53 41](https://github.com/user-attachments/assets/f71abf64-d443-4a22-996d-345bbece07be)<br>

### Note : Both system must connect with same network.

## 5. Calling.

* Dial 7100 in 7000<br>
![Screenshot from 2025-04-17 14-47-50](https://github.com/user-attachments/assets/4ed7aa37-9a50-402b-bb0b-0478ea58cb60)<br>

* 7000<br>
  ![Screenshot from 2025-04-17 14-51-40](https://github.com/user-attachments/assets/b816bf64-761e-48ee-8297-e1fe6555a6d3)<br>
* 7001<br>
![WhatsApp Image 2025-04-17 at 14 53 41 (1)](https://github.com/user-attachments/assets/8f2b4710-29f2-42d2-b2ad-fe15a12ce7d7)

## 6. Basic calling set-up with FreePBX<br>

* Before Setup Update FreePBX. Go FreePBX GUI click Admin -> Module Admin. Then select all 4 repositories(Standard, Unsupported, Extended, Commercial). Then Click Check Online Then Download all then Upgrade all then process and at last apply config

![Screenshot from 2025-04-17 15-27-41](https://github.com/user-attachments/assets/2ed4ddf3-8edd-4397-b98a-77c8e84c254f)<br>
### calling set-up<br>
Click Connectivity -> Extension. Then Click add Extension -> Add New SIP[chan_pjsip] Extention. And enter display name, Secret, Username, Password for New user(Same in all four (eg. 8000)).  And if groups have some entry remove it. Gropes column should be empty.<br>
![Screenshot from 2025-04-17 15-18-52](https://github.com/user-attachments/assets/685faacb-febb-4444-8d21-d0f37268f6b7)<br>
click submit and then apply config.

### Now follow 4th & 5th Point (with 8000)
now we have 3 calling numbers(7000,7100,8000). We can communicate with all three. We only need to connect with same network. And we can also create any no of calling numbers with Asterisk and FreePBX.



## 7. Setup a IVR using FreePBX
* First we need some recording. Click Admin -> System Redording -> Add Recording. Enter Name and Browse Recording and click Submit -> Apply config.<br>
  ![Screenshot from 2025-04-17 15-45-24](https://github.com/user-attachments/assets/b045cc72-383f-4032-94ff-1daf3a34a3cd)<br>

* **Add Announcement**. Click Application -> Announcement -> Add Announcement. Enter Decription, Recording. Then Submit -> Apply config.<br>
 ![Screenshot from 2025-04-17 15-46-16](https://github.com/user-attachments/assets/95fb0e0c-4297-4366-a666-fb0b9ed906c5)<br>
 
* **Add IVR**. Click Applications -> IVR -> Add IVR. Enter Name, Description, Announcement(play just after call), Enable Direct Dial. then at last in IVR Entries setup tha digits and destinations(what will play after dial that digit). And then Submit -> Apply config.<br>
![Screenshot from 2025-04-17 15-46-44](https://github.com/user-attachments/assets/8ca32a08-a810-45cf-945a-f4ce14ccc655)<br>
![Screenshot from 2025-04-17 15-47-05](https://github.com/user-attachments/assets/421a4bab-1231-45e8-b24c-f10f598495f0)<br>

* **Setup Inbound Routes**. Click Connectivity -> Inbound Routes -> Add Inbound Routes. Enter Description, DID Number(any number), Set Destination (select IVR). Then Submit -> Apply Config.<br>
![Screenshot from 2025-04-17 15-53-24](https://github.com/user-attachments/assets/f519403a-890f-4053-9765-1ad7bd63bb71)<br>

* **Setup Outbound Routes**. Click Connectivity -> Outbound Routes -> Add Outbound Routes. Enter Route Name, Route CID(Sat a number to call for IVR), Route Password, Optional Destination on Congestion(Select Inbound Routes which is connect with IVR). Then Submit -> Apply Config.<br>

### Now if you call that route CID(Outbound Routes) your IVR will play. 










  
    
