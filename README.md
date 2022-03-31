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

*** 1. Adim: Network olusturmak***

```
$ docker network create moodle-network
```

*** 2. Adim: MariaDB surekliligi ve konteyneri icin volume olusturmak***

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

*** 3. Adim: Moodle surekliligi ve konteyner erisimi icin volume olusturmak***

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
