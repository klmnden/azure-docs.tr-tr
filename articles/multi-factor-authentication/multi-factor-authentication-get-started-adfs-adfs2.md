<properties 
    pageTitle="AD FS 2.0 ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirin." 
    description="Bu, nasıl Azure MFA ve AD FS 2.0 kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>
# AD FS 2.0 ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirin.

Kuruluşunuz Azure Active Directory ile birleştirildiyse ve bulutta ya da şirket içi güvenli hale getirmek istediğiniz kaynaklarınız varsa, bunu Azure Multi-Factor Authentication Sunucusu kullanarak ve onu yüksek değerli uç noktalar için multi-factor authentication tetiklenecek şekilde yapılandırarak yapabilirsiniz.

Bu belge AD FS 2.0 ile Multi-Factor Authentication Sunucusu kullanmayı ele alır.  Windows Server 2012 R2 AD FS ile Azure Multi-Factor Authentication kullanma hakkında bilgi edinmek için, bkz. [Windows Server 2012 R2 AD FS ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme](multi-factor-authentication-get-started-adfs-w2k12.md).


## AD FS 2.0 ara sunucusu
Ara sunucu ile AD FS 2.0 güvenliğini sağlamak için, ADFS ara sunucusunda Azure Multi-Factor Authentication Sunucusu’nu yükleyin ve aşağıdaki adımları kullanarak Sunucu’yu yapılandırın. 

### Bir ara sunucu ile AD FS 2.0’ı güvenli hale getirmek için

