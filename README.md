| Nama    | NRP     | 
| ------- | ------- | 
| Imam Nurhadi    | 50272210     | 
| Jojo     | 5027221062     |


## SOAL 12
Karena pusat ingin sebuah website yang ingin digunakan untuk memantau kondisi markas lainnya maka deploy lah webiste ini (cek resource yg lb) pada severny menggunakan apache

Melakukan configurasi terhadap file apache default pada direktori ```cd /etc/apache2/sites-available/000-default.conf``` dan memberikan konfigurasinya pada port 8080 sebagai berikut: 

```shell
<VirtualHost *:8080>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/it04

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
Setelah itu memindahkan file default ke it04.conf dengan ```cp 000-default.conf it04.conf``` dan menjalankan beberapa command 
```
service apache2 reload
service apache2 restart
service apache2 status #untuk mencek apakah sudah running
```
dan mengatur index.php pada direktori /var/wwww/it04/index.php sesuai pada ketentuan soal 
```
<?php
$hostname = gethostname();
$date = date('Y-m-d H:i:s');
$php_version = phpversion();
$username = get_current_user();



echo "Hello World!<br>";
echo "Saya adalah: $username<br>";
echo "Saat ini berada di: $hostname<br>";
echo "Versi PHP yang saya gunakan: $php_version<br>";
echo "Tanggal saat ini: $date<br>";
?>
```

lalu dapat kita test dengan menjalankan ```lynx http://localhost:8080```
dan hasilnya sebagai sebagai berikut 

#### FOTO
![lynxlocalhostxxxx.jpg](FOTO/lynxlocalhostxxxx.jpg)

## Soal 13
Tapi pusat merasa tidak puas dengan performanya karena traffic yag tinggi maka pusat meminta kita memasang load balancer pada web nya, dengan Severny, Stalber, Lipovka sebagai worker dan Mylta sebagai Load Balancer menggunakan apache sebagai web server nya dan load balancernya

Mencoba melakukan konfigurasi load balancing pada ```nano /etc/apache2/sites-available/000-default.conf``` sebagai berikut 

```shell
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.


    <Proxy balancer://itbalancer>
        BalancerMember http://192.168.2.2:8080
        BalancerMember http://192.168.2.3:8080
        BalancerMember http://192.168.2.4:8080
        ProxySet lbmethod=byrequests
    </Proxy>

    ProxyPreserveHost On
    ProxyPass / balancer://itbalancer/
    ProxyPassReverse / balancer://itbalancer/


        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        # Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
ServerName 127.0.1.1.
```

setelah itu menerapkan 
```
a2ensite 000-default.conf
service apache2 reload
service apache2 restart
```
lalu kita jalankan ```lynx http://192.168.2.5/index.php```

### FOTO
### FOTO

## Soal 16
### FOTO lynx
### FOTO Konfigurasi
