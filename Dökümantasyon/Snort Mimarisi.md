![[Snort Mimarisi.png]]
*Yukarıdaki görsel lastguardsecurity.com internet sitesinden alınmıştır.*

Snort temelde 4 modülden oluşur. Bunlar kod çözücü, ön işlemci, tespit motoru, alarm üretimi. İşlemler aşağıdaki gibi gerçekleşir.
- Libpcap tarafından yakalanan paketler hem .pcap dosyaları aracılığıyla hem de canlı trafik üzerinden çözümlenir. 
- Kod çözücü, paketleri katmanlarına ayırır. (Ethernet, IP, TCP/UDP gibi..)
- Ön işlemci, paketleri tespit motorunun anlayabileceği şekle sokar.
- Tespit motoru, Snort'un beynidir diyebiliriz. Tespit motorunda kural setleri yazılır. Bu kısımda kural setleri ile saldırı tespitleri yapılır.
- Uyarı oluşturma kısmı ise tespit edilen saldırıların ne tür uyarı oluşturacağını belirler. 

![[Snort Mimarisi 2.png]]
*DÜMF Mühendislik Dergisi syf. 66*

- Yukarıdaki görselden de gördüğümüz gibi oluşturulan uyarılardan bir uyarı veritabanı oluşturulabilir. Daha sonra uyarılar oluşturulur.

* libpcap, ağ arayüzü üzerinden geçen trafiği yakalamak ve işlemek için kullanılan bir düşük seviye C kütüphanesidir. Ağdaki ham paketleri yakalar. Snort, Wireshark gibi uygulamalar ise bu verileri analiz eder.

NOT:

Aslında Snort'ta, Suricata'da hem IDS hem IPS olarak çalışabilir. Fakat ilk niyet ve varsayılan kullanım şekli Snort -> IDS, Suricata -> IPS olarak bir **etiketlenmeye** yol açtı. Bunu iki neden de inceleyecek olursak;
- Snort, ilk çıktığında sadece bir IDS'ti. Daha sonraki sürümlerinde IPS olarakta kullanılabiliyor olmasına rağmen, IDS olarak kullanılmaya devam etti.
- Suricata IPS destekli olarak tasarlandı ve IPS olarak Snort'tan daha performanslı çalışmakta. 

Yani kısacası, topluluk alışkanlıkları ve yaygın kullanım şekillerinden dolayı snort IDS, suricata IPS olarak kullanılıyor. Fakat her ikiside hem IDS, hem IPS olarak çalışabiliyor.