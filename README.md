# SC-Notif-Vocher-Wifi

Untuk melihat Chat ID ada dua cara yaitu dengan menggunakan perintah getUpdates dan menggunakan bantuan bot @get_id_bot.
https://api.telegram.org/bot(Token Anda)/getUpdates

Cara yang kedua anda bisa mencari di menu pencarian @get_id_bot lalu klik tombol star maka bot tersebut akan mengirimkan pesan berupa chat id anda

Untuk mengecek Bot mengirim pesan anda dapat menggunakan alamat dibawah ini di browser
https://api.telegram.org/bot(token anda)/sendMessage?chat_id=(chat id anda)&text=Ping Hotspot

Paste script di mikrotik, yaitu di menu
IP -> Hospot -> User Profiles -> ( Pilih Profile yang akan di tambah script )

## SC - ON Login
```bash
Script Via Telegram
:local mac $"mac-address";
:set mac [:ip dhcp-server lease get [:ip dhcp-server lease find mac-address="$mac"] host];
:local nama "$user";
:local ips [/ip hotspot active get [find user="$nama"] address];
:local exp [/ip hotspot user get [find name="$nama"] comment];
:local profile [/ip hotspot user get [find name="$nama"] profile];
:local datetime "$[/system clock get date] $[/system clock get time]";
:local mac [/ip hotspot active get [find user="$nama"] mac-address];
:local host [/ip dhcp-server lease get [find address="$ips"] host-name];
:local lby [/ip hotspot active get [find user="$nama"] login-by];
:local limit [/ip hotspot active get [find user="$nama"] limit-bytes-total];
:local totq [(($limit)/1048576)];
:local useraktif [/ip hotspot active print count-only];
:tool fetch url="https://api.telegram.org/bot<API TOKEN BOT TELE>/sendMessage?chat_id=<CHAT ID>&text===>>INFO LOGIN<<==%0A- Kode Voucher : $nama%0A- IP Address : $ips %0A- Mac Address : $mac%0A- Menggunakan : $host%0A- Metode Login : $lby%0A- Kuota : $totq Mb%0A- Expired Voucher : $exp%0A- Waktu Login : $datetime%0A- Paket : $profile%0A- User Online : $useraktif user" mode=http keep-result=no;
```
<br>

## SC - ON LOG-OUT
```bash
:tool fetch url="https://api.telegram.org/bot<API TOKEN BOT TELE>/sendMessage?chat_id=<CHAT ID>&text=<<==INFO LOGOUT==>>%0A- Kode Voucher : $user%0A- IP Address : $address" keep-result=no;
```
