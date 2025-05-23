![Project Status](https://img.shields.io/badge/Status-Ongoing-blue)
![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Technologies](https://img.shields.io/badge/Technologies-Suricata%2C%20Nmap%2C%20QEMU%2FKVM%2C%20Linux%20Mint%2C%20Kali%20Linux-red)
![Cybersecurity Project](https://img.shields.io/badge/Category-Cybersecurity%20Project-informational)

# 🛡️ **AttackDetector** 🛡️
👀 ⚔️ Suricata İncelemesi ve Saldırı Simülasyonu ⚔️ 👀

<table>
  <tr>
    <td align="center">
      <b>Makineler Arası Haberleşme Kontrolü</b><br>
      <a href="images/iletisimkontrol.gif" target="_blank">
        <img src="images/iletisimkontrol.gif" alt="İletişim Kontrolü GIF" width="280">
      </a>
    </td>
    <td align="center">
      <b>Suricata Servis Başlatma Durumu</b><br>
      <a href="images/Servis%20Başlatması.png" target="_blank">
        <img src="images/Servis%20Başlatması.png" alt="Servis Başlatma Görseli" width="280">
      </a>
    </td>
  </tr>
  <tr>
    <td align="center">
      <b>ICMP (Ping) Kural Testi</b><br>
      <a href="images/test-2.gif" target="_blank">
        <img src="images/test-2.gif" alt="ICMP Test GIF" width="280">
      </a>
    </td>
    <td align="center">
      <b>Nmap SYN Tarama Tespiti</b><br>
      <a href="images/test-3.gif" target="_blank">
        <img src="images/test-3.gif" alt="Nmap Tespit GIF" width="280">
      </a>
    </td>
  </tr>
</table>


## **Kurulum ve Çalıştırma**

Bu projeyi yerel ortamınızda kurmak ve çalıştırmak için aşağıdaki adımları takip edebilirsiniz.

### **Ön Gereksinimler**

* **Sanal Makine Yazılımı:** QEMU/KVM, VirtualBox veya VMware Workstation (Bu proje QEMU/KVM ile geliştirilmiştir)
* **İşletim Sistemleri ISO'ları:**
    * Linux Mint (Tercihen XFCE sürümü)
    * Kali Linux
* 8GB RAM ve 4 CPU çekirdeğine sahip bir ana makine üzerinde denendi, tercih sizindir.

### **Adım Adım Kurulum**

1.  **Sanal Makineleri Kurun:**
    * QEMU/KVM (veya tercih ettiğiniz sanallaştırma yazılımı) üzerinde **Mint Linux** ve **Kali Linux** sanal makinelerini kurun.
    * Aşağıdaki [3.1. Sanal Makine Konfigürasyonu](#31-sanal-makine-konfigürasyonu) bölümündeki kaynak atamalarına ve IP adreslerine uygun şekilde yapılandırın.
    * Her iki sanal makinenin de [NAT ağ bağdaştırıcısına](#32-ağ-topolojisi) sahip olduğundan ve birbirleriyle iletişim kurabildiğinden emin olun.

2.  **Bağımlılıkları Yükleyin (Mint Linux üzerinde):**

    ```bash
    sudo apt update && sudo apt upgrade
    sudo apt install suricata
    ```

3.  **Suricata Temel Yapılandırması:**

    * `suricata.yaml` dosyasını açın: `sudo mousepad /etc/suricata/suricata.yaml`
    * `vars:` bölümündeki `HOME_NET` değerini kendi Mint Linux IP bloğunuza göre düzenleyin. Örneğin: `HOME_NET: "[192.168.122.0/24]"`
    * `rule-files:` bölümündeki varsayılan Suricata kurallarını (örneğin `suricata.rules`) yorum satırına alın ve `local.rules` dosyasını ekleyin:
        ```yaml
        # - suricata.rules
        - local.rules
        ```
    * `default-rule-path:` ayarının `rules` klasörüne işaret ettiğinden emin olun (örn: `/etc/suricata/rules`).

4.  **Suricata Servisini Başlatın ve Kontrol Edin:**

    ```bash
    sudo systemctl enable suricata
    sudo systemctl start suricata
    sudo systemctl status suricata
    sudo suricata -T -c /etc/suricata/suricata.yaml -i enp1s0 # Kendi arayüzünüzü kullanın
    ```

5.  **Özel Kuralları Ekleyin:**

    * `/etc/suricata/rules/` dizini altına `local.rules` adında bir dosya oluşturun.
    * Projede kullanılan ICMP ve Nmap SYN kurallarını bu dosyaya ekleyin. (Örnekler aşağıdaki kapsamlı raporumuzdan alınabilir.)

    ```bash
    echo 'alert icmp any any -> $HOME_NET any (msg:"PROJE TEST: ICMP Echo Request Tespit Edildi"; sid:2000001; rev:1;)' | sudo tee -a /etc/suricata/rules/local.rules
    echo 'alert tcp any any -> $HOME_NET any (msg:"PROJE TEST2: Nmap SYN Tarama Tespit Edildi"; flags:S; sid:2000002; rev:1;)' | sudo tee -a /etc/suricata/rules/local.rules
    ```

6.  **Saldırı Senaryolarını Çalıştırın:**

    * Kali Linux makinesinden hedef Mint Linux makinesine Nmap taramaları ve ping istekleri göndererek Suricata'nın tespitlerini gözlemleyin.
    * Logları takip etmek için: `sudo tail -f /var/log/suricata/fast.log`

## 🌟 **Katkıda Bulunma** 🌟

Bu projeyi daha da geliştirmek için katkılarınızı bekliyoruz! Her türlü geri bildirim, hata düzeltmesi veya yeni özellik önerisi değerlidir. Katkıda bulunmak isterseniz:

1.  Bu depoyu (repository) Fork'layın.
2.  Yeni bir özellik veya hata düzeltmesi için dal (branch) oluşturun: `git checkout -b feature/your-feature-name`
3.  Değişikliklerinizi yapın ve commit edin.
4.  Değişikliklerinizi kendi deponuza (fork'unuza) push edin.
5.  Bir Pull Request (PR) açın ve değişikliklerinizi açıklayın.

> [!CAUTION]
> **LÜTFEN KURAL EKLEME KATKISINDA BULUNMAK İÇİN YARDIMCI ŞABLONLAR KLASÖRÜNE GİDİN VE [BU ŞABLONU](/Yardımcı%20Şablonlar/YENİ%20KURAL%20EKLEME%20ŞABLONU.md) KULLANIN** ‼️‼️ 


AŞAĞIDAKİ RAPORDA DAHA DETAYLI BİR İNCELEME MEVCUTTUR.

### **İçindekiler**
- [Giriş](#1-gi̇ri̇ş)
	- [Projenin Amacı](#11-projenin-amacı)
	- [Projenin Önemi](#12-projenin-önemi)
- [Arka Plan ve Temel Kavramlar](#2-arkaplan-ve-temel-kavramlar)
	- [Saldırı Tespit Sistemleri (IDS) ve Saldırı Önleme Sistemleri (IPS) Nedir?](#21-saldırı-tespit-sistemleri-ids-ve-saldırı-önleme-sistemleri-ips-nedir)
	- [Snort Mimarisi](#22-snort-mimarisi)
	- [Suricata Mimarisi](#23-suricata-mimarisi)
	- [Saldırı Senaryoları ve Nmap](#24-saldırı-senaryoları-ve-nmap)
- [Proje Ortamı ve Kullanılan Araçlar](#3-proje-ortamı-ve-kullanılan-araçlar)
	- [Sanal Makine Konfigürasyonu](#31-sanal-makine-konfigürasyonu)
		- [Mint Linux (Hedef/IDS Makinesi)](#311-mint-linux-hedefids-makinesi)
		- [Kali Linux (Saldırgan Makinesi)](#312-kali-linux-saldırgan-makinesi)
	- [Ağ Topolojisi](#32-ağ-topolojisi)
	- [Kullanılan Yazılımlar ve Versiyonları](#33-kullanılan-yazılımlar-ve-versiyonları)
- [Gerçekleştirilen Çalışmalar ve Uygulamalı Adımlar](#4-gerçekleştirilen-çalışmalar-ve-uygulamalı-adımlar)
	- [Makinelerin haberleşme kontrolü](#40-makinelerin-haberleşme-kontrolü)
	- [Suricata Kurulumu ve Temel Konfigürasyon](#41-suricata-kurulumu-ve-temel-konfigürasyon)
		- [Kurulum Süreci](#411-kurulum-süreci)
		- [`suricata.yaml` Dosyası Ayarları (Özellikle `$HOME_NET` tanımı)](#412-suricatayaml-dosyası-ayarları)
		- [Servis Yönetimi (Başlatma, Durdurma, Durum Kontrolü)](#413-servis-yönetimi-başlatma-durdurma-durum-kontrolü)
	- [Özel Kural Geliştirme ve Testler](#42-özel-kural-geliştirme-ve-testler)
		- [ICMP (Ping) Tespit Kuralı](#421-icmp-ping-tespit-kuralı)
			- [Kuralın Tanımı](#4211-kuralın-tanımı)
			- [Uygulama ve Test Süreci](#4212-uygulama-ve-test-süreci)
			- [Elde Edilen Log Çıktıları ve Yorumları](#4213-elde-edilen-log-çıktıları-ve-yorumları)
		- [Nmap SYN Port Tarama Tespit Kuralı](#422-nmap-syn-port-tarama-tespit-kuralı)
			- [Kuralın Tanımı](#422-nmap-syn-port-tarama-tespit-kuralı)
			- [Saldırı Senaryosu (Kali'den Nmap Taraması)](#4222-saldırı-senaryosu-kaliden-nmap-taraması)
			- [Uygulama ve Test Süreci](#4223-uygulama-ve-test-süreci)
			- [Elde Edilen Log Çıktıları ve Yorumları](#4224-elde-edilen-log-çıktıları-ve-yorumları)
- [Log Analizi ve Yorumlama](#5-log-analizi-ve-yorumlama)
	- [`fast.log` Analizi](#51-fastlog-analizi)
- [Sonuç ve Proje Kazanımları](#6-sonuç-ve-proje-kazanımları)
	- [Projeden Elde Edilen Temel Bilgi ve Beceriler](#61-projeden-elde-edilen-temel-bilgi-ve-beceriler)
	- [Projenin Başarıları](#62-projenin-başarıları)
	- [Uygulamalı Öğrenmenin Önemi](#63-uygulamalı-öğrenmenin-önemi)
- [Gelecek Çalışmalar ve Geliştirme Önerileri](#gelecek-çalışmalar-ve-geliştirme-önerileri)
	- [Daha Gelişmiş Kural Senaryoları (HTTP, DNS, İç Ağ Hareketleri)](#71-daha-gelişmiş-kural-senaryoları-http-dns-i̇ç-ağ-hareketleri)
	- [IPS (Engelleme) Modu Araştırması](#72-ips-engelleme-modu-araştırması)

---
# 1. GİRİŞ

Siber güvenlik dünyasında bireylere ve kuruluşlara gerçekleştirilen tehditleri tabiri caizse 'avlamak' için, başlıca savunma mekanizmalarımız haline gelen Saldırı Tespit Sistemlerini incelediğimiz bu raporda, IDS-IPS mantığını, Suricata ve Snort yazılımlarını incelemeye çalıştık. Bir çok konuda araştırma yaptık ve Temel Seviye de 'eğitim odaklı' bir yaklaşımla yaptığımız araştırmaların hepsini bir rapor haline getirdik. Bilgileri olabildiğince sindirilebilir ve öz vermeye çalıştık. Keyifli okumalar.

## 1.1 Projenin Amacı

- Suricata ve Snort mimarilerini inceleyerek, arka planda nasıl çalıştıklarını anlamak
- Uygulamalı bir yaklaşım ile IDS, IPS mantığını derinlemesine öğrenmek
- Sanal bir laboratuvar oluşturup, aynı ağda bulunan iki bilgisayarın 'kurban', 'hacker' yaklaşımıyla simüle edilmesi
- ...
## 1.2 Projenin Önemi

Siber saldırıların karmaşıklığı göze alındığında IDS, IPS savunma mekanizmalarının güvenlik stratejilerinde ayrılmaz bir parça olduğunu söylemeye gerek yoktur sanırım.

- Tehdit avcılığı; saldırıların ve şüpheli ağ faaliyetlerinin nasıl tespit edileceğine dair iç görüler sunar.
- Kural geliştirme yetenekleri; özel güvenlik ihtiyaçlarına yönelik özelleştirilmiş kurallar yazabilme becerisi kazandırır.
- Hata ayıklama ve problem çözme; güvenlik araçları ile çalışırken karşılaşılan teknik zorlukları (Bu projede karşılaştığım hata sayısı yaklaşık 15-20 adet, fakat her birini projeye eklemek zor.) aşma ve çözüme ulaşma yeteneğini geliştirir.
- Log Analizi; Üretilen güvenlik loglarından anlamlı bilgiler çıkarma ve olaylara müdahale için bu bilgileri kullanma pratiği sağlar.

Bu yönleri ile projenin incelenmesinin, öğrencilere ve siber güvenlik alanına ilgi duyan bireylere kritik düşünme ve pratik uygulama becerileri kazandırarak, bu alanda gelecekteki kariyerleri için sağlam bir temel oluşturacağını düşünüyorum.

# 2. ARKAPLAN VE TEMEL KAVRAMLAR

İşin arkaplanına bakmamızın sebebi şu: Bilgiyi öğrenmek; google ya da çeşitli AI platformlarına sorup okumak kadar basit değil. Bu yüzden uygulamaya geçmeden önce bilmemiz gereken şeyleri kavramlara ayırarak, kafamızda bir zihin haritasına dönüştürmemiz gerekiyor ki, işin arka planında sistemin nasıl ilerlediğini bilelim. Bu yüzden dökümantasyon klasörümüze konular hakkında bilgiler toparladım. 

Projenin daha sıkıcı ve daha yoğun (ki gayet yoğun) olmaması için Snort kısmının uygulamasını projeden çıkardık. Projede  IDS ve IPS'i daha iyi anlamak için Snort üzerinde temel kavramları öğreneceğiz. Snort'un temel kavramlarını öğrendikten ve anladıktan sonra Suricata'nın sadece mimarisine baktığımızda, zaten kafamızda bir çok şey oturacaktır.

Bu kısımda bilmemiz gerekenler sırasıyla;

## 2.1. Saldırı Tespit Sistemleri (IDS) ve Saldırı Önleme Sistemleri (IPS) Nedir?

[Konu hakkında detaylı bilgi için buraya tıklayın.](https://github.com/xenntzodium/AttackDetector/blob/main/D%C3%B6k%C3%BCmantasyon/IDS%20ve%20IPS%20Nedir%3F.md)

## 2.2. Snort Mimarisi

[Konu hakkında detaylı bilgi için buraya tıklayın.](https://github.com/xenntzodium/AttackDetector/blob/main/D%C3%B6k%C3%BCmantasyon/Snort%20Mimarisi.md)

## 2.3. Suricata Mimarisi

DİKKAT: Suricata mimarisini aşağıdaki canvas üzerinde daha detaylı incelemek için, dosyayı indirip obsidian üzerinden görüntüleyebilirsiniz. 

![Suricata-Canvas](suricata-canvas.png)

[Dosyayı şu sayfadan sağda bulunan indirme kısmından indirebilirsiniz.](https://github.com/xenntzodium/AttackDetector/blob/main/D%C3%B6k%C3%BCmantasyon/Suricata%20Mimarisi.canvas)

Suricata'yı güçlü kılan bazı temel özellikler şunlardır:

- **Çoklu Çekirdek Desteği (Multi-threading):** Geleneksel IDS'lerin aksine, Suricata çoklu CPU çekirdeğinden faydalanarak yüksek performanslı ağ ortamlarında bile verimli çalışabilir.
- **Protokol Tanımlama:** HTTP, TLS/SSL, DNS gibi birçok uygulama katmanı protokolünü otomatik olarak tanır ve bu protokoller içindeki tehditleri tespit edebilir.
- **Dosya Çıkarma (File Extraction):** HTTP veya SMTP trafiği üzerinden geçen şüpheli dosyaları otomatik olarak çıkarabilir ve disk üzerine kaydedebilir. Bu, zararlı yazılım analizinde önemli bir özelliktir.
- **Modüler Mimari:** Eklentiler ve yeni özellikler için modüler bir yapıya sahiptir.
- **Çoklu Log Formatları:** Tespit edilen olayları `fast.log` (insan okunabilir özet) ve `eve.json` (makine tarafından işlenebilir detaylı JSON) gibi farklı formatlarda loglayabilir.
- **IPS Yeteneği:** Hem IDS (sadece tespit) hem de IPS (tespit ve engelleme) modunda çalışabilir.

## 2.4. Saldırı Senaryoları ve Nmap

[Konu hakkında detaylı bilgi için buraya tıklayın.](https://github.com/xenntzodium/AttackDetector/blob/main/D%C3%B6k%C3%BCmantasyon/Sald%C4%B1r%C4%B1%20Senaryolar%C4%B1%20-%20NMAP.md)

NOT: Bu bölümde açıklanan kavramlar ve araçlar, projemizin ilerleyen adımlarında Suricata üzerinde geliştirilen kuralların ve gerçekleştirilen saldırı simülasyonlarının temelini oluşturmuştur. Tabiki konular bunlarla sınırlı değil, daha öğrenmemiz gereken çok şey var. Fakat şimdilik yukarıdaki bilgilerin kafamızda bir şeyler canlandırması yeterli. 

---

# 3. Proje Ortamı ve Kullanılan Araçlar

Projenin uygulanması ve test edilmesi için bir sanal ortam oluşturuldu. 
## 3.1. Sanal Makine Konfigürasyonu

Proje, **QEMU/KVM (Kernel-based Virtual Machine)** sanallaştırma platformu üzerinde iki ayrı sanal makine kullanılarak gerçekleştirilmiştir. QEMU/KVM, Linux çekirdeğinin sanallaştırma yeteneklerinden faydalanan güçlü ve performansa dayalı bir sanallaştırma çözümüdür. Bu makineler, projenin güvenlik senaryolarını simüle etmek üzere belirli rollerle yapılandırılmıştır:

Sanal Makine Yöneticisi: QEMU/KVM
- Sanal Bilgisayar 1: Kali Linux -> Saldırgan
- Sanal Bilgisayar 2: Mint -> Hedef (Kurban)

### 3.1.1. Mint Linux (Hedef/IDS Makinesi)

**Rol:** IDS'i Mint Linux üzerine kuracağız. Makinenin diğer bir rolü ise saldırıların hedefi olması. Kısacası kurban bilgisayar.

- **İşletim Sistemi:** Linux Mint 
- **IP Adresi:** 192.168.122.165

Ağ Bağdaştırıcısı Ayarı:

- **Tip:** NAT (Network Address Translation)
- **Amaç:** Kurduğumuz iki sistemin birbiri ile haberleşebilmesini sağlamak.

**Kaynak Ataması:** 

- RAM: 3072 MB
- CPU: 2

Kurulu yazılımlar: 

- **Suricata**
- **jq (JSON log analizi için)**

### **3.1.2. Kali Linux (Saldırgan Makinesi)**

**Rol:** Saldırıyı gerçekleştireceğimiz saldırgan bilgisayar.

- **İşletim Sistemi:** Kali Linux
- **IP Adresi:** 192.168.122.153

 **Ağ Bağdaştırıcısı Ayarı:**
 
- **Tip:** NAT (Network Address Translation)
- Amaç: Kurduğumuz iki sistemin birbiri ile haberleşebilmesini sağlamak.

**Kaynak Ataması:**

 - RAM: 2048 MB
 - CPU: 2

**Kurulu Yazılımlar:**

- Nmap (Ağ tarama ve keşif aracı)

## 3.2. Ağ Topolojisi 

![Ağ%20Topolojisi.png](/images/Ağ%20Topolojisi.png)

## 3.3. Kullanılan Yazılımlar ve Versiyonları

 **Sanal Makine Platformu:**

- **QEMU/KVM:** 10.0.0
- **libvirt:** 11.3.0
- **virt-manager:** 5.0.0

**İşletim Sistemleri:**

- **Linux Mint:** 22.1
- **Kali Linux:** 2025.1

**Ağ Saldırı Tespit Sistemi (IDS):**

- **Suricata:** **7.0.10 RELEASE**

**Ağ Keşif Aracı**

- **Nmap:** 7.95

# 4. Gerçekleştirilen Çalışmalar ve Uygulamalı Adımlar 

Bu bolümde, projenin hedeflerine ulaşmak için adım adım gerçekleştirilen teknik çalışmalar ve uygulamalı adımlar detaylandırıldı. Suricata'nın kurulumundan özel kural geliştirme ve saldırı simülasyonlarına kadar olan süreç, karşılaşılan zorluklar ve elde edilen öğrenimlerle birlikte sunulmaktadır.

Suricata IDS'in proje ortamına entegrasyonu, sistemin temel yapılandırmasıyla başlamıştır.

## 4.0. Makinelerin haberleşme kontrolü

Önce `ip a` komutu ile hangi ip bloğunu kullanacağımızı öğrenmeliyiz. Bunu öğrenirken bir de iki bilgisayarın haberleşip haberleşemediğini test edelim.

- Kali makinemizin terminaline ve mint makinemizin terminaline `ip a` komutunu girerek ip adreslerini öğrenebiliriz.
- Kali makinemizin ip adresi: 192.168.122.153 
- Mint: 192.168.122.165
- Şimdi Kali makinemizin terminaline `ping 192.168.122.165` yazıp mint makinemize ping gönderebiliyor muyuz test edelim.
- Ardından aynı şekilde mint makinemizin terminaline `ping 192.168.122.153` yazarak testimizi yapalım. Sonuçlar aşağıdaki gibi olmalıdır.

![iletisimkontrol.gif](/images/iletisimkontrol.gif)
## 4.1. Suricata Kurulumu ve Temel Konfigürasyon

Suricata IDS'in proje ortamına entegrasyonu, sistemin temel yapılandırmasıyla başlamıştır.

### 4.1.1. Kurulum Süreci

Mint Linux (Hedef/IDS makinesi) üzerinde Suricata'nın kurulumu, dağıtımın paket yöneticisi aracılığıyla gerçekleştirilmiştir. Kurulumun ardından, Suricata servisinin başlatılması ve otomatik başlatma ayarları yapılmıştır.

```bash
sudo apt update && sudo apt upgrade # güncelleme
```

```bash
sudo apt install suricata # Suricata kurulumu
```

```bash
sudo systemctl enable suricata # Suricata'nın her sistem açılışında otomatik olarak başlamasını sağlar
sudo systemctl start suricata # Suricatayı aktif eder, şu an başlatır.
sudo systemctl status suricata # Servis durum kontrolü
```

Çıktı aşağıdaki gibi olmalıdır.

![Servis%20Başlatması.png](/images/Servis%20Başlatması.png)

###### NOT!

Bazı durumlar da aşağıdaki gibi `sudo systemctl enable suricata` `suricata.service is not a native service, redirecting to systemd-sysv-install.` şeklinde bir çıktı verdiğini görürüz. 

![executingsystemd.png](/images/executingsystemd.png)

Hata değildir, yalnızca `systemctl enable suricata` komutunun `/usr/lib/systemd/systemd-sysv-install enable suricata` komutuna yönlendirildiğini söylüyor. Suricata bazı dağıtımlarda (genelde debian sistemlerinde) native `systemd` servisi olarak değil eski yöntem olan `/etc/init.d/suricata` yoluyla çalışıyor. Yani aslında enable komutu başarılı olmuştur.

Böyle bir durumda `sudo systemctl status suricata` komutu bize enable döndürmeyecektir. 

![enablepr.png](/images/enablepr.png)

`enable` durumundan şüphe ederseniz şu komutu çalıştırarak durumu kontrol edebilirsiniz: `ls /etc/rc*.d | grep suricata`

![kssuricata.png](/images/kssuricata.png)

K -> Kill S -> Start (shutdown ya da reboot anında servis durur onun haricinde çalışır. Türkçesi bu.) Yani enable olmuş. Güzel.
### 4.1.2. `suricata.yaml` Dosyası Ayarları

`suricata.yaml` -> Suricata'nın konfigürasyon dosyası. Burada ağ ortamımıza göre özel ayarlamalar yapabiliyoruz. En kritik düzenlemelerden biri, ağ değişkenlerini tanımlamak.

Önce `ip a` komutu ile hangi ip bloğunu kullanacağımızı öğrenmeliyiz. Bunu öğrenirken bir de iki bilgisayarın haberleşip haberleşemediğini test edelim.

- Kali makinemizin terminaline ve mint makinemizin terminaline `ip a` komutunu girerek ip adreslerini öğrenebiliriz.
- Kali makinemizin ip adresi: 192.168.122.153 
- Mint: 192.168.122.165
- Şimdi Kali makinemizin terminaline `ping 192.168.122.165` yazıp mint makinemize ping gönderebiliyor muyuz test edelim.
- Ardından aynı şekilde mint makinemizin terminaline `ping 192.168.122.153` yazarak testimizi yapalım. Sonuçlar aşağıdaki gibi olmalıdır.

![iletisimkontrol](/images/iletisimkontrol.gif)

Dosyayı açtığımız da ilk olarak Suricata'nın takip edeceği IP bloğunu Suricata'ya belirtiyoruz.

```yaml
vars:
	address-groups:
		HOME_NET: "[192.168.122.0/24]" # doldurduğumuz alan
		EXTERNAL_NET: "any" # burası varsayılan olarak bu şekilde kalabilir.
		# EXTERNAL_NET değişkeni kurum dışı IP adreslerini temsil eder.
		# 0.0.0.0 - 255.255.255.255 -> Yani tüm ip adresleri
		# Biz temel düzeyde gittiğimiz için burada bir değişiklik yapmadık.
```
###### NOT!
**İzlenecek Arayüz:** Projemizde `enp1s0` arayüzü seçilmiştir. `ip a` komutunu yazarak kendi arayüzünüzü öğrenebilirsiniz.

### 4.1.3. Servis Yönetimi (Başlatma, Durdurma, Durum Kontrolü)

Servisi tekrar başlatma, başlatma, durdurma, kontrol etme ve suricata.yaml konfigürasyonlarında bir hata olup olmadığını görmek için iki grup altında 5 adet kod bilmeliyiz.

Sistem kodları;

```bash 
sudo systemctl start suricata # servis başlatma
sudo systemctl restart suricata # yeniden başlatma
sudo systemctl stop suricata # servis durdurma
sudo systemctl status suricata # kontrol
```

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -i enp1s0

# -T -> Test modu
# -c -> kullanılacak olan yapılandırma dosyasını belirtiyoruz.
# -i -> kullandığımız arayüzü belirtiyoruz.
```


NOT!

Bu komutun çıktısı, konfigürasyon dosyasında genel bir hata olmadığını göstermiştir. Ancak, kural dosyalarındaki daha derin sözdizimi hatalarının bu aşamada her zaman açıkça belirtilmediği, ileri aşamalardaki deneyimlerle anlaşılmıştır.

## 4.2. Özel Kural Geliştirme ve Testler

Projemizin en önemli adımlarından biri, belirli saldırı senaryolarını tespit etmek üzere Suricata için özel kurallar (signatures) yazmak ve bu kuralları canlı ağ trafiği üzerinde test etmektir. Bu süreç, Suricata'nın kural dili (`Suricata Rule Language - SRL`) hakkında derinlemesine bilgi edinmemizi sağlamıştır.

Şimdi herşeyimiz hazır. Ortamı kurduk. Konfigürasyonları ayarladık. Servisleri ayarladık sıra geldi Suricata'nın amacını gerçekleştirmeye.

### 4.2.1. ICMP (Ping) Tespit Kuralı

KURAL TANIMI: İlk önce basit bir ping kuralı ile yazdığımız kuralların çalışıyor olup olmadığını test etmek, gelecekte yazdığımız kurallar için bir önayak olacaktı. Bu yüzden ilk önce basit bir ICMP kuralı yazarak, Kali makinesinden Mint makinemize atılan bir ping'i tespit etmeye çalıştık. 

![kurallar](/images/kurallar.png)

NOT: Bu kısımda aslında önce rules klasörü yoktu. Çünkü Suricata varsayılan kuralları kullanıyordu. Fakat biz kendi kurallarımızı yazacağımız için bir Rules klasörü oluşturduk. İçerisine `local.rules` adında bir dosya oluşturduk. Artık oluşturduğumuz kuralları buraya yazacağız.  

#### 4.2.1.1. Kuralın Tanımı
SYNTAX;

```plaintext
<action> <protocol> <src_ip> <src_port> <direction> <dst_ip> <dst_port> (<options>)
```

```plaintext
alert icmp any any -> $HOME_NET any (msg:"PROJE TEST: ICMP Echo Request Tespit Edildi"; sid:2000001; rev:1;)
```

- Suricata bu kural ile bir **alert (uyarı)** üretecek.
- **ICMP protokolü** üzerinden gelen paketleri inceleyecek. (Bu, genelde "ping" gibi işlemlerdir.)
- **Herhangi bir kaynak IP adresinden** ve **herhangi bir porttan** gelen ICMP trafiği için geçerlidir. (Yani dışarıdan gelen tüm ICMP trafiği)
- Hedef, **$HOME_NET** olarak tanımlanan ağ bloğundaki herhangi bir IP adresi olabilir. Yani bu kural, bizim iç ağımıza yönelik saldırı ya da taramaları gözlemler.
- **Port numarası önemli değildir**, çünkü ICMP protokolü bağlantısız çalışır (TCP/UDP gibi port kavramı yoktur).
- - `msg: "PROJE TEST: ICMP Echo Request Tespit Edildi"` → Eğer kural tetiklenirse loglara bu mesaj yazılır. ICMP Echo Request (ping isteği) tespit edildiğini bildirir.
- `itype:8` → Sadece **ICMP Type 8**, yani **Echo Request** paketlerini hedef alır. Bu tür paketler genellikle bir sistemin "canlı" olup olmadığını test etmek için gönderilir.
- `sid:2000001` → Bu kuralın **benzersiz kimliğidir**. Suricata her kurala bir "Signature ID" verir. 1.000.000 ve üzeri SID'ler kullanıcı tanımlı özel kurallar içindir.
- `rev:1` → Bu kuralın **ilk sürümüdür**. Kurala ileride bir güncelleme yapılırsa `rev` numarası artırılır (örneğin `rev:2` olur).

#### 4.2.1.2. Uygulama ve Test Süreci 

Şimdi gelelim uygulama kısmımıza. 

Mint makinemizin terminaline  `sudo suricata -T -c /etc/suricata/suricata.yaml -i enp1s0` kodunu yazdıktan sonra bir sorun olmadığına dair bir çıktı almamız gerekiyor.
ardından `sudo systemctl restart suricata` diyerek suricatayı tekrar başlatmamız gerekiyor.

![durumkontrolü.png](/images/durumkontrolü.png)

Öyleyse deneyelim.

Şuan da Mint'i tail moduna alıp daha sonra log dosyamızı dinlememiz gerekiyor. Ardından Kali'den ping atacağız ve bu atılan ping Mint log dosyamız da gözükecek.

![test-1.gif](/images/test-1.gif)

```bash
sudo tail -f /var/log/suricata/fast.log # Dosyayı dinliyoruz.
```

```bash
ping 192.168.122.165 # Kaliden Mint'e ping isteği
```

Yakaladık fakat bir sorun var. Suricata'nın kendi içerisinde barındırdığı kural setleri olduğu için bizim oluşturduğumuz kural seti çalışmadı. Aslında çalıştı, fakat varsayılan olarak Suricata kural setleri daha önde çalışmış oldu. Bu durumu düzeltmek için `/etc/suricata/suricata.yaml` dosyasına girelim ve CTRL+F ile rule-files kısmında bir düzenleme yapalım.

![rule-files düzeltmesi.png](/image/rule-files%20düzeltmesi.png)

Ne yaptık? `sudo mousepad /etc/suricata/suricata.yaml` ile suricata dosyasını açtık. CTRL+F ile rule-files kısmına gittik. 2187. satırda bulunan default-rule-path ve 2190. satırda bukunan - suricata.rules kısımlarını yorum satırına aldık. Dosyayı kaydedip, tekrar deneyelim.

Aşağıdaki komutları sırayla girelim.

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -i enp1s0
sudo systemctl restart suricata
sudo systemctl status suricata
```

Ardından dinlemeye alalım: `sudo tail -f /var/log/suricata/fast.log`

![test-2.gif](/images/test-2.gif)

Kuralımızın çalıştığını gördük. Fakat kural varsayılan kuraldan biraz farklı çalıştı. Sürekli her `icmp_req` isteğinde bize uyarı vermiyor varsayılan kuralda olduğu gibi. Bunun sebebi local.rules içerisine yazdığımız kuralın içerisine bu seçeneği vermememiz. O konulara girmiyorum ama kısaca;

```plaintext
<action> <protocol> <src_ip> <src_port> <direction> <dst_ip> <dst_port> (<options>)
```

options kısmına gireceğimiz farklı seçenekler ile bunu değiştirebiliriz. Biz sadece test ettiğimiz için bu uyarıyı bu şekilde almış olmamız yeterli. 

AÇIKLAMA

- İlk testlerde, sürekli ping atılmasına rağmen Suricata'nın her bir ping paketi için uyarı üretmediği, aksine belirli bir süre boyunca yalnızca ilk uyarıyı kaydettiği fark edilmiştir. Bu durum, Suricata'nın aşırı log şişkinliğini önlemek amacıyla varsayılan olarak aktif olan "uyarı bastırma" (alert suppression) mekanizmasından kaynaklanmaktadır. Bu davranışın anlaşılması, IDS'lerin sadece tehditleri algılamakla kalmayıp, aynı zamanda üretilen logların yönetilebilirliğini de göz önünde bulundurduğunu göstermiştir. Her pakette uyarı almak için `threshold` gibi anahtar kelimelerin kullanılması gerektiği, ancak bu konudaki ilk denemelerin kural sözdizimi hataları nedeniyle zorluklar çıkardığı gözlemlenmiştir. Bu süreç, Suricata kural dilinin hassas yapısını ve detaylara dikkat etmenin önemini kavramamızı sağlamıştır.

#### 4.2.1.3. Elde Edilen Log Çıktıları ve Yorumları 

```plaintext
05/22/2025-16:41:27.196216  [**] [1:2000001:1] PROJE TEST: ICMP Echo Request Tespit Edildi [**] [Classification: (null)] [Priority: 3] {ICMP} 192.168.122.165:0 -> 192.168.122.153:0

[TARİH-SAAT]  [**] [GENEL BİLGİ] MESAJ [**] [SINIFLANDIRMA] [ÖNCELİK] {PROTO} KAYNAK_IP:PORT -> HEDEF_IP:PORT

```

Sanırım bu kadar yeterli, yukarıdaki çıktıların altında belirttiğim syntax'ı incelerseniz kafanızda bir şeyler canlanacaktır.

--

Güzel. Şimdi testimizi bitirdik. Fakat kimse bir ping için bu kuralı görmek istemez, özel durumlar dışında. Bu yüzden bu sadece Suricata'nın çalışma mantığını anlamamız için bir örnekti. 

Şimdi bir saldırı senaryosu oluşturmamız gerek. 

### 4.2.2. Nmap SYN Port Tarama Tespit Kuralı

KURAL TANIMI VE AMACI: Basit şekilde anlatacak olursak, 100 bilgisayarlı bir şirkette çalıştığımızı varsayalım. Bir saldırgan ağımıza erişti. Ağımızda yapacağı ilk tehlikeli faaliyetlerden biri nmap sorgusu ile açık port taraması yapmaktır diyebiliriz. nmap ile daha neler yapılabileceğini [bu kısımda konuşmuştuk.](https://github.com/xenntzodium/AttackDetector/blob/main/D%C3%B6k%C3%BCmantasyon/Sald%C4%B1r%C4%B1%20Senaryolar%C4%B1%20-%20NMAP.md) Daha detaylı bilgi için nmap dökümantasyonlarına [buradan ulaşabilirsiniz.](https://nmap.org/docs.html)

Öyle ki saldırgan ağımıza eriştiğinde bizim hangi işletim sistemini kullandığımızı, hangi portların açık olduğunu öğrenmek için nmap sorgusu atacaktır. **Bu da bize gösterir ki bizim iç ağımızda nmap sorgusu atıldığında bizim haberimiz olması gerekir.**

Öyleyse kendimize haber verelim :).

SYNTAX;

```plaintext
<action> <protocol> <src_ip> <src_port> <direction> <dst_ip> <dst_port> (<options>)
```

```plaintext
alert tcp any any -> $HOME_NET any (msg:"PROJE TEST2: Nmap SYN Tarama Tespit Edildi"; flags:S; sid:2000002; rev:1;)
```

- Suricata bu kural ile bir **alert (uyarı)** üretecek.
- **TCP protokolü** üzerinden gelen paketleri inceleyecek. Bu, genelde bağlantı başlatma, tarama veya iletişim kurma amacı taşıyan trafiği ifade eder.
- **Herhangi bir kaynak IP adresinden** ve **herhangi bir porttan** gelen TCP trafiği için geçerlidir. Yani dışarıdan gelen tüm TCP trafiği taranır.
- Hedef, **$HOME_NET** olarak tanımlanan ağ bloğundaki herhangi bir IP adresi olabilir. Bu kural, iç ağa yönelik TCP SYN (bağlantı başlatma) denemelerini gözlemler.
- **Hedef port fark etmeksizin** geçerlidir. Herhangi bir servise yönelik olabilir.
- `msg: "PROJE TEST: Nmap SYN Tarama Tespit Edildi"` → Eğer kural tetiklenirse loglara bu mesaj yazılır. SYN bayrağı taşıyan TCP paketlerinin muhtemel bir Nmap taraması olabileceği varsayılır.
- `flags:S` → Sadece TCP **SYN** bayrağı ayarlanmış olan paketleri hedef alır. Bu bayrak, TCP bağlantı başlatma paketidir ve genellikle port taramaları sırasında kullanılır (özellikle Nmap SYN scan).
- `sid:2000002` → Bu kuralın **benzersiz kimliğidir**. Suricata her kurala bir "Signature ID" verir. 1.000.000 ve üzeri SID’ler kullanıcı tanımlı özel kurallar içindir.
- `rev:1` → Bu kuralın **ilk sürümüdür**. Kurala ileride bir güncelleme yapılırsa `rev` numarası artırılır (örneğin `rev:2` olur).

#### 4.2.2.2. Saldırı Senaryosu (Kali'den Nmap Taraması)

Saldırgan makinesi Kali Linux (IP: 192.168.122.153) üzerinden hedef makine Mint Linux (IP: 192.168.122.165) adresine yönelik bir SYN port taraması başlatılmıştır. 

```bash
sudo nmap -sS -p 1-1000 192.168.122.165

# -sS -> Yarı açık (Stealth) TCP SYN taraması yapar. Bu, Nmap’in en yaygın ve hızlı tarama yöntemidir. TCP üç yönlü el sıkışmanın ilk adımı olan SYN paketini gönderir ama bağlantıyı tamamlamaz.

# -p 1-1000 -> port aralığı
```

Bu komutu kali makinemiz de kullanacağız. Cebimizde kalsın.

#### 4.2.2.3. Uygulama ve Test Süreci 

Şimdi tekrardan bir kural tanımlamamız gerekiyor. Bunu terminale yapıştırıp direk işimizi kolaylaştıralım. 

```bash
echo 'alert tcp any any -> $HOME_NET any (msg:"PROJE TEST: Nmap SYN Tarama Tespit Edildi"; flags:S; sid:2000002; rev:1;)' | sudo tee -a /etc/suricata/rules/local.rules
```

Şimdi hızlıca sistemi test edelim ve sistemi yeniden başlatalım. 

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -i enp1s0 # test
sudo systemctl restart suricata
sudo systemctl status suricata
```

Eğer Suricata aktif olduysa ve test aşamasında bir hata ile karşılaşılmadıysa bir problem yok demektir.

```bash
sudo tail -f /var/log/suricata/fast.log # log dosyamızı dinlemeye aldık
```

```bash
# Kali terminaline
nmap -sS -p 1-1000 192.168.122.165
```

Aşağıdaki gibi bir sonuç beklenmektedir.

![test-3.gif](/images/test-3.gif)

#### 4.2.2.4. Elde Edilen Log Çıktıları ve Yorumları

```plaintext
05/22/2025-17:46:22.778336  [**] [1:2000002:1] PROJE TEST2: Nmap SYN Tarama Tespit Edildi [**] [Classification: (null)] [Priority: 3] {TCP} 192.168.122.153:54675 -> 192.168.122.165:771

[TARİH-SAAT]  [**] [GENEL BİLGİ] MESAJ [**] [SINIFLANDIRMA] [ÖNCELİK] {PROTOKOL} KAYNAK_IP:PORT -> HEDEF_IP:PORT

```

Sanırım bu kadar yeterli, yukarıdaki çıktıların altında belirttiğim syntax'ı incelerseniz kafanızda bir şeyler canlanacaktır.

# 5. Log Analizi ve Yorumlama

Saldırı Tespit Sistemlerinin (IDS) en temel işlevlerinden biri, algıladığı şüpheli veya kötü niyetli ağ faaliyetlerini loglamaktır. Bu loglar, güvenlik analistleri için bir olayın ne zaman, nerede ve nasıl gerçekleştiğine dair kritik bilgiler sunar. Suricata, bu logları farklı ihtiyaçlara yönelik çeşitli formatlarda üretmektedir. Bu bölümde, projemiz kapsamında elde edilen temel log formatları incelenmiş ve yorumlanmıştır.

## 5.1. `fast.log` Analizi

`fast.log` dosyası, Suricata tarafından üretilen ve insan tarafından hızlıca okunabilecek, özet bilgi sağlayan bir uyarı (alert) logudur. Bu format, bir güvenlik olayının temel detaylarını anında gözden geçirmek için idealdir. Projemizdeki Nmap SYN taraması sonrası `/var/log/suricata/fast.log` dosyasında aşağıdaki benzeri bir çıktı elde edilmiştir:

```plaintext
05/21/2025-18:48:03.024831 [**] [1:2000002:1] PROJE TEST: Nmap SYN Tarama Tespit Edildi [**] [Classification: (null)] [Priority: 3] {TCP} 192.168.122.153:xxxxx -> 192.168.122.165:yyyyy
```

- **`05/21/2025-18:48:03.024831`**: Olayın tam tarih ve zaman damgasıdır (AY/GÜN/YIL-Saat:Dakika:Saniye.Mikrosaniye). Bu bilgi, olayların kronolojisini ve zaman çizelgesini oluşturmak için kullanılır.
- **`[**]`**: Suricata uyarılarının başlangıcını ve sonunu işaretleyen görsel ayırıcılardır.
- **`[1:2000002:1]`**: Bu kısım `[GID:SID:REV]` formatındadır ve kuralın benzersiz tanımlayıcılarını belirtir:
    - **`1` (GID - Group ID):** Suricata'da varsayılan olarak kullanılan genel kural grubunun kimliğidir.
    - **`2000002` (SID - Signature ID):** Kurala verdiğimiz benzersiz imza kimliğidir. Bu sayede hangi spesifik kuralın tetiklendiği anlaşılır. Bizim durumumuzda bu, Nmap SYN taramasını tespit eden kuralımızdır.
    - **`1` (REV - Revision):** Kuralın revizyon numarasıdır. Kuralda yapılan her değişiklikte bu numara artırılır, bu da kural versiyon takibi için önemlidir.
- **`PROJE TEST: Nmap SYN Tarama Tespit Edildi (Basit)`**: Kuralın `msg` (mesaj) seçeneğinde tanımladığımız uyarı metnidir. Bu, olayın ne olduğunu özetleyen insan okunabilir bir açıklamadır.
- **`[Classification: (null)] [Priority: 3]`**:
    - **`Classification: (null)`**: Kuralımızda özel bir sınıflandırma belirtmediğimiz için varsayılan olarak `null` görünmektedir. Suricata'nın ticari kural setlerinde genellikle "Web Application Attack", "Attempted Admin Priv" gibi daha spesifik sınıflandırmalar bulunur.
    - **`Priority: 3`**: Kuralın öncelik seviyesidir. Genellikle 1 (en yüksek) ile 255 (en düşük) arasında bir değer alır. Bu, analistlerin hangi uyarıya önce odaklanması gerektiğini belirlemeye yardımcı olur.
- **`{TCP}`**: Tespit edilen olayın gerçekleştiği ağ protokolüdür. Bu durumda TCP trafiğidir.
- **`192.168.122.153:xxxxx -> 192.168.122.165:yyyyy`**: Olaydaki kaynak ve hedef IP adresleri ile portlarını gösterir.
    - **`192.168.122.153`**: Saldırgan makinemiz olan Kali Linux'un IP adresidir.
    - **`xxxxx`**: Kaynak port numarasıdır (dinamik olarak değişebilir).
    - **`->`**: Trafik yönünü belirtir (kaynaktan hedefe).
    - **`192.168.122.165`**: Hedef makinemiz olan Mint Linux'un IP adresidir.
    - **`yyyyy`**: Hedef port numarasıdır (Nmap taramasında taranan port).

Bu `fast.log` çıktısı, Kali Linux'tan Mint Linux'a yönelik bir Nmap SYN port taramasının Suricata tarafından başarılı bir şekilde algılandığını ve kuralımızla eşleştiğini net bir şekilde ortaya koymaktadır.

# 6. Sonuç ve Proje Kazanımları

Gerçekleştirilen bu Suricata IDS projesinde, ağ güvenliği ve saldırı tespiti konularında geniş bir yelpazede bilgi ve pratik beceriler kazandırmıştır:

## 6.1. Projeden Elde Edilen Temel Bilgi ve Beceriler

- **Ağ Saldırı Tespit Sistemleri (IDS) Kavramının Derinlemesine Anlaşılması:**
    
    - IDS'lerin ağ güvenliği mimarisindeki rolü ve önemi kavranmıştır.
    - IDS ile IPS arasındaki temel farklar ve entegrasyon potansiyeli öğrenilmiştir.
    
- **Suricata IDS Uzmanlığı:**
    
    - **Kurulum ve Konfigürasyon:** Linux ortamında Suricata'nın kurulumu, ana konfigürasyon dosyası (`suricata.yaml`) üzerinden ağ değişkenlerinin tanımlanması (`$HOME_NET`, `$EXTERNAL_NET`) ve servis yönetimi (`systemctl` komutları) konularında yetkinlik kazanılmıştır.
    - **Ağ Arayüzü Yönetimi:** Suricata'nın belirli ağ arayüzlerini dinlemesi için doğru yapılandırma becerisi edinilmiştir (`enp1s0` gibi).
    - **Kural Dosyası Yönetimi:** Kendi özel kural dosyalarının (`local.rules`) Suricata'ya nasıl entegre edileceği ve varsayılan kural setlerinin nasıl yönetileceği öğrenilmiştir.
    
- **Özel Kural (Signature) Geliştirme ve Uygulama:**
    
    - **Kural Dili (`SRL`) Hakimiyeti:** Suricata Kural Dili'nin (`SRL`) temel sözdizimi (`alert`, `protocol`, `source/destination`, `direction`, `rule options`) ve önemli anahtar kelimeler (`msg`, `flags`, `sid`, `rev`) detaylı olarak öğrenilmiş ve uygulanmıştır.
    - **Tehdit Odaklı Kural Yazımı:** ICMP ping ve Nmap SYN port taramaları gibi spesifik tehdit senaryolarını tespit etmek üzere özelleştirilmiş ve etkili kurallar yazma becerisi geliştirilmiştir. Bu, gerçek dünya güvenlik ihtiyaçlarına yönelik çözümler üretebilme yeteneğini göstermiştir.
    
- **Saldırı Simülasyonu ve Tespiti:**
    
    - **Sanal Laboratuvar Kurulumu:** QEMU/KVM üzerinde çok makineli bir sanal laboratuvar ortamı kurma ve ağ topolojisini (NAT ağı, IP adreslemeleri) yapılandırma becerisi edinilmiştir.
    - **Nmap Kullanımı:** Nmap gibi popüler bir ağ keşif aracını kullanarak farklı port tarama tekniklerini (`-sS` SYN taraması) uygulama ve bir saldırganın bakış açısını anlama fırsatı bulunmuştur.
    - **Gerçek Zamanlı Tespit Gözlemi:** Yapılan saldırıların Suricata tarafından gerçek zamanlı olarak nasıl algılandığı ve loglara nasıl yansıdığı canlı olarak gözlemlenmiştir.
    
- **Hata Ayıklama ve Problem Çözme Becerileri:**
    
    - Proje boyunca karşılaşılan sayısız kural sözdizimi hatası (özellikle `sid` eksikliği, `threshold` yanlış kullanımları ve `pcre_exec parse error` gibi ki çoğunu rapora koymadık çünkü her bir hata ile karşılaştığımızda herşeyi rapora koymak, okunabilirlik ve zaman açısından olumsuz proje yönetimi sağlardı) sayesinde, hata mesajlarını anlama, sorunun kök nedenini tespit etme ve çözüm odaklı yaklaşımlar geliştirme konusunda önemli bir deneyim kazanılmıştır.
    - Suricata'nın hata ayıklama komutları (`sudo suricata -T`) ve log analizi (`tail -f`) teknikleri, sorun giderme sürecinin temel araçları olarak pekiştirilmiştir. Bu süreç, siber güvenlikte "pes etmeme" ve "detaylı inceleme"nin önemini vurgulamıştır.
    
- **Güvenlik Logu Analizi ve Yorumlama:**
    
    - **`fast.log` Analizi:** IDS tarafından üretilen özet uyarı loglarını (tarih, SID, mesaj, IP adresleri, portlar vb.) hızlı ve doğru bir şekilde okuma ve yorumlama becerisi edinilmiştir.

...

## **6.2. Projenin Başarıları**

Bu proje, belirlenen hedeflere ulaşarak ve bir dizi zorluğun üstesinden gelerek önemli başarılar elde etmiştir. Temel başarılar aşağıda özetlenmiştir:

- **Suricata IDS'in Başarılı Kurulumu ve Konfigürasyonu:** Sanal laboratuvar ortamında Suricata'nın sorunsuz bir şekilde kurulması, temel ağ değişkenlerinin (`$HOME_NET`, `$EXTERNAL_NET`) doğru bir şekilde yapılandırılması ve kural dosyalarının entegrasyonu başarıyla tamamlanmıştır. Bu, projenin temel altyapısının sağlam bir şekilde kurulduğunu göstermektedir.

- **Özel Kural Geliştirme Yeteneğinin Kanıtlanması:** Projede, ICMP trafiği ve Nmap SYN port taramaları gibi spesifik ağ olaylarını tespit etmek üzere **kendi özel Suricata kurallarımız başarıyla yazılmıştır**. Bu, sadece var olan kuralları kullanmakla kalmayıp, belirlenen tehdit senaryolarına özel güvenlik imzaları oluşturabilme yeteneğini açıkça ortaya koymuştur.

- **Saldırı Tespiti ve Simülasyonunun Gerçekleştirilmesi:** Kali Linux üzerinden gerçekleştirilen Nmap SYN taraması gibi saldırı senaryolarının, Mint Linux üzerindeki Suricata IDS tarafından **gerçek zamanlı ve doğru bir şekilde algılanması** sağlanmıştır. Bu uygulamalı testler, Suricata'nın belirlenen tehditleri başarıyla yakalayabildiğini kanıtlamıştır.

- **Detaylı Log Analizi ve Yorumlama:** Suricata tarafından üretilen `fast.log`  formatlarının başarıyla analiz edilmesi ve yorumlanması sağlanmıştır. Log kayıtlarındaki her bir alanın (SID, mesaj, kaynak/hedef IP vb.) anlamı kavranmış ve bir güvenlik olayı hakkında nasıl bilgi çıkarılacağı pratik olarak öğrenilmiştir.

- **Problem Çözme ve Hata Ayıklama Becerilerinin Geliştirilmesi:** Kural yazımı sırasında karşılaşılan **sözdizimi hataları (özellikle `sid` eksikliği ve `threshold` parametrelerinin yanlış kullanımı)** gibi zorluklar, azim ve sistematik bir hata ayıklama süreciyle başarıyla aşılmıştır. Bu deneyimler, siber güvenlik alanında karşılaşılması muhtemel teknik engellerin üstesinden gelme ve çözüm üretme kapasitesini önemli ölçüde artırmıştır.

- **Uygulamalı Öğrenme Yaklaşımının Başarısı:** Projenin teorik bilgiyi pratik uygulama ile birleştirme hedefi tam anlamıyla karşılanmıştır. Sanal bir laboratuvar ortamında gerçek saldırı senaryolarının simüle edilmesi ve IDS'in tepkilerinin gözlemlenmesi, konuya dair derinlemesine ve kalıcı bir anlayış sağlamıştır.

...

## 6.3. Uygulamalı Öğrenmenin Önemi

Bu proje, siber güvenlik gibi hızla gelişen ve dinamik bir alanda **uygulamalı öğrenmenin** vazgeçilmez rolünü bir kez daha kanıtlamıştır. Teorik bilgi, bir konunun temellerini anlamak için kritik olsa da, gerçek dünya senaryolarının simüle edildiği ve pratik problemlerle yüzleşildiği bir ortamda öğrenim, bilginin kalıcılığını ve uygulanabilirliğini artırır.

Bu proje özelinde, uygulamalı öğrenmenin sağladığı başlıca faydalar şunlardır:

- **Teoriden Pratiğe Köprü Kurma:** IDS'lerin nasıl çalıştığı, kural yazımının detayları ve log analizi gibi teorik bilgiler, sanal bir laboratuvarda canlı ağ trafiği üzerinde test edildiğinde çok daha somut ve anlaşılır hale gelmiştir. Suricata'nın `flags:S` gibi basit bir kural seçeneğiyle bile Nmap SYN taramasını nasıl yakaladığını görmek, bu seçeneğin teorik tanımından çok daha etkilidir.

- **Problem Çözme Becerilerinin Gelişimi:** Kural sözdizimi hataları, servis başlatma sorunları veya beklenmedik log çıktıları gibi karşılaşılan teknik zorluklar, beni çözüm bulmaya zorlamıştır. Bu süreçte, hata mesajlarını analiz etme, farklı yaklaşımlar deneme ve sistematiğe oturtulmuş bir hata ayıklama süreci izleme gibi kritik problem çözme yetkinlikleri pekiştirilmiştir. Bu beceriler, siber güvenlik alanındaki herhangi bir profesyonel için temeldir.

- **Deneyimsel Bilgi ve Kalıcı Öğrenme:** Kitaplardan veya derslerden edinilen bilgilerin aksine, bizzat bir sanal makineyi kurmak, bir IDS'i yapılandırmak, kendi kuralınızı yazmak ve bir saldırıyı tespit etmek, bilgiyi "yaparak öğrenme" metoduyla kalıcı hale getirmiştir. Bu tür deneyimler, gelecekteki siber güvenlik görevlerinde karşılaşılabilecek benzer durumlar için sağlam bir temel oluşturur.

- **Gerçek Dünya Senaryolarına Hazırlık:** Sanal bir "hacker" makinesi ve "kurban" makinesi kullanarak saldırı ve savunma senaryolarını simüle etmek, beni gerçek siber saldırıların nasıl gerçekleştiğini ve bir IDS'in bu tür olaylara nasıl tepki verdiğini anlamaya hazırladı. Bu, yalnızca bir aracı kullanmakla kalmayıp, güvenlik operasyonlarının dinamiklerini kavramamı da kolaylaştırdı..
- **Özgüven ve Yeterlilik Duygusu:** Başlangıçtaki zorluklara rağmen, her bir aşamada elde edilen başarılar (örneğin, Nmap taramasının loglarda görünmesi), bana teknik yeteneklerime dair özgüven kazandırmış ve siber güvenlik alanında daha karmaşık projelere girişmek için cesaret vermiştir.


# Gelecek Çalışmalar ve Geliştirme Önerileri 

## 7.1. Daha Gelişmiş Kural Senaryoları (HTTP, DNS, İç Ağ Hareketleri)

Eğer daha gelişmiş kural senaryolarınız var ise, İçindekiler bölümünün Gerçekleştirilen Çalışmalar ve Uygulamalı Adımlar başlığının altındaki 4.2. Özel Kural Geliştirme ve Testler kısmına diğer kuralları ekleyebilirsiniz. Başlıklara, numaralandırma sistemine dikkat etmeniz dileğiyle. Lütfen Yardımcı Şablonlar kısmında bulunan şablonlardan hareket edin.

## 7.2. IPS (Engelleme) Modu Araştırması

Projenin sadece IDS odaklı ve Suricata yaklaşımının sebebi, olabildiğince basit ve detaylandırmalardan kaçınarak sağlam bir temel oluşturmaktır. İlerleyen vakitlerde eğer zamanım kalırsa IPS için de kurallar tanımlamaya çalışacağım. Eğer sizinde geliştirme önerileriniz varsa ama nasıl yapacağınızı bilmiyorsanız, fikir danışmak, sohbet etmek aynı şehirdeysek çay içmek ya da aklınızdaki ne ise onun için bana [buradan](https://www.linkedin.com/in/alpulkegul/) ya da [buradan](https://www.instagram.com/alpulkegul/) ulaşabilirsiniz :) 
