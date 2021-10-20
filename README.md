# Pywsus Aracı ile WSUS Güncellemesine Zararlı Yazılım Enjeksiyonu

Bu yazıda inceleyeceğimiz araç WSUS Server’dan güncelleme çeken bir bilgisayara gerekli güncelleme yerine sahte bir güncelleme dosyası göndermemizi ve istediğimiz komutu yetkili kullanıcı olarak çalıştırmamızı sağlar.

Çok kullanıcılı şirketler internet trafiği sıkıntısı çekmemek adına güncellemeleri Microsoft Sunucularından kendi güncelleme sunucularına çekerek tek bir indirme ile iç ağda belki de binlerce cihaza güncellemeyi sağlayabilir.

10.000 tane bilgisayarın Windows güncellemelerini Microsoft sunucularından ayrı ayrı indirdiğini bir hayal edin.

- WSUS Server: Windows Server üzerinde Windows Update Center’dan gerekli updateleri kendi üstüne indirerek yapıdaki diğer cihazlara bu updateleri iletmekle görevli yapıdır.
- Pywsus: Kendine gelen güncelleme isteğine sahte güncelleme yöntemi ile cevap vererek istediğimiz bir komutu NT AUTHORITY\SYSTEM hakları ile çalıştırmamızı sağlar.

Deneme yapmak için gereken Lab ortam bilgileri aşağıda belirtilmiştir:

- Saldırgan Sistem: Kali Linux 2021.2
- Hedef Sistem: Windows 10 1909
- Bettercap: Kali Terminalde “sudo apt install bettercap” komutu ile yükleyebilirsiniz.
- Pywsus Kali Terminalde “git clone https://github.com/GoSecure/pywsus.git” komutu ile github’dan programı indirebilir veya github linkinden zip dosyası olarak indirip kullanabilirsiniz. Sonrasında yine link üzerindeki installation kısmının altındaki gereksinimleri uygulayarak uygulamamızı hazır hale getiriyoruz.

**Gerçekleştirilecek saldırı senaryosu:**

Bu saldırı senaryosunda ağ üzerinden erişilebilen Windows 10 makinenin WSUS Server’dan güncelleme yükleme isteği sırasında trafiği kendi makinemize yönlendirerek ve sahte bir güncellemeyi araya sokarak hedef sistemimize yetkili kullanıcı olarak erişmeye çalışacağız, bunun için öncelikle ortadaki adam olup trafiğe müdahale edebilmemiz gerekiyor.

**Uygulama:**

Öncelikle saldırıda kullanacağımız Bettercap aracını Kali Linux’e yüklüyoruz.

![Untitled](Pywsus%20Arac%C4%B1%20ile%20WSUS%20Gu%CC%88ncellemesine%20Zararl%C4%B1%20Yaz%C4%B1%20fb8a445a571f4207a90cfb1997994e96/Untitled.png)

Yükleme gerçekleştikten sonra gerekli olan diğer aracımızı yani Pywsus aracını yüklüyoruz. Bunun için öncelikle masaüstüne geliyoruz ki dosyamızı masaüstüne indirelim.

![Untitled](Pywsus%20Arac%C4%B1%20ile%20WSUS%20Gu%CC%88ncellemesine%20Zararl%C4%B1%20Yaz%C4%B1%20fb8a445a571f4207a90cfb1997994e96/Untitled%201.png)

Sonra git clone komutu ile aracımızı github’dan indiriyoruz.

![Untitled](Pywsus%20Arac%C4%B1%20ile%20WSUS%20Gu%CC%88ncellemesine%20Zararl%C4%B1%20Yaz%C4%B1%20fb8a445a571f4207a90cfb1997994e96/Untitled%202.png)

Klasör indikten sonra klasörün içine gidiyorum komut satırında ve sırası ile aşağıdaki komutları çalıştırıyorum.

![Untitled](Pywsus%20Arac%C4%B1%20ile%20WSUS%20Gu%CC%88ncellemesine%20Zararl%C4%B1%20Yaz%C4%B1%20fb8a445a571f4207a90cfb1997994e96/Untitled%203.png)

Öncelikle işlemlere başlamadan önce Windows cihazımızı network üzerinde gördüğümüzden ve erişimimiz olduğundan emin oluyoruz ama zaten bizim senaryomuzda biz ağa sızdık ve Windows cihaza erişiyoruz ve WSUS serveri tespit edilmiş durumda, bu işlemler farklı bir yazının konusu olduğu için buraya girmiyoruz ve bu senaryodan devam ediyoruz, amacımız karşı makinede admin yetkisi ile komut çalıştırmak.

