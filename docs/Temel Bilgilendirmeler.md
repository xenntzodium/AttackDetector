# Kurulum Aşamaları

Ubuntu, kali...

```bash
apt install suricata
```

```bash
apt install snort
```

Arch...

```bash
yay -S suricata
```

```bash
yay -S snort
```

Version

```bash
snort -V
```

```bash
suricata -V
```

Snort Dizin

```bash
cd /etc/snort
ls
```
- `snort.conf` -> Ana yapılandırma dosyası
- `rules dizini` -> **imza tabanlı tespit kurallarını** içeren dosyalar bulunur
	- Örneğin: Web saldırılarına, exploit tespitlerine yönelik kurallar
- `clasification.config` -> Hangi alarmın hangi kuralı vereceğini ve kategorilerini belirliyoruz
- `file_magic.conf` -> Snort'un tespit için kullandığı dosya tiplerini tanımlayan imza kuralları bulunuyor
- `gen-msg.map` -> Kural IDleri ve açıklayıcı mesajları eşleştiren bir dosya
- `reference.config` -> Alarmlar için harici referans linkleri
- `threshold.conf` -> Alarmlar için eşik değerleri ve bastırma kuralları
	- suppression gen_id 1, sig_id 1852, track by_src, ip 192.168.1.100
	
	  Eğer `sig_id 1852`  (signature (imzalı)) numaralı alarm, **192.168.1.100** IP adresinden geliyorsa, bu alarmı **görmezden gel** (loglama, uyarı verme vb. yapma).
- `unicode.map` -> Unicode karakter dönüşüm dosyasıdır
	- Bu dosya da genelde UNICODE saldırılarını tespit etmek için kullanılır.

Suricata Dizini

```bash
cd /etc/suricata
ls
```

`suricata.yaml` -> Ana yapılandırma dosyası

`clasification.config` -> Alarmların öncelik seviyeleri ve kategorileri bulunuyor

`reference.config` -> Alarmlar için harici referans linkleri

# Snort & Suricata
## IDS ve IPS nedir?

I**D**S | Instrusion Detection System | İzinsiz Giriş **Tespit** Sistemi
- Pasif izleme yapar, saldırıları **loglar** ve uyarı verir.
- Olayları kaydeden bir güvenlik kamerası olarak düşünülebilir. İzler, kaydeder fakat **müdahale etmez**.
- **Dedektif**

I**P**S | Intrusion Prevention System | İzinsiz Giriş **Önleme** Sistemi
- **Aktif** müdahale eder. Saldırıları **engeller**.
- Kapıdaki güvenlik görevlisi gibi düşünülebilir. Şüpheliyi içeri almaz.
- **Polis**
 
## IDS vs. IPS

| Özellik         | IDS                    | IPS                      |
| --------------- | ---------------------- | ------------------------ |
| **Tepki verme** | Pasif (Loglama/Uyarı)  | Aktif (Engelleme)        |
| **Kullanım**    | * `snort -l`           | * `suricata -q`          |
| **Örnek**       | Port taramasını tespit | Brute force'u engelleme  |
| **Gecikme**     | * Düşük                | * Yüksek (Analiz süresi) |
| **Risk**        | * False Positive       | * False Negative         |

- `snort -l /log/dizin`: Komut, snort'un **loglama modunda** yani IDS gibi çalıştırıldığını ifade eder. -l komutu ile logların nereye kaydedileceğini belirtebiliyoruz. 
- `suricata -q 0`: Suricata'yı bir IPS gibi çalıştırmak için kullanılır. `-q` parametresi ile hangi NFQUEUE kuyruğunu dinleyeceğini belirtiyoruz. Bu modda Suricata gelen paketleri analiz eder ve şüpheli olanları **aktif olarak** engeller. Yani gerçek zamanlı müdahale de bulunur.

- Gecikme burada, **ağ paketlerinin analiz edilip işlenmesi sırasında oluşan zaman farkını** ifade ediyor. 
    - IDS (Snort): Paketi alır, kopyasını inceler ama aktarıma müdahale etmez. Bu yüzden **gecikme çok düşüktür**.
    - IPS (Suricata): Paketi analiz eder. Analiz ettikten sonra karar verir: paket geçsin mi, engellensin mi ? Bu analiz süresi bir kaç milisaniyelikte olsa gecikme yaratır. Kritik ağlarda bu bile fark edilebilir.

