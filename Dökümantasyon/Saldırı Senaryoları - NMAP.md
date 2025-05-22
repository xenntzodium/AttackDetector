Bir Saldırı Tespit Sistemi'nin (IDS) etkinliğini test etmek ve uygulamalı öğrenme deneyimini pekiştirmek için gerçek dünya saldırı senaryolarını taklit etmek hayati önem taşımaktadır. Bu proje kapsamında, özellikle iç ağda gerçekleşebilecek bir tehdit modellemesi yapılmış ve bir saldırganın (Kali Linux makinesi) ağdaki potansiyel hedefler (Mint Linux makinesi) hakkında bilgi toplama sürecini simüle etmek için çeşitli senaryolar uygulanmıştır. Bu senaryolarda temel keşif aracı olarak **Nmap** kullanılmıştır.

**Nmap (Network Mapper):** Açık kaynaklı ve çok yönlü bir ağ keşif ve güvenlik denetim aracıdır. Nmap, ağ yöneticileri ve güvenlik uzmanları tarafından aşağıdakiler de dahil olmak üzere geniş bir yelpazedeki görevler için kullanılır:

- Ağdaki aktif cihazları keşfetme (Host Discovery).
- Hedef sistemlerde açık portları belirleme (Port Scanning).
- Çalışan servislerin versiyonlarını ve özelliklerini tespit etme (Service Version Detection).
- Hedef sistemlerin işletim sistemlerini tahmin etme (Operating System Detection).
- Ağdaki güvenlik zafiyetlerini belirleme.

Siber saldırıların ilk aşamalarından biri genellikle **keşif (reconnaissance)** olarak adlandırılır. Bu aşamada saldırgan, hedef ağın yapısını, canlı sistemleri, açık portları ve çalışan servisleri anlamaya çalışır. Nmap, bu bilgi toplama sürecinin en yaygın ve etkili araçlarından biridir.

Projemizde, Nmap'in en temel ve sık kullanılan tarama yöntemlerinden biri olan **SYN taraması (TCP SYN Scan)** kullanılmıştır. SYN taraması, TCP üçlü el sıkışmasını tamamlamadan, sadece SYN bayraklı paketler göndererek portların açık olup olmadığını anlamaya çalışır. Bu yöntem, hedef sistemde tam bir bağlantı kurmadığı için, bazı durumlarda daha gizli kalmayı sağlar ancak bir IDS tarafından tespit edilmeye elverişli karakteristik trafik desenleri bırakır.