###### I**D**S | Instrusion Detection System | İzinsiz Giriş **Tespit** Sistemi

- Pasif izleme yapar, saldırıları **loglar** ve uyarı verir.
- Olayları kaydeden bir güvenlik kamerası olarak düşünülebilir. İzler, kaydeder fakat **müdahale etmez**.
- **Dedektif**
###### I**P**S | Intrusion Prevention System | İzinsiz Giriş **Önleme** Sistemi

- **Aktif** müdahale eder. Saldırıları **engeller**.
- Kapıdaki güvenlik görevlisi gibi düşünülebilir. Şüpheliyi içeri almaz.
- **Polis**

## IDS VS IPS

| Özellik         | IDS                    | IPS                      |
| --------------- | ---------------------- | ------------------------ |
| **Tepki verme** | Pasif (Loglama/Uyarı)  | Aktif (Engelleme)        |
| **Örnek**       | Port taramasını tespit | Brute force'u engelleme  |
| **Gecikme**     | * Düşük                | * Yüksek (Analiz süresi) |
| **Risk**        | * False Positive       | * False Negative         |

- Burada belirttiğimiz gecikmeyi basitçe ifade etmemiz gerekirse, **ağ paketlerinin analiz edilip işlenmesi sırasında oluşan zaman farkını** ifade eder. IDS, paketi alır, kopyasını inceler ama aktarıma müdahale etmez. Bu yüzden **gecikme çok düşüktür**. IPS ise paketi analiz eder. Analiz ettikten sonra karar verir: paket geçsin mi, engellensin mi ? Bu analiz süresi bir kaç milisaniyelikte olsa gecikme yaratır. Bu milisaniyelik gecikmeler bile fark edilebilir.
- False Positive: Kısaca aslında tehdit olmayan bir şeyi tehdit olarak algılamaktır. 
	**Mesela normal bir port taramasını saldırı olarak algılayıp alarm vermesi**. Bu durum IDS'lerde daha sık olur çünkü **saldırı benzeri davranışlar, saldırılardan daha yüksek bir olasılık getirir.** Bu nedenle de IDS yanlış pozitif olarak değerlendirilir. 
- False Negative: **Gerçek bir saldırıyı görememesi, gözden kaçırması.** 
	Mesela gerçek bir brute force saldırısını fark edememesi durumu. IPS sistemleri bazen performans kaygısıyla bazı paketleri atlayabilir. Bu da false negative, yanlış negatif riskini arttırıyor.