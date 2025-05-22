### **İçindekiler**

- [x] Giriş. 
	- [x] 1.1  Projenin Amacı 
	- [x] 1.2 Projenin Önemi
- [x] Arka Plan ve Temel Kavramlar 
	- [x] 2.1. Saldırı Tespit Sistemleri (IDS) ve Saldırı Önleme Sistemleri (IPS) Nedir? 
	- [x] 2.2. Snort Mimarisi
	- [x] 2.3. Suricata Mimarisi 
	- [x] 2.4. Saldırı Senaryoları ve Nmap
- [x] Proje Ortamı ve Kullanılan Araçlar 
	- [x] 3.1. Sanal Makine Konfigürasyonu 
		- [x] 3.1.1. Mint Linux (Hedef/IDS Makinesi) 
		- [x] 3.1.2. Kali Linux (Saldırgan Makinesi) 
	- [x] 3.2. Ağ Topolojisi 
	- [x] 3.3. Kullanılan Yazılımlar ve Versiyonları
- [ ] Gerçekleştirilen Çalışmalar ve Uygulamalı Adımlar 
	- [ ] 4.1. Suricata Kurulumu ve Temel Konfigürasyon 
		- [x] 4.1.1. Kurulum Süreci 
		- [ ] 4.1.2. `suricata.yaml` Dosyası Ayarları (Özellikle `$HOME_NET` tanımı)
		- [ ] 4.1.3. Servis Yönetimi (Başlatma, Durdurma, Durum Kontrolü) 
	- [ ] 4.2. Özel Kural Geliştirme ve Testler 
		- [ ] 4.2.1. ICMP (Ping) Tespit Kuralı 
			- [ ] 4.2.1.1. Kuralın Tanımı ve Amacı 
			- [ ] 4.2.1.2. Uygulama ve Test Süreci 
			- [ ] 4.2.1.3. Elde Edilen Log Çıktıları ve Yorumları 
			- [ ] 4.2.1.4. Karşılaşılan Zorluklar ve Öğrenimler (Threshold ve Bastırma Mekanizması) 
		- [ ] 4.2.2. Nmap SYN Port Tarama Tespit Kuralı 
			- [ ] 4.2.2.1. Kuralın Tanımı ve Amacı (Neden İç Ağ Taraması Önemli?) 
			- [ ] 4.2.2.2. Saldırı Senaryosu (Kali'den Nmap Taraması) 
			- [ ] 4.2.2.3. Uygulama ve Test Süreci 
			- [ ] 4.2.2.4. Elde Edilen Log Çıktıları ve Yorumları 
			- [ ] 4.2.2.5. Karşılaşılan Zorluklar ve Öğrenimler (Sözdizimi Hataları, Hata Ayıklama Süreci)
- [ ] Log Analizi ve Yorumlama 
	- [ ] 5.1. `fast.log` Analizi 
		- [ ] 5.1.1. İçeriği ve Yapısı 
		- [ ] 5.1.2. Önemli Alanlar ve Yorumlama 
	- [ ] 5.2. `eve.json` Analizi 
		- [ ] 5.2.1. İçeriği ve Yapısı (JSON Formatı) 
		- [ ] 5.2.2. `jq` Aracı Kullanımı ve Önemi 
		- [ ] 5.2.3. SIEM Entegrasyonu Potansiyeli
- [ ] Sonuç ve Proje Kazanımları 
	- [ ] 6.1. Projeden Elde Edilen Temel Bilgi ve Beceriler 
	- [ ] 6.2. Projenin Başarıları 
	- [ ] 6.3. Uygulamalı Öğrenmenin Önemi
- [ ] Gelecek Çalışmalar ve Geliştirme Önerileri 
	- [ ] 7.1. Daha Gelişmiş Kural Senaryoları (HTTP, DNS, İç Ağ Hareketleri) 
	- [ ] 7.2. Performans Optimizasyonu ve `threshold` Detayları 
	- [ ] 7.3. SIEM Entegrasyonu (ELK Stack vb.) 
	- [ ] 7.4. IPS (Engelleme) Modu Araştırması
- [ ] Referanslar (Varsa)
- [ ] Ekler (Opsiyonel: Ekran Görüntüleri, Konfigürasyon Dosyaları vb.)

---
#### 1. GİRİŞ

Siber güvenlik dünyasında bireylere ve kuruluşlara gerçekleştirilen tehditleri tabiri caizse 'avlamak' için, başlıca savunma mekanizmalarımız haline gelen Saldırı Tespit Sistemlerini incelediğimiz bu raporda, IDS-IPS mantığını, Suricata ve Snort yazılımlarını incelemeye çalıştık. Bir çok konuda araştırma yaptık ve Temel Seviye de 'eğitim odaklı' bir yaklaşımla yaptığımız araştırmaların hepsini bir rapor haline getirdik. Bilgileri olabildiğince sindirilebilir ve öz vermeye çalıştık. Keyifli okumalar.

##### 1.1 Projenin Amacı

- Suricata ve Snort mimarilerini inceleyerek, arka planda nasıl çalıştıklarını anlamak
- Uygulamalı bir yaklaşım ile IDS, IPS mantığını derinlemesine öğrenmek
- Sanal bir laboratuvar oluşturup, aynı ağda bulunan iki bilgisayarın 'kurban', 'hacker' yaklaşımıyla simüle edilmesi
- ...
##### 1.2 Projenin Önemi

Siber saldırıların karmaşıklığı göze alındığında IDS, IPS savunma mekanizmalarının güvenlik stratejilerinde ayrılmaz bir parça olduğunu söylemeye gerek yoktur sanırım.

- Tehdit avcılığı; saldırıların ve şüpheli ağ faaliyetlerinin nasıl tespit edileceğine dair iç görüler sunar.
- Kural geliştirme yetenekleri; özel güvenlik ihtiyaçlarına yönelik özelleştirilmiş kurallar yazabilme becerisi kazandırır.
- Hata ayıklama ve problem çözme; güvenlik araçları ile çalışırken karşılaşılan teknik zorlukları (Bu projede karşılaştığım hata sayısı yaklaşık 15-20 adet, fakat her birini projeye eklemek zor.) aşma ve çözüme ulaşma yeteneğini geliştirir.
- Log Analizi; Üretilen güvenlik loglarından anlamlı bilgiler çıkarma ve olaylara müdahale için bu bilgileri kullanma pratiği sağlar.

Bu yönleri ile projenin incelenmesinin, öğrencilere ve siber güvenlik alanına ilgi duyan bireylere kritik düşünme ve pratik uygulama becerileri kazandırarak, bu alanda gelecekteki kariyerleri için sağlam bir temel oluşturacağını düşünüyorum.

#### 2. ARKAPLAN VE TEMEL KAVRAMLAR

İşin arkaplanına bakmamızın sebebi şu: Bilgiyi öğrenmek; google ya da çeşitli AI platformlarına sorup okumak kadar basit değil. Bu yüzden uygulamaya geçmeden önce bilmemiz gereken şeyleri kavramlara ayırarak, kafamızda bir zihin haritasına dönüştürmemiz gerekiyor ki, işin arka planında sistemin nasıl ilerlediğini bilelim. Bu yüzden dökümantasyon klasörümüze konular hakkında bilgiler toparladım. 

Projenin daha sıkıcı ve daha yoğun (ki gayet yoğun) olmaması için Snort kısmının uygulamasını projeden çıkardık. Projede  IDS ve IPS'i daha iyi anlamak için Snort üzerinde temel kavramları öğreneceğiz. Snort'un temel kavramlarını öğrendikten ve anladıktan sonra Suricata'nın sadece mimarisine baktığımızda, zaten kafamızda bir çok şey oturacaktır.

Bu kısımda bilmemiz gerekenler sırasıyla;

##### 2.1. Saldırı Tespit Sistemleri (IDS) ve Saldırı Önleme Sistemleri (IPS) Nedir?

[Konu hakkında detaylı bilgi için buraya tıklayın.](https://github.com/xenntzodium/AttackDetector/blob/main/D%C3%B6k%C3%BCmantasyon/IDS%20ve%20IPS%20Nedir%3F.md)

##### 2.2. Snort Mimarisi

[Konu hakkında detaylı bilgi için buraya tıklayın.](https://github.com/xenntzodium/AttackDetector/blob/main/D%C3%B6k%C3%BCmantasyon/Snort%20Mimarisi.md)

##### 2.3. Suricata Mimarisi

DİKKAT: Suricata mimarisini aşağıdaki canvas üzerinde daha detaylı incelemek için, dosyayı indirip obsidian üzerinden görüntüleyebilirsiniz. 

![Suricata-Canvas](/images/Pasted%20image%2020250521234847.png)

[Dosyayı şu sayfadan sağda bulunan indirme kısmından indirebilirsiniz.](https://github.com/xenntzodium/AttackDetector/blob/main/D%C3%B6k%C3%BCmantasyon/Suricata%20Mimarisi.canvas)

Suricata'yı güçlü kılan bazı temel özellikler şunlardır:

- **Çoklu Çekirdek Desteği (Multi-threading):** Geleneksel IDS'lerin aksine, Suricata çoklu CPU çekirdeğinden faydalanarak yüksek performanslı ağ ortamlarında bile verimli çalışabilir.
- **Protokol Tanımlama:** HTTP, TLS/SSL, DNS gibi birçok uygulama katmanı protokolünü otomatik olarak tanır ve bu protokoller içindeki tehditleri tespit edebilir.
- **Dosya Çıkarma (File Extraction):** HTTP veya SMTP trafiği üzerinden geçen şüpheli dosyaları otomatik olarak çıkarabilir ve disk üzerine kaydedebilir. Bu, zararlı yazılım analizinde önemli bir özelliktir.
- **Modüler Mimari:** Eklentiler ve yeni özellikler için modüler bir yapıya sahiptir.
- **Çoklu Log Formatları:** Tespit edilen olayları `fast.log` (insan okunabilir özet) ve `eve.json` (makine tarafından işlenebilir detaylı JSON) gibi farklı formatlarda loglayabilir.
- **IPS Yeteneği:** Hem IDS (sadece tespit) hem de IPS (tespit ve engelleme) modunda çalışabilir.

##### 2.3. Saldırı Senaryoları ve Nmap

[Konu hakkında detaylı bilgi için buraya tıklayın.](https://github.com/xenntzodium/AttackDetector/blob/main/D%C3%B6k%C3%BCmantasyon/Sald%C4%B1r%C4%B1%20Senaryolar%C4%B1%20-%20NMAP.md)

NOT: Bu bölümde açıklanan kavramlar ve araçlar, projemizin ilerleyen adımlarında Suricata üzerinde geliştirilen kuralların ve gerçekleştirilen saldırı simülasyonlarının temelini oluşturmuştur. Tabiki konular bunlarla sınırlı değil, daha öğrenmemiz gereken çok şey var. Fakat şimdilik yukarıdaki bilgilerin kafamızda bir şeyler canlandırması yeterli. 

---

#### 3. Proje Ortamı ve Kullanılan Araçlar

Projenin uygulanması ve test edilmesi için bir sanal ortam oluşturuldu. 
##### 3.1. Sanal Makine Konfigürasyonu

Proje, **QEMU/KVM (Kernel-based Virtual Machine)** sanallaştırma platformu üzerinde iki ayrı sanal makine kullanılarak gerçekleştirilmiştir. QEMU/KVM, Linux çekirdeğinin sanallaştırma yeteneklerinden faydalanan güçlü ve performansa dayalı bir sanallaştırma çözümüdür. Bu makineler, projenin güvenlik senaryolarını simüle etmek üzere belirli rollerle yapılandırılmıştır:

Sanal Makine Yöneticisi: QEMU/KVM
- Sanal Bilgisayar 1: Kali Linux -> Saldırgan
- Sanal Bilgisayar 2: Mint -> Hedef (Kurban)

###### 3.1.1. Mint Linux (Hedef/IDS Makinesi)

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

##### **3.1.2. Kali Linux (Saldırgan Makinesi)**

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

##### 3.2. Ağ Topolojisi 

![Ağ%20Topolojisi.png](/images/Ağ%20Topolojisi.png)

##### 3.3. Kullanılan Yazılımlar ve Versiyonları

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

#### 4. Gerçekleştirilen Çalışmalar ve Uygulamalı Adımlar 

Bu bolümde, projenin hedeflerine ulaşmak için adım adım gerçekleştirilen teknik çalışmalar ve uygulamalı adımlar detaylandırıldı. Suricata'nın kurulumundan özel kural geliştirme ve saldırı simülasyonlarına kadar olan süreç, karşılaşılan zorluklar ve elde edilen öğrenimlerle birlikte sunulmaktadır.

Suricata IDS'in proje ortamına entegrasyonu, sistemin temel yapılandırmasıyla başlamıştır.

##### 4.1. Suricata Kurulumu ve Temel Konfigürasyon

Suricata IDS'in proje ortamına entegrasyonu, sistemin temel yapılandırmasıyla başlamıştır.

###### 4.1.1. Kurulum Süreci

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

###### 4.1.2. `suricata.yaml` Dosyası Ayarları

`suricata.yaml` -> Suricata'nın konfigürasyon dosyası. Burada ağ ortamımıza göre özel ayarlamalar yapabiliyoruz. En kritik düzenlemelerden biri, ağ değişkenlerini tanımlamak.
<<<<<<< HEAD

Önce `ip a` komutu ile hangi ip bloğunu kullanacağımızı öğrenmeliyiz. Bunu öğrenirken bir de iki bilgisayarın haberleşip haberleşemediğini test edelim.

- Kali makinemizin terminaline ve mint makinemizin terminaline `ip a` komutunu girerek ip adreslerini öğrenebiliriz.
- Kali makinemizin ip adresi: 192.168.122.153 
- Mint: 192.168.122.165
- Şimdi Kali makinemizin terminaline `ping 192.168.122.165` yazıp mint makinemize ping gönderebiliyor muyuz test edelim.
- Ardından aynı şekilde mint makinemizin terminaline `ping 192.168.122.153` yazarak testimizi yapalım. Sonuçlar aşağıdaki gibi olmalıdır.

![[iletisimkontrol.gif]]



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



=======
>>>>>>> 2485daf65cb7f63e864e7ccc58a26e84302f4890