- False Positive: Kısaca aslında tehdit olmayan bir şeyi tehdit olarak algılamaktır. **Mesela normal bir port taramasını saldırı olarak algılayıp alarm vermesi**. Bu durum IDS'lerde daha sık olur çünkü **saldırı benzeri davranışlar, saldırılardan daha yüksek bir olasılık getirir.** Bu nedenle de IDS yanlış pozitif olarak değerlendirilir.
- False Negative: **Gerçek bir saldırıyı görememesi, gözden kaçırması.** Mesela gerçek bir brute force saldırısını fark edememesi durumu. IPS sistemleri bazen performans kaygısıyla bazı paketleri atlayabilir. Bu da false negative, yanlış negatif riskini arttırır.



# Snort

## Modüler Yapı

![Snort-Moduler-Yapı](https://private-user-images.githubusercontent.com/158475086/436662188-cf57a4c7-0b98-4931-b8bf-c224a10f4346.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDU1NDA0MzMsIm5iZiI6MTc0NTU0MDEzMywicGF0aCI6Ii8xNTg0NzUwODYvNDM2NjYyMTg4LWNmNTdhNGM3LTBiOTgtNDkzMS1iOGJmLWMyMjRhMTBmNDM0Ni5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNDI1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDQyNVQwMDE1MzNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT04YjZiY2RkNDVkMGI5MzJiMjkyODk5YTk3NGJiZWM3NTI1Mjg2YTUyM2QzZDBlNTU4ZDk0OWIzMDkyZWUyMTAzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.jBI3-MeHPjmgq8tdES7HghUZevw9pCbDa9sATlAkPG8)
*Yukarıdaki görsel lastguardsecurity.com internet sitesinden alınmıştır.*

Snort temelde 4 modülden oluşur. Bunlar kod çözücü, ön işlemci, tespit motoru, alarm üretimi. İşlemler aşağıdaki gibi gerçekleşir.
- Libpcap tarafından yakalanan paketler hem .pcap dosyaları aracılığıyla hem de canlı trafik üzerinden çözümlenir. 
- Kod çözücü, paketleri katmanlarına ayırır. (Ethernet, IP, TCP/UDP gibi..)
- Ön işlemci, paketleri tespit motorunun anlayabileceği şekle sokar.
- Tespit motoru, Snort'un beynidir diyebiliriz. Tespit motorunda kural setleri yazılır. Bu kısımda kural setleri ile saldırı tespitleri yapılır.
- Uyarı oluşturma kısmı ise tespit edilen saldırıların ne tür uyarı oluşturacağını belirler. 

![Snort-Moduler-Yapı2](https://private-user-images.githubusercontent.com/158475086/436668808-71bbd172-a08e-4867-8912-4ba956a042be.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDU1NDA0MzMsIm5iZiI6MTc0NTU0MDEzMywicGF0aCI6Ii8xNTg0NzUwODYvNDM2NjY4ODA4LTcxYmJkMTcyLWEwOGUtNDg2Ny04OTEyLTRiYTk1NmEwNDJiZS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNDI1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDQyNVQwMDE1MzNaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT01ZDIzYWVkOTU0NjhkZWEzMWU4M2RlZWNlNWZlZTBlMjgzZjg0OGZmNjk5MjBlODNjOTVhNWUxMDA1YjNmZjI1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.eNfyEVaJlbB6FxsHiliJHdSHJfxkpMUNFAwCFXbjSKs)

*DÜMF Mühendislik Dergisi syf. 66*

- Yukarıdaki görselden de gördüğümüz gibi oluşturulan uyarılardan bir uyarı veritabanı oluşturulabilir. Daha sonra uyarılar oluşturulur.

* libpcap, ağ arayüzü üzerinden geçen trafiği yakalamak ve işlemek için kullanılan bir düşük seviye C kütüphanesidir. Ağdaki ham paketleri yakalar. Snort, Wireshark gibi uygulamalar ise bu verileri analiz eder.

NOT:

Aslında Snort'ta, Suricata'da hem IDS hem IPS olarak çalışabilir. Fakat ilk niyet ve varsayılan kullanım şekli Snort -> IDS, Suricata -> IPS olarak bir **etiketlenmeye** yol açtı. Bunu iki neden de inceleyecek olursak;
- Snort, ilk çıktığında sadece bir IDS'ti. Daha sonraki sürümlerinde IPS olarakta kullanılabiliyor olmasına rağmen, IDS olarak kullanılmaya devam etti.
- Suricata IPS destekli olarak tasarlandı ve IPS olarak Snort'tan daha performanslı çalışmakta. 

Yani kısacası, topluluk alışkanlıkları ve yaygın kullanım şekillerinden dolayı snort IDS, suricata IPS olarak kullanılıyor. Fakat her ikiside hem IDS, hem IPS olarak çalışabiliyor.

# Suricata 

![[Suricata Ağ Mimarisi.canvas]]