Öncelikle Bettercap ile bağlantı yapmak için hazırlıklarımızı yapıyoruz ve aşağıdaki bilgileri bir metin editöründe satır satır yazıp WSUS.cap olarak kaydediyoruz.

![Untitled](Pywsus%20Arac%C4%B1%20ile%20WSUS%20Gu%CC%88ncellemesine%20Zararl%C4%B1%20Yaz%C4%B1%20fb8a445a571f4207a90cfb1997994e96/Untitled%204.png)

Gerekli ayarlamalarımızı yaptıktan sonra artık bettercap aracımızı çalıştırıp hedef ip mizden çıkan 8530 portunu kullanan bütün istekleri kendimize yönlendirelim.

![Untitled](Pywsus%20Arac%C4%B1%20ile%20WSUS%20Gu%CC%88ncellemesine%20Zararl%C4%B1%20Yaz%C4%B1%20fb8a445a571f4207a90cfb1997994e96/Untitled%205.png)

- -iface parametresi ile bizim hedef makineye eriştiğimiz interfaceyi seçiyoruz, eğer bilmiyorsak bunu “ifconfig” komutu ile internet bilgilerimizi listeleyerek görebiliriz.
- -caplet parametresi ile gerekli ayarları ve komutları çekeceğimiz dosyayı belirliyoruz ki bu dosya bizim az önce hazırladığımız dosya.

Daha sonra indirip hazırlığını yaptığımız programı çalıştırıyoruz ve gelecek güncelleme isteklerini dinlemesi için bekletiyoruz.

![Untitled](Pywsus%20Arac%C4%B1%20ile%20WSUS%20Gu%CC%88ncellemesine%20Zararl%C4%B1%20Yaz%C4%B1%20fb8a445a571f4207a90cfb1997994e96/Untitled%206.png)

`python3 pywsus.py -H 10.0.2.6 -p 8530 -e PsExec64.exe -c '/accepteula /s cmd.exe /c "whoami > C:\\poc.txt" '`

Eğer hedefimiz biz bu şekilde dinlerken update yapmaya çalışırsa komut olarak yazdığımız cmd.exe’de çalıştırılacak whoami komutu C disk’in içerisinde poc.txt adında bir dosyaya yazdırılacak.

Güncelleme isteği geldiği zaman programımız güncelleme cevabı dönerek aksiyon aldı ve istediğimiz fonksiyonu yerine getirdi. Şimdi hedef makinemizi kontrol edelim bakalım neler olmuş.

![Untitled](Pywsus%20Arac%C4%B1%20ile%20WSUS%20Gu%CC%88ncellemesine%20Zararl%C4%B1%20Yaz%C4%B1%20fb8a445a571f4207a90cfb1997994e96/Untitled%207.png)

Komutu hangi kullanıcı hakları ile çalıştırdığımızı da poc.txt içinden görebiliyoruz.

![Untitled](Pywsus%20Arac%C4%B1%20ile%20WSUS%20Gu%CC%88ncellemesine%20Zararl%C4%B1%20Yaz%C4%B1%20fb8a445a571f4207a90cfb1997994e96/Untitled%208.png)

Dolayısıyla buradan şunu anlıyoruz ki biz cmd.exe yi çalıştırıp whoami komutunu girip çalıştırdığımızda biz bu komutu nt authority\system olarak çalıştırıyoruz ki bu kullanıcı admin yetkilerine sahip bir kullanıcıdır (aslinda "nt authority\system" ne bir kullanici, ne de bir gruptur. Security ID ismidir, servis hesaplarinin yetkilendirilmesi icin kullanilir, SID i baska kullanicilara veya servis hesaplarina eklenebilir ornegin. Gorev yoneticisinde "SYSTEM" olarak gorunur).

Yapabileceğimiz bir diğer şey ise tabii sisteme erişmeye çalışmak olacaktır ki bunu da komutta küçük bir değişiklik yaparak cmd.exe üzerinden powershell.exe’yi çalıştırmasını söyleriz, ardından whoami komutu çalıştırdığımız kısımda da [buradan](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#powershell) bulabileceğimiz powershell reverse shell komutlarını kendi IP adresimize göre düzenleyip çalıştırarak ve ardından belirlediğimiz portu netcat ile dinleyerek sisteme erişim sağlayabiliriz, sonrasında yapacaklarınız hayal gücünüze kalmış.
