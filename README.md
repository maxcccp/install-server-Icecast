# Setup server Icecast :)

![Minion](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f7/Icecast_logo_large_2004.svg/934px-Icecast_logo_large_2004.svg.png)

- [Icecast ](https://icecast.org/) - это сервер потокового мультимедиа, который в настоящее время поддерживает аудиопотоки Ogg Vorbis и MP3. Его можно использовать для создания интернет-радиостанции или частного музыкального автомата, а также для многих других вещей. Он очень универсален, поскольку относительно легко добавляются новые форматы и поддерживает открытые стандарты для связи и взаимодействия.
- [Icecast-Docs ](https://www.icecast.org/docs/icecast-trunk/) - Узнать подробней о сервере можно в официальной документации.

- [Ezstream ](https://icecast.org/ezstream/) -  это исходный клиент командной строки для серверов потокового мультимедиа Icecast. Он начинался как преемник старой утилиты «shout» и с тех пор получил множество полезных функций. Бесплатное программное обеспечение и распространяется под Стандартной общественной лицензией GNU.

***

### Update the APT package list:
```
$ sudo apt-get update
```

### Install Icecast:
```
$ sudo apt-get install icecast2
```


### Copy file configure Icecast for case error:
```
$ cp /etc/icecast2/icecast.xml /etc/icecast2/icecast_dump.xml
```

### Configure Icecast:
```
$ nano /etc/icecast2/icecast.xml
```

### Change field count clients:
```
 <limits>
        <clients>100</clients>
        <sources>2</sources>
        <queue-size>524288</queue-size>
        <client-timeout>30</client-timeout>
        <header-timeout>15</header-timeout>
        <source-timeout>10</source-timeout>
        ....
  </limits>
```

### Change fields:
 - <source-password>hackme</source-password>
 - <relay-password>hackme</relay-password>
 - <admin-password>hackme</admin-password>
```
<authentication>
        <!-- Sources log in with username 'source' -->
        <source-password>hackme</source-password>
        <!-- Relays log in with username 'relay' -->
        <relay-password>hackme</relay-password>

        <!-- Admin logs in with the username given below -->
        <admin-user>admin</admin-user>
        <admin-password>hackme</admin-password>
</authentication>
```

### File defaults change field(ENABLE=true):
```
$ nano /etc/default/icecast2 
```

### To start the Icecast Server:
```
$ sudo systemctl start icecast2
$ /etc/init.d/icecast2 start
```
### To restart and reload configuration changes:
```
$ sudo systemctl restart icecast2
```
### To stop Icecast:
```
$ sudo systemctl stop icecast2
```

***

### Install Ezstream client:
```
$ sudo apt-get install ezstream
```
### Make folder ezstream and create on current folder file  config ezstream.xml:
```
$ cd /etc/
$ mkdir ezstream
$ touch ezstream.xml
```
### Add data in to file ezstream.xml:
```
<ezstream>
	<url>http://localhost:8000/play</url>
	<sourcepassword>пароль source в конфиге icecast</sourcepassword>
	<format>MP3</format>
	<filename>/полный/адрес/до/плейлиста</filename>
	<shuffle>0</shuffle>
	<playlist_program>0</playlist_program>
	<svrinfoname>Name</svrinfoname>
	<svrinfourl>http://radiocms.ru/</svrinfourl>
	<svrinfogenre>Ofther</svrinfogenre>
	<svrinfodescription>Description</svrinfodescription>
	<svrinfobitrate>128</svrinfobitrate>
	<svrinfochannels>2</svrinfochannels>
	<svrinfosamplerate>44100</svrinfosamplerate>
	<svrinfopublic>1</svrinfopublic>
</ezstream>
```

***

### Install codec LAME:
[LAME](https://lame.sourceforge.io/) - это высококачественный кодировщик MPEG Audio Layer III (MP3), имеющий лицензию LGPL.

```
$ sudo apt-get install lame
```

### Add data in to file  ezstream.xml about codec-lame(to the end of the file):
```
<reencode>
<enable>1</enable>
<encdec>
<format>MP3</format>
<match>.mp3</match>
<decode>lame -f --preset cbr 128 --bitwidth 16 "@T@" -</decode>
</encdec>
</reencode>
```
- Exemple file ezstream.xml: 

```
<ezstream>
	<url>http://localhost:8000/play</url>
	<sourcepassword>пароль source в конфиге icecast</sourcepassword>
	<format>MP3</format>
	<filename>/полный/адрес/до/плейлиста</filename>
	<shuffle>0</shuffle>
	<playlist_program>0</playlist_program>
	<svrinfoname>Name</svrinfoname>
	<svrinfourl>http://radiocms.ru/</svrinfourl>
	<svrinfogenre>Ofther</svrinfogenre>
	<svrinfodescription>Description</svrinfodescription>
	<svrinfobitrate>128</svrinfobitrate>
	<svrinfochannels>2</svrinfochannels>
	<svrinfosamplerate>44100</svrinfosamplerate>
	<svrinfopublic>1</svrinfopublic>

	<reencode>
		<enable>1</enable>
		<encdec>
			<format>MP3</format>
			<match>.mp3</match>
			<decode>lame -f --preset cbr 128 --bitwidth 16 "@T@" -</decode>
		</encdec>
	</reencode>
</ezstream>
```

### Create folder name multimedia files and add here file *.mp3:
- Exemple:
```
$ mkdir /var/multimedia
```
### Add an entry to the playlist.txt :
::: warning 
File with your tracks, specify the full path to the file:
- Exemple entry playlist.txt: /var/multimedia/music.mp3
::: 

### For Set encoding for mount point insert into shared code section in file icecast.xml
```
<icecast>
.... text....
<mount>
	<mount-name>/live</mount-name>
	<charset>UTF-8</charset>
	<fallback-mount>/play</fallback-mount>
	<fallback-override>1</fallback-override>
	<fallback-when-full>1</fallback-when-full>
</mount>

<mount>
	<mount-name>/play</mount-name>
	<charset>UTF-8</charset>
</mount>
.... text....
</icecast>
``` 

***
## This guide does not suggest doing so!
***






