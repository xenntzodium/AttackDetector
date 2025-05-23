
Lütfen aşağıdaki şablonu takip edin. 
Lütfen sıralamalara dikkat edin.

### 4.{x}.1. KURAL ADI

KURAL TANIMI VE AMACI: 

SYNTAX;

```plaintext
<action> <protocol> <src_ip> <src_port> <direction> <dst_ip> <dst_port> (<options>)
```

```plaintext
{YENİ KURALI BURAYA YAZABİLİRSİNİZ}
```

- Birinci komut
- İkinci komut
- Üçüncü komut
- Dördüncü komut
- beşinci komut
- ...
- mesaj kısmı
- seçenek 1
- ...
- ...

#### 4.{x}.2.1. Saldırı Senaryosu (ÖRN: Kali'den Nmap Taraması)

#################### ÖRNEK METİN #################
Saldırgan makinesi Kali Linux (IP: 192.168.122.153) üzerinden hedef makine Mint Linux (IP: 192.168.122.165) adresine yönelik bir SYN port taraması başlatılmıştır. 

```bash
sudo nmap -sS -p 1-1000 192.168.122.165

# -sS -> Yarı açık (Stealth) TCP SYN taraması yapar. Bu, Nmap’in en yaygın ve hızlı tarama yöntemidir. TCP üç yönlü el sıkışmanın ilk adımı olan SYN paketini gönderir ama bağlantıyı tamamlamaz.

# -p 1-1000 -> port aralığı
```

Bu komutu kali makinemiz de kullanacağız. Cebimizde kalsın.
############### ÖRNEK METİN SONU ###################
#### 4.{x}.2.2. Uygulama ve Test Süreci 

################ ÖRNEK METİN #######################
Şimdi tekrardan bir kural tanımlamamız gerekiyor. Bunu terminale yapıştırıp direk işimizi kolaylaştıralım. 

```bash
echo 'alert tcp any any -> $HOME_NET any (msg:"PROJE TEST: Nmap SYN Tarama Tespit Edildi"; flags:S; sid:2000002; rev:1;)' | sudo tee -a /etc/suricata/rules/local.rules
```

######### SABİT METİN ########
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

######## SABİT METİN  SONU #######

```bash
# Kali terminaline
nmap -sS -p 1-1000 192.168.122.165
```

Aşağıdaki gibi bir sonuç beklenmektedir.

![test-3.gif](/images/test-3.gif)

############EKRAN GÖRÜNTÜLERİYLE DESTEKLEYEBİLİRSİNİZ ##############
##########ÖRNEK METİN SONU #############
#### 4.{x}.2.3. Elde Edilen Log Çıktıları ve Yorumları

############ ÖRNEK METİN ############## 
```plaintext
05/22/2025-17:46:22.778336  [**] [1:2000002:1] PROJE TEST2: Nmap SYN Tarama Tespit Edildi [**] [Classification: (null)] [Priority: 3] {TCP} 192.168.122.153:54675 -> 192.168.122.165:771

[TARİH-SAAT]  [**] [GENEL BİLGİ] MESAJ [**] [SINIFLANDIRMA] [ÖNCELİK] {PROTOKOL} KAYNAK_IP:PORT -> HEDEF_IP:PORT
```

Sanırım bu kadar yeterli, yukarıdaki çıktıların altında belirttiğim syntax'ı incelerseniz kafanızda bir şeyler canlanacaktır.
##########ÖRNEK METİN SONU #############