1. Azure Multi-Factor Authentication Sunucusu’nda, soldaki menüde IIS Kimlik Doğrulaması simgesine tıklayın.
2. Form Tabanlı sekmesine tıklayın.
3. Ekle düğmesine... tıklayın.
<center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Kullanıcı adı, parola ve etki alanı değişkenlerini otomatik olarak algılamak için, Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusuna Oturum Açma URL’sini girin (örneğin, https://sso.contoso.com/adfs/ls) ve Tamam’a tıklayın.
5. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, Azure Multi-Factor Authentication İste kullanıcı eşleşme kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın. Bu özellik hakkında ek bilgi için yardım dosyasına bakın.
6. Sayfa değişkenleri otomatik olarak algılanamaz ise, Otomatik Yapılandırma Form Tabanlı... Web Sitesi iletişim kutusunda Elle Belirt düğmesine tıklayın.
7. Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusunda, URL Gönder alanına (örneğin, https://sso.contoso.com/adfs/ls) ADFS oturum açma sayfası URL’sini girin ve Uygulama adı girin (isteğe bağlı). Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir. URL Gönder ayarlar hakkında daha fazla bilgi için yardım dosyasına bakın.
8. İstek biçimini "POST veya GET" olarak ayarlayın.
9. Kullanıcı adı değişkeni (ctl00$ContentPlaceHolder1$UsernameTextBox) ve Parola değişkenini (ctl00$ContentPlaceHolder1$PasswordTextBox) girin. Form tabanlı oturum açma sayfanız bir etki alanı metin kutusu görüntülerse, Etki alanı değişkenini de girin. Oturum açma sayfasındaki giriş kutularının adlarını bulmak için, bir web tarayıcısında oturum açma sayfasına gitmeniz, sayfada sağ tıklamanız ve “Kaynağı Görüntüle” seçeneğini seçmeniz gerekebilir.
10. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, Azure Multi-Factor Authentication İste kullanıcı eşleşme kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın.
<center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Gelişmiş düğmesine... tıklayarak, özel bir reddetme sayfa dosyası seçebilme dahil gelişmiş ayarları görüntüleyin, tanımlama bilgilerini kullanarak web sitesine başarılı kimlik doğrulamalarını önbelleğe kaydedin ve birincil kimlik bilgileri için kimlik doğrulamanın nasıl gerçekleştirileceğini seçin.
12. ADFS ara sunucusunun etki alanına katılması olası olmadığından, kullanıcı içeri aktarma ve ön kimlik doğrulama için etki alanı denetleyicisine bağlanmak için büyük olasılıkla LDAP kullanırsınız. Gelişmiş Form Tabanlı Web Sitesi iletişim kutusunda, Birincil Kimlik Doğrulama sekmesine tıklayın ve "LDAP Bağlama" için Ön Kimlik Doğrulama türünü seçin.
13. Tamamlandığında, Form Tabanlı Web Sitesi Ekle iletişim kutusuna dönmek için Tamam düğmesine tıklayın. Gelişmiş ayarlar hakkında daha fazla bilgi için yardım dosyasına bakın.
14. İletişim kutusunu kapatmak için Tamam düğmesine tıklayın.
15. URL ve sayfa değişkenleri algılandığında veya girildiğinde, web sitesi verileri Form Tabanlı panelde görüntülenir.
16. Yerel Modül sekmesine tıklayın ve IIS eklentisini istediğiniz düzeyde etkinleştirmek için ADFS ara sunucusunun altında çalıştığı web site (örneğin, “Varsayılan Web Sitesi”) sunucusunu veya ADFS ara sunucu uygulamasını (örneğin, “ls” altında “adfs”) seçin.
17. Ekranın üst kısmındaki IIS kimlik doğrulamasını etkinleştir kutusuna tıklayın.
18. IIS kimlik doğrulaması şimdi etkinleştirildi. Ancak, LDAP aracılığıyla Active Directory (AD) dizininiz için ön kimlik doğrulaması gerçekleştirmek üzere etki alanı denetleyicisine LDAP bağlantısını yapılandırmanız gerekir. Bunu yapmak için, Dizin Tümleştirme simgesine tıklayın.
19. Ayarlar sekmesinde, Özel LDAP yapılandırması kullan radyo düğmesini seçin.
<center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
20. Düzenle düğmesine... tıklayın.
21. LDAP Yapılandırmasını Düzenle iletişim kutusunda, AD etki alanı denetleyicisine bağlanmak için gerekli bilgilerle alanları doldurun. Alanların açıklamaları aşağıdaki tabloda yer almaktadır: Not: Bu bilgiler ayrıca Azure Multi-Factor Authentication Yardım dosyasında da bulunmaktadır.
22. Test Et düğmesine tıklayarak LDAP bağlantısını test edin.
<center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
23. LDAP bağlantısı testi başarılı olursa, Tamam düğmesine tıklayın.
24. Daha sonra, Şirket Ayarları simgesine tıklayın ve Kullanıcı Adı Çözümleme sekmesini seçin.
25. Kullanıcı adlarını eşleştirme radyo düğmesi için LDAP benzersiz tanımlayıcı özniteliğini seçin.
26. Kullanıcılar ADFS ara sunucusu oturum açma formunda kullanıcı adlarını “etki alanı\kullanıcı adı” biçiminde girerse, Sunucu LDAP sorgusu oluşturduğunda etki alanını kullanıcı adından çıkarabilmesi gerekir. Bu, bir kayıt defteri ayarı aracılığıyla yapılabilir.
27. Kayıt defteri düzenleyicisini açın ve 64 bit sunucuda HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/Positive Networks/PhoneFactor öğesine gidin. 32 bit sunucuda, “Wow6432Node” öğesini yoldan çıkarın. “UsernameCxz_stripPrefixDomain” adlı yeni bir DWORD kayıt defteri anahtarı oluşturun ve değeri 1 olarak ayarlayın. Azure Multi-Factor Authentication artık ADFS ara sunucusunu güvenli hale getirir. Kullanıcıların Active Directory’den Sunucuya aktarıldığından emin olun. Bu konumlardan web sitesinde oturum açarken iki öğeli kimlik doğrulamasının gerekli olmamasını sağlayacak şekilde iç IP adreslerini güvenilir listeye almak istiyorsanız, aşağıda Güvenilen IP’ler bölümüne bakın.

<center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## Ara sunucu olmadan AD FS 2.0 Direct

AD FS ara sunucusu kullanılmadığında AD FS’yi güvenli hale getirmek için, AD FS sunucusunda Azure Multi-Factor Authentication Sunucusu’nu yükleyin ve aşağıdaki adımları kullanarak Sunucu’yu yapılandırın. 

### Ara sunucu olmadan AD FS 2.0’ı güvenli hale getirmek için
1. Azure Multi-Factor Authentication Sunucusu’nda, soldaki menüde IIS Kimlik Doğrulaması simgesine tıklayın.
2. HTTP sekmesine tıklayın.
3. Ekle düğmesine... tıklayın.
4. Taban URL’si Ekle iletişim kutusuna, Taban URL’si alanına HTTP kimlik doğrulamasının gerçekleştirileceği ADFS web sitesi için URL’yi (örneğin, https://sso.domain.com/adfs/ls/auth/integrated) girin ve Uygulama adının girin (isteğe bağlı). Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
5. İsterseniz, Boşta kalma zaman aşımı ve Maksimum oturum sürelerini ayarlayın.
6. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, Azure Multi-Factor Authentication İste kullanıcı eşleşme kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın. Bu özellik hakkında ek bilgi için yardım dosyasına bakın.
7. İsterseniz tanımlama bilgisi önbellek kutusunu işaretleyin.
<center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Tamam düğmesine tıklayın.
9. Yerel Modül sekmesine tıklayın ve IIS eklentisini istediğiniz düzeyde etkinleştirmek için ADFS’nin altında çalıştığı web site (örneğin, “Varsayılan Web Sitesi”) sunucusunu veya ADFS uygulamasını (örneğin, “ls” altında “adfs”) seçin.
10. Ekranın üst kısmındaki IIS kimlik doğrulamasını etkinleştir kutusuna tıklayın. Azure Multi-Factor Authentication artık ADFS’yi güvenli hale getirir. Kullanıcıların Active Directory’den Sunucuya aktarıldığından emin olun. Bu konumlardan web sitesinde oturum açarken iki öğeli kimlik doğrulamasının gerekli olmamasını sağlayacak şekilde iç IP adreslerini güvenilir listeye almak istiyorsanız, aşağıdaki Güvenilen IP’ler bölümüne bakın.


## Güvenilen IP'ler
Güvenilen IP'ler kullanıcıların belirli IP adresleri veya alt ağlardan kaynaklanan web sitesi istekleri için Azure Multi-Factor Authentication’ı atlamasına olanak tanır. Örneğin, ofisten oturum açarken kullanıcıların Azure Multi-Factor Authentication’dan muaf tutulmasını isteyebilirsiniz. Bunun için ofis alt ağını Güvenilen IP’ler girişi olarak belirtebilirsiniz. 

### Güvenilen IP'leri yapılandırmak için


1. IIS Kimlik Doğrulaması bölümünde, Güvenilen IP'ler sekmesine tıklayın.
1. Ekle düğmesine... tıklayın.
1. Güvenilen IP'leri Ekle iletişim kutusu göründüğünde, Tek bir IP, IP aralığı veya Alt ağ radyo düğmesini seçin.
1. Güvenilir listesinde olması gereken IP adresini, IP adresleri aralığını ya da alt ağı girin. Bir alt ağ giriyorsanız, uygun Ağ maskesini seçin ve Tamam düğmesine tıklayın. Güvenilen IP şimdi eklendi.


<center>![Kurulum](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>

 



<!--HONumber=Aug16_HO4-->


