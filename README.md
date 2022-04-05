# Bitnami-Moodle-Overview

### Moodle TM tarafindan guclendirilen Bitnami LMS Nedir?

Moodle LMS acik kaynakli online ogrenme sistemi olup, universitelerde, okullarda ve kurumlarda geniz bir kullanim yelpazesine sahiptir. Moduler yapisi ve yeni kosullara adapte olabilme ozelligi online egitimlerde on plana cikmasini sagliyor.

[Overview of Bitnami LMS powered by Moodle™ LMS](https://moodle.org/)

Docker Pull komutu: ```$ docker pull bitnami/moodle```


```
$ curl -sSL https://raw.githubusercontent.com/bitnami/bitnami-docker-moodle/master/docker-compose.yml > docker-compose.yml

$ docker-compose up -d
```

Yukarida belirtilen kod dizini, yazilim gelistirme ortami icin hizli kurulum icin tasarlanmistir. Guvenlik oldugu dusunulmeyen varsayilan bilgiler degistirilebilir ve daha guvenli bir urun gelistirme icin konfigurasyon opsiyonlari degistirilebilir.

### Nicin Bitnami Imajlarini kullanmaliyiz?

* Bitnami sistem girisindeki kaynaklari yakindan takip eder ve imajin yeni versiyonlarini derhal yayinlar.  
* Bitnami imajlari ile en sonki hatalar tamir edilir ve mumkun olan en kisa sure icerisinde bu ozelligini gosterir.
* Bitnami konteynerlari, sanal makineler ve bulut imajlari ayni bilesenleri ve konfigurasyon yaklasimini kullanir. Bu durum bize, projemizin ihtiyaclari dogrultusunda cikan formatlar arasi degisimlerde kolaylik saglar.
* Butun imajlar [minideb](https://github.com/bitnami/minideb) dedigimiz minimalist Debian'i temel alan konteynera dayanir ve bu durum bizlere Linux bazli sistemlere olan asinaligi saglar.
* Butun Bitnami imajlari DockerHub'da mevcut olup, [Docker Content Trust (DCT) ](https://docs.docker.com/engine/security/trust/)tarafindan imzalanmistir. Imajin butunlugunu dogrulatmak icin ```DOCKER_CONTENT_TRUST=1 ``` kullanabiliriz.
* Bitnami konteyner imajlari gunluk olarak paketin son surumu ile servis edilir.


[CVE scan report](https://quay.io/repository/bitnami/moodle?tab=tags) acik olan butun CVE'leri, guvenlik raporlarini icinde barindirir. Aksiyona tabii tutulan guvenlik problemleri listesine erisebilmek icin, 'latest' en sonki etiket bulunur, 'Security scan' altindaki guvenlik acigi linkine tiklanir, ve 'Only show fixable' filtresi isaretlenerek erisilebilir.

### Moodle Kubernetes icerisinde nasil ayaklandirilir?

Bitnami uygulamarini Helm Charts olarak ayaklandirmak bu uygulama ile baslayabilmek icin kolay yollardan biridir. [Bitnami Chart for Moodle Github](https://github.com/bitnami/charts/tree/master/bitnami/moodle) reposu kurulumu icinde barindirir.

```
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install my-release bitnami/moodle
```
Bitnami konteynerlari [Kubeapps](https://kubeapps.com/) ile uygulama gelistirmek ve yonetmek icin de kullanilabilir.

DockerFile linkine erismek icin [3-ol-7, 3.4.6-ol-7-r40
3-debian-9, 3.4.6-debian-9-r24, 3, 3.4.6, 3.4.6-r24, latest](https://github.com/bitnami/bitnami-docker-moodle/blob/3.11.6-debian-10-r16/3/debian-10/Dockerfile) kullanilabilir.

## Imaji elde etme

Moodle icin tavsiye edilen Bitnami Docker imajini elde etme yontemi [DockerHub Registry](https://hub.docker.com/r/bitnami/moodle) 'den ``` pull request``` etmektir.

```
$ docker pull bitnami/moodle:latest
```

Istege bagli olarak, spesifik bir versiyona da TAG kullanilarak erisilebilir. Mevcut olan [versiyonlar](https://hub.docker.com/r/bitnami/moodle/tags/).

```
$ docker pull bitnami/moodle:[TAG]
```

Eger istenilirse, imaji kendimiz de olusturabiliriz:

```
$ docker build -t bitnami/moodle:latest 'https://github.com/bitnami/bitnami-docker-moodle.git#master:3/debian-10'
```

### Imaj Nasil Kullanilir?

Oncelikli olarak bilinmesi gereken, Moodle erisim icin MySQL veya MariaDB veritabanlarina gereksinim duyar. Bu yazi dizininde veritabani gereksinimi icin [MariaDB](https://github.com/bitnami/bitnami-docker-mariadb) kullanacagiz.

### Uygulamayi Docker Compose Kullanarak Calistirma

Repository'nin ana klasoru fonksiyonel [docker-compose.yml](https://github.com/bitnami/bitnami-docker-moodle/blob/master/docker-compose.yml) dosyasini icinde barindirir. Asagidaki komut ile calistirilabilir:

```
$ curl -sSL https://raw.githubusercontent.com/bitnami/bitnami-docker-moodle/master/docker-compose.yml > docker-compose.yml

$ docker-compose up -d
```

### Docker Komut Satirini Kullanma

Uygulama ```docker-compose``` yerine manual olarak calistirilmak istenirse, asagida belirtilen adimlar takip edilmelidir:

***1. Adim: Network olusturmak***

```
$ docker network create moodle-network
```

***2. Adim: MariaDB surekliligi ve konteyneri icin volume olusturmak***

```
$ docker volume create --name mariadb_data

$ docker run -d --name mariadb \
  --env ALLOW_EMPTY_PASSWORD=yes \
  --env MARIADB_USER=bn_moodle \
  --env MARIADB_PASSWORD=bitnami \
  --env MARIADB_DATABASE=bitnami_moodle \
  --network moodle-network \
  --volume mariadb_data:/bitnami/mariadb \
  bitnami/mariadb:latest
```

***3. Adim: Moodle surekliligi ve konteyner erisimi icin volume olusturmak***

```
$ docker volume create --name moodle_data

$ docker run -d --name moodle \
  -p 8080:8080 -p 8443:8443 \
  --env ALLOW_EMPTY_PASSWORD=yes \
  --env MOODLE_DATABASE_USER=bn_moodle \
  --env MOODLE_DATABASE_PASSWORD=bitnami \
  --env MOODLE_DATABASE_NAME=bitnami_moodle \
  --network moodle-network \
  --volume moodle_data:/bitnami/moodle \
  bitnami/moodle:latest
```
Bu islemler tamamlandiktan sonra uygulamaya erisim ``` http://kendi-ip-adresinden/``` erisilebilir.

### Uygulamanin Surekliligi

Eger konteyneri silersek, butun veriler silinecektir ve daha sonra imajimizi tekrar calistirdigimizda veritabani yeniden baslatilacaktir. Veri kaybini onlemek adina, disk bolumu (volume) eklentisi yapmaliyiz, bu sayede, konteyner silinse dahi sureklilik saglanmis olacaktir.

Surekliligin saglanmasi icin, depolama bolumu icin dosyaya ait yeri ``` /bitnami/moodle``` olarak eklemeliyiz. Eger belirtilmis dosya yeri bos ise, disk ilk calismada sifirlanacaktir. Buna ek olarak, MariaDB veritabani sureklilik icin olusturmamiz gerekiyor.
[https://github.com/bitnami/bitnami-docker-mariadb#persisting-your-database](https://github.com/bitnami/bitnami-docker-mariadb#persisting-your-database).

Yukaridaki ornekler Docker'in disk bolumunu ***mariadb_data*** ve ***moodle_data*** olarak tanimliyor. Disk bolumu silinmedigi, kaldirilmadigi surece Moodle TM uygulamasi surekliligini koruyacaktir.

Yanlislikla disk bolumunun silinmesini onlemek adina, host dizinini veri disk bolumu olarak monteleyebiliriz.

### Docker Compose ile Host Dizinini Veri Disk Bolumu Olarak Deklare Etmek

Bu durumu saglamak icin ```docker-compose.yml``` dosyasinda kucuk bir degisiklik yapmak gerekiyor:

```
mariadb:
  ...
  volumes:
-      - 'mariadb_data:/bitnami/mariadb'
+      - /path/to/mariadb-persistence:/bitnami/mariadb
...
moodle:
  ...
  volumes:
-      - 'moodle_data:/bitnami/moodle'
+      - /path/to/moodle-persistence:/bitnami/moodle
...
-volumes:
-  mariadb_data:
-    driver: local
-  moodle_data:
-    driver: local
```

### Docker Komutlari ile Host Dizinini Veri Disk Bolumu Olarak Deklare Etmek  

**1. Adim: Network olusturmak**

```
$ docker network create moodle-network
```

**2. Adim: MariaDB konteynerini host disk bolumu ile olusturmak**

```
$ docker run -d --name mariadb \
  --env ALLOW_EMPTY_PASSWORD=yes \
  --env MARIADB_USER=bn_moodle \
  --env MARIADB_PASSWORD=bitnami \
  --env MARIADB_DATABASE=bitnami_moodle \
  --network moodle-network \
  --volume /path/to/mariadb-persistence:/bitnami/mariadb \
  bitnami/mariadb:latest
```

**3. Adim: Moodle konteynerini host disk bolumu ile olusturmak**

```
$ docker run -d --name moodle \
  -p 8080:8080 -p 8443:8443 \
  --env ALLOW_EMPTY_PASSWORD=yes \
  --env MOODLE_DATABASE_USER=bn_moodle \
  --env MOODLE_DATABASE_PASSWORD=bitnami \
  --env MOODLE_DATABASE_NAME=bitnami_moodle \
  --network moodle-network \
  --volume /path/to/moodle-persistence:/bitnami/moodle \
  bitnami/moodle:latest
```

### Konfigurasyon

#### Cevre Degiskeni

Moodle imajini calistirdiginda, docker-compose dosyasi ile veya ``` docker run``` komutu ile bir veya birden fazla cevre degiskeninin konfigurasyonu ayarlanabilir. Eger yeni bir cevre degiskeni atanacaksa;

* Docker-compose dosyasi icin, eklenecek degerin ismi ve degiskeni ```docker-compose.yml``` dosyanisa eklenmelidir.

```
moodle:
  ...
  environment:
    - MOODLE_PASSWORD=my_password
  ...
  ```

* Manual olarak eklenecek olursa, ```--env``` opsiyonu her bir degisken ve deger ile kullanilmalidir.

```
$ docker run -d --name moodle -p 80:8080 -p 443:8443 \
 --env MOODLE_PASSWORD=my_password \
 --network moodle-tier \
 --volume /path/to/moodle-persistence:/bitnami \
 bitnami/moodle:latest
 ```

 **Kullanilmasi uygun olan cevre degiskenleri:**

 ***Kullanici ve site konfigurasyonu***

 * ``` MOODLE_USERNAME```: Moodle uygulamasi kullanici adidir. Default: **user**
 * ``` MOODLE_PASSWORD```: Moodle uygulamasi sifresidir. Default: **bitnami**
 * ``` MOODLE_EMAIL```: Moodle uygulamasi emailidir. Default: user@example.com
 * ``` MOODLE_SITE_NAME```: Moodle site ismidir. Default: **New Site**
 * ``` MOODLE_SKIP_BOOTSTRAP```: Moodle veritabanini ayaklandirmak icin sifirlamamak gerekir. Veritabaninda daha onceden deklare edilmis Moodle verileri bulunabilir. Default: **no**
 * ``` MOODLE_HOST```: Moodle'in wwwroot ozelligini konfigure etmesine izin verir. Orneklendirmek gerekirse; example.com. Default olarak PHP superglobal degiskenidir. Default: **$_SERVER['HTTP_HOST']**
 * ``` MOODLE_REVERSEPROXY``` : Moodle'in reverseproxy ozelligini aktive etmesine izin verir. Default: **no**
 * ``` MOODLE_SSLPROXY```: Moodle'in sslproxy ozelligini aktive etmesine izin verir. Default: **no**


 ***Var olan veritabaninin kullanimi***

 * ``` MOODLE_DATABASE_TYPE```: Veritabani turudur. Gecerli olan degerler; mariadb, mysqli, pgsql. Default: **mariadb**
 * ``` MOODLE_DATABASE_HOST```: Veritabani server'i icin host adidir. Default: **mariadb**
 * ``` MOODLE_DATABASE_PORT_NUMBER```: Veritabani server'i tarafindan kullanilan port'tur. Default: **3306**
 * ``` MOODLE_DATABASE_NAME```: Moodle'in veritabanina baglanmak icin kullandigi veritabani ismidir. Default: **bitnami_moodle**
 * ``` MOODLE_DATABASE_USER```: Moodle'in veritabanina baglanmak icin kullandigi kullanici ismidir. Default: **bn_moodle**
 * ``` MOODLE_DATABASE_PASSWORD```: Moodle'in veritabanina baglanmak icin kullandigi sifredir. Default yok.
 * ``` ALLOW_EMPTY_PASSWORD```: Bos sifre olarak kullanilmasina izin verir. Default: **no**


***mysql-client kullanarak Moodle veritabani olusturmak***

* ``` MYSQL_CLIENT_FLAVOR```: SQL veritabani aromasi denebilir. Gecerli olan degerler; ```mariadb``` veya ```mysql```. Default: **mariadb**
* ``` MYSQL_CLIENT_DATABASE_HOST```: MariaDB server'i icin host ismidir. Default: **mariadb**
* ``` MYSQL_CLIENT_DATABASE_PORT_NUMBER```: MariaDB server'i tarafindan kullanilan port'tur. Default: **3306**
* ``` MYSQL_CLIENT_DATABASE_ROOT_USER```: Veritabani admin kullanicisi. Default: **root**   
* ``` MYSQL_CLIENT_DATABASE_ROOT_PASSWORD```: Veritabani admin kullanicisi icin veritabani sifresi. Default yok.
* ``` MYSQL_CLIENT_CREATE_DATABASE_NAME```: Mysql client tarafindan olusturulan yeni veritabani. Default yok.
* ``` MYSQL_CLIENT_CREATE_DATABASE_USER```: Mysql client tarafindan olusturulan yeni veritabani kullanicisi. Default yok.
* ``` MYSQL_CLIENT_CREATE_DATABASE_PASSWORD```: ``` MYSQL_CLIENT_CREATE_DATABASE_USER``` icin veritabani sifresi. Default yok.
* ``` MYSQL_CLIENT_CREATE_DATABASE_CHARACTER_SET```: Yeni veritabani icin olusturulan karakterler. Default yok.
* ``` MYSQL_CLIENT_CREATE_DATABASE_COLLATE```: Yeni veritabani icin kullanilan veritabani karsilastirmasi ve harmanlanmasi. Default yok.
* ``` MYSQL_CLIENT_CREATE_DATABASE_PRIVILEGES```: ```MYSQL_CLIENT_CREATE_DATABASE_USER``` tarafindan ```MYSQL_CLIENT_CREATE_DATABASE_NAME``` icerisinde spesifik olarak deklare edilen veritabani ayricaliklari.
* ``` MYSQL_CLIENT_ENABLE_SSL_WRAPPER```: **mysql** tarafindan SSL baglantilarinin veritabanina mecbur edilmesi. API yerine CLI kullanan uygulamalar icin faydalidir. Default: **no**  
* ``` MYSQL_CLIENT_ENABLE_SSL```: Veritabani icin SSL baglantilarinin mecbur edilmesi. Default: **no**
* ``` MYSQL_CLIENT_SSL_CA_FILE```: Yeni veritabani icin SSL CA dosyasina giden dizin. Default yok.
* ``` MYSQL_CLIENT_SSL_CERT_FILE```: Yeni veritabani icin SSL CA dosyasina giden dizin. Default yok.
* ``` MYSQL_CLIENT_SSL_KEY_FILE```: Yeni veritabani icin SSL CA dosyasina giden dizin. Default yok.
* ``` ALLOW_EMPTY_PASSWORD```: Bos sifre olarak kullanilmasina izin verir. Default: **no**


***postgresql-client kullanarak Moodle veritabani olusturmak***

* ``` POSTGRESQL_CLIENT_DATABASE_HOST```: PostgreSQL server'i icin host ismidir. Default: **postgresql**
* ``` POSTGRESQL_CLIENT_DATABASE_PORT_NUMBER```: PostgreSQL server'i tarafindan kullanilan port'tur. Default: **5432**
* ``` POSTGRESQL_CLIENT_POSTGRES_USER```: Veritabani admin kullanicisi. Default: **root**
* ``` POSTGRESQL_CLIENT_POSTGRES_PASSWORD```: Veritabani admin kullanicisi icin veritabani sifresi. Default yok.
* ``` POSTGRESQL_CLIENT_CREATE_DATABASE_NAMES```: Postgresql-client tarafindan olusturulan yeni veritabani listesi. Default yok.
* ``` POSTGRESQL_CLIENT_CREATE_DATABASE_USER```: Postgresql-client tarafindan olusturulan yeni veritabani kullanicisi. Default yok.
* ``` POSTGRESQL_CLIENT_CREATE_DATABASE_PASSWORD```: ```POSTGRESQL_CLIENT_CREATE_DATABASE_USER``` kullanicisi icin veritabani sifresi. Default yok.
* ``` POSTGRESQL_CLIENT_CREATE_DATABASE_EXTENSIONS```: Ilk baslatma sirasinda spesifik olarak belirtilen PostgreSQL uzantilari. Default yok.
* ``` POSTGRESQL_CLIENT_EXECUTE_SQL```: SQL kodunu PostgreSQL serveri icinde calistirir. Default yok.
* ``` ALLOW_EMPTY_PASSWORD```: Bos sifre olarak kullanilmasina izin verir. Default: **no**


***SMTP Konfigurasyonu***

Asagidaki cevre degiskenleri ayarlanirsa SMTP kullanarak email gonderme Moodle Konfigurasyonu saglanir:

* ``` MOODLE_SMTP_HOST```: SMTP host'u.
* ``` MOODLE_SMTP_PORT```: SMTP port'u.
* ``` MOODLE_SMTP_USER```: SMTP hesabi kullanicisi.
* ``` MOODLE_SMTP_PASSWORD```: SMTP hesabi sifresi.
* ``` MOODLE_SMTP_PROTOCOL```: SMTP protokolu.


***PHP Konfigurasyonu***

* ``` PHP_ENABLE_OPCACHE```: PHP script'leri icin OPcache'yi devreye sokar, gecerli kilar. Default yok.
* ``` PHP_EXPOSE_PHP```: PHP versiyonu ile HTTP header'i devreye sokar, gecerli kilar. Default yok.
* ``` PHP_MAX_EXECUTION_TIME```: PHP script'leri icin max calisma zamani. Default yok.
* ``` PHP_MAX_INPUT_TIME```: PHP script'leri icin max girdi zamani. Default yok.
* ``` PHP_MAX_INPUT_VARS```: PHP script'leri icin max girdi degiskenlerinin miktari. Default yok.
* ``` PHP_MEMORY_LIMIT```: PHP script'leri icin bellek limiti. Default: **256M**
* ``` PHP_POST_MAX_SIZE```: PHP POST istekleri icin max boyut.
* ``` PHP_UPLOAD_MAX_FILESIZE```: PHP yuklemeleri icin max dosya boyutu. Default yok.

***Ornekler***

Asagida belirtilen ornek, Gmail hesabi kullanildigi durumdaki SMTP konfigurasyonunu temsil eder.

* ```docker-compose.yml``` dosyasi modifiye edilmeli:

```
moodle:
  ...
  environment:
    - MOODLE_DATABASE_USER=bn_moodle
    - MOODLE_DATABASE_NAME=bitnami_moodle
    - ALLOW_EMPTY_PASSWORD=yes
    - MOODLE_SMTP_HOST=smtp.gmail.com
    - MOODLE_SMTP_PORT=587
    - MOODLE_SMTP_USER=your_email@gmail.com
    - MOODLE_SMTP_PASSWORD=your_password
    - MOODLE_SMTP_PROTOCOL=tls
...
```

* Manuel calistirma icin:

```
$ docker run -d --name moodle -p 80:8080 -p 443:8443 \
  --env MOODLE_DATABASE_USER=bn_moodle \
  --env MOODLE_DATABASE_NAME=bitnami_moodle \
  --env MOODLE_SMTP_HOST=smtp.gmail.com \
  --env MOODLE_SMTP_PORT=587 \
  --env MOODLE_SMTP_USER=your_email@gmail.com \
  --env MOODLE_SMTP_PASSWORD=your_password \
  --env MOODLE_SMTP_PROTOCOL=tls \
  --network moodle-tier \
  --volume /path/to/moodle-persistence:/bitnami \
  bitnami/moodle:latest
```

Asagida belirtilen bu ornek ise, NGINX load balancer esliginde uretilir:

* ```docker-compose.yml``` dosyasi modifiye edilmeli:

```
moodle:
  ...
  environment:
    - MOODLE_HOST=example.com
    - MOODLE_REVERSEPROXY=true
    - MOODLE_SSLPROXY=true
...
```

* Manuel calistirma icin:

```
$ docker run -d --name moodle -p 80:8080 -p 443:8443 \
  --env MOODLE_HOST=example.com \
  --env MOODLE_REVERSEPROXY=true \
  --env MOODLE_SSLPROXY=true \
  --network moodle-tier \
  --volume /path/to/moodle-persistence:/bitnami \
  bitnami/moodle:latest
```

#### Dil Paketlerinin Eklenmesi

Default olarak Moodle'da dil secenegi Ingilizce olarak gelmektedir. Bununla birlikte, daha fazla dil paketi opsiyonlari da sunmaktadir. Admin arayuzundedeki platform araciligi ile dil paketleri konfigure edilebilir. Yeni eklenecek olan dil paketininden tam verim almak icin, sistem guncellemesi akilli bir yontem olacaktir, bunu sistemin yerel dosyalarinda yapabiliriz. Bu dil paketi eklemesini yapmak icin bir kac opsiyonumuz var:

**Default gelen imaj'in ```EXTRA_LOCALES``` "build-time" degiskeni ile olusturulmasi**

Docker imajini olusturdugumuzda ```EXTRA_LOCALES``` build-time degiskeni kullanilarak eklenti yapabiliriz. Degerler yazilirken virgul ile veya noktali virgul ile ayristirilmali ve girisi girisi konteyner icinde ```/usr/share/i18n/SUPPORTED``` tanimlanmalidir.

Ornek olarak, asagida Fransizca, Almanca, Italyanca, Ispanyolca ve Turkce dillerini tanimlayacagiz.

```
fr_FR.UTF-8 UTF-8, de_DE.UTF-8 UTF-8, it_IT.UTF-8 UTF-8, es_ES.UTF-8, tr_TR.UTF-8 UTF-8
```

**EXTRA_LOCALES** kullanmak icin iki opsiyomuz var:

* ```docker-compose.yml``` dosyasi modifiye edilmeli:

```
moodle:
...
  # image: 'bitnami/moodle:3' # remove this line !
  build:
    context: .
    dockerfile: Dockerfile
    args:
      - EXTRA_LOCALES=fr_FR.UTF-8 UTF-8, de_DE.UTF-8 UTF-8, it_IT.UTF-8 UTF-8, es_ES.UTF-8 UTF-8
...
```

* Manuel calistirma icin, repository klonlanmali ve asagida belirtilen komut ```3/debian-10``` icine yazilmali:

```
$ docker build -t bitnami/moodle:latest --build-arg EXTRA_LOCALES="fr_FR.UTF-8 UTF-8, de_DE.UTF-8 UTF-8, it_IT.UTF-8 UTF-8, es_ES.UTF-8 UTF-8" .
```

**Yerel tanimlayicilara ```WITH_ALL_LOCALES``` ile olanak saglamak**

Cevre degiskenini ```WITH_ALL_LOCALES=yes``` olarak duzenleyebiliriz. Biraz zaman alacaktir ancak desteklenen tum paketler yuklenecektir.

Iki farkli opsiyon ile ```WITH_ALL_LOCALES=yes``` kullanabiliriz:

* ```docker-compose.yml``` dosyasi modifiye edilmeli:

```
moodle:
...
  # image: 'bitnami/moodle:3' # remove this line !
  build:
    context: .
    dockerfile: Dockerfile
    args:
      - WITH_ALL_LOCALES=yes
...

```

* Manuel calistirma icin, repository klonlanmali ve asagida belirtilen komut ```3/debian-10``` icine yazilmali:

```
$ docker build -t bitnami/moodle:latest --build-arg WITH_ALL_LOCALES=yes .
```

**Default Imaj'i Genisletmek**

Son olarak, default gelen imaj'imizi genisletebilir ve bir cok yerel tanimlayici ekleyebiliriz:

```
OM bitnami/moodle
RUN echo "es_ES.UTF-8 UTF-8" >> /etc/locale.gen && locale-gen
```
#### Log'lar, Sistem Gunlukleri

Moodle icin Bitnami Docker imaj'i konteyner log'larini ```stdout``` ile gonderir. log'lari gormek icin;

```
$ docker logs moodle
```

Docker compose da kullanilabilir;

```
$ docker-compose logs moodle
```

Konteynerlarin log suruculerini de ``` --log-driver``` kullanarak konfigure edebiliriz. Aklimizda bulunmasi adina, default konfigurasyon icin Docker ```json-file``` surucusunu kullaniyor.
