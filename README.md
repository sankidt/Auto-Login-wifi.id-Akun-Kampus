# Auto Login @wifi.id Akun Kampus
Auto Login @wifi.id pakai akun kampus

# Usage
- Scheduler
```
/system scheduler
add interval=2m name=Auto_Login on-event="/ping 8.8.8.8 count=10; :if ([/ping \
    8.8.8.8 count=10] = 0) do={ /system script run ganti_mac_work; :delay 10s;\
    \_/system script run autologin_kampus_work; }" policy=\
    ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon \
    start-time=startup
```
- Ganti Mac Otomatis
```
/system script
add dont-require-permissions=yes name=ganti_mac_work owner=msn00 policy=\
    ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon source="#\
    \_Menghasilkan 2 digit acak untuk MAC address\r\
    \n:local generate ([/certificate scep-server otp generate minutes-valid=0 \
    as-value]->\"password\");\r\
    \n:local interface \"wlan2\"\r\
    \n:local currentMac [/interface wireless get [find name=\$interface] mac-a\
    ddress]\r\
    \n:log warning (\"MAC Address: \$interface Lama: \$currentMac\");\r\
    \n/interface disable \$interface\r\
    \n:delay 2s;\r\
    \n\r\
    \n# Memisahkan MAC address saat ini menjadi komponen-komponen\r\
    \n:local mac1 [:pick \$currentMac 0 2]\r\
    \n:local mac2 [:pick \$currentMac 3 5]\r\
    \n:local mac3 [:pick \$currentMac 6 8]\r\
    \n:local mac4 [:pick \$currentMac 9 11]\r\
    \n:local mac5 [:pick \$currentMac 12 14]\r\
    \n:local mac6 [:pick \$generate 10 12]\r\
    \n\r\
    \n# Membentuk MAC address baru dengan 2 digit terakhir yang diubah\r\
    \n:local newMac \"\$mac1:\$mac2:\$mac3:\$mac4:\$mac5:\$mac6\"\r\
    \n:log warning (\"Hasil Generate MAC Address : \$newMac\");\r\
    \n:delay 5s;\r\
    \n\r\
    \n# Mengatur MAC address baru ke interface\r\
    \n/interface wireless set \$interface mac-address=\$newMac\r\
    \n:log warning (\"MAC Address \$interface Berhasil di Ganti Menjadi \$newM\
    ac\");\r\
    \n/interface enable \$interface\r\
    \n:delay 5s;"
```
- Script Auto Login
```
/system script
add dont-require-permissions=yes name=autologin_kampus_work owner=msn00 \
    policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon \
    source="# AUTOLOGIN KAMPUS\r\
    \n# TINGGAL GANTI NAMA INTERFACE, EMAIL, DAN PASSWORD KAMPUS\r\
    \n:local WAN \"wlan2\"\r\
    \n:local email \"username akun kampus\"\r\
    \n:local pass \"password akun kampus\"\r\
    \n\r\
    \n:local ipclient [/ip dhcp-client get [find interface=\$WAN] address]\r\
    \n:local ip [:put [:pick \$ipclient 0 [:find \$ipclient \"/\"]]]\r\
    \n:local mac [:put [/interface get [/interface find name=\$WAN] mac-addres\
    s ]]\r\
    \n\r\
    \n/tool fetch mode=https http-header-field=\"Content-Type: application/x-w\
    ww-form-urlencoded; charset=UTF-8, User-Agent: Firefox, Referer: https://w\
    elcome2.wifi.id/login/\?gw_id=WAG-D7-PNK&client_mac=\$mac&wlan=GTDPK00024-\
    N/TLK-CI-17849:@wifi.id\" http-method=post http-data=\"username=\$email@c\
    om.unsiq&password=\$pass&landURL=\" url=\"https://welcome2.wifi.id/authnew\
    /login/check_login.php\?ipc=\$ip&gw_id=WAG-D7-PNK&mac=\$mac&redirect=&wlan\
    =GTISM00065-N/TLK-CI-105823:@wifi.id\" dst-path=wifi.id.txt;\r\
    \n:local lognya [/file get wifi.id.txt contents];\r\
    \n/log warning \$lognya;"
```
# Full Tutorial
https://youtu.be/JCmZtD8wcOo
