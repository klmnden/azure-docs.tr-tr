<properties 
    pageTitle="IIS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu" 
    description="Bu, IIS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/12/2016" 
    ms.author="billmath"/>

# IIS Kimlik Doğrulaması

Azure Multi-Factor Authentication Sunucusu’nun IIS Kimlik Doğrulaması bölümü Microsoft IIS web uygulamaları ile IIS kimlik doğrulamasını etkinleştirmenizi ve yapılandırmanızı sağlar. Azure Multi-Factor Authentication Sunucusu, Azure Multi-Factor Authentication eklemek üzere IIS web sunucusuna yapılan istekleri filtreleyebilecek bir eklenti yükler. IIS eklentisi Form Tabanlı Kimlik Doğrulaması ve Tümleşik Windows HTTP Kimlik Doğrulaması için destek sağlar. Güvenilen IP’ler iç IP adreslerini iki öğeli kimlik doğrulamasından muaf tutmak için de kullanılabilir. 


![IIS Kimlik Doğrulaması](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## Azure Multi-Factor Authentication Sunucusu ile Form Tabanlı IIS Kimlik Doğrulaması kullanma

Form tabanlı kimlik doğrulaması kullanan bir IIS web uygulamasını güvenli hale getirmek için, IIS web sunucusuna Azure Multi-Factor Authentication Sunucusu yükleyin ve Sunucu’yu aşağıdaki yordama göre yapılandırın.

1. Azure Multi-Factor Authentication Sunucusu’nda, soldaki menüde IIS Kimlik Doğrulaması simgesine tıklayın.
2. Form Tabanlı sekmesine tıklayın.
3. Ekle düğmesine... tıklayın.
4. Kullanıcı adı, parola ve etki alanı değişkenlerini otomatik olarak algılamak için, Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusuna Oturum Açma URL’sini girin (örneğin, https://localhost/contoso/auth/login.aspx) ve Tamam’a tıklayın.
5. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, Multi-Factor Authentication İste kullanıcı eşleşme kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın.
6. Sayfa değişkenleri otomatik olarak algılanamaz ise, Otomatik Yapılandırma Form Tabanlı... Web Sitesi iletişim kutusunda Elle Belirt düğmesine tıklayın.
7. Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusunda, URL Gönder alanına oturum açma sayfası URL’sini girin ve Uygulama adı girin (isteğe bağlı). Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir. URL Gönder ayarlar hakkında daha fazla bilgi için yardım dosyasına bakın. 
8. Doğru İstek biçimini seçin. Bu çoğu web uygulaması için “YAYIMLA” ya da AL” olarak ayarlanır.
9. Kullanıcı adı değişkenini, Parola değişkenini ve Etki alanı değişkenini (oturum açma sayfasında görünürse) girin. Sayfadaki giriş kutularının adlarını bulmak için, bir web tarayıcısında oturum açma sayfasına gitmeniz, sayfada sağ tıklamanız ve “Kaynağı Görüntüle” seçeneğini seçmeniz gerekebilir.
10. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, Azure Multi-Factor Authentication İste kullanıcı eşleşme kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın. Bu özellik hakkında ek bilgi için yardım dosyasına bakın.
11.  Gelişmiş düğmesine... tıklayarak, özel bir reddetme sayfa dosyası seçebilme dahil gelişmiş ayarları görüntüleyin, tanımlama bilgilerini kullanarak web sitesine başarılı kimlik doğrulamalarını önbelleğe kaydedin ve birincil kimlik bilgileri için Windows Etki Alanı, LDAP dizini ya da RADIUS sunucusuna göre kimlik doğrulama gerçekleştirmeyi seçin. Tamamlandığında Form Tabanlı Web Sitesi Ekle iletişim kutusuna dönmek için Tamam düğmesine tıklayın. Gelişmiş ayarlar hakkında daha fazla bilgi için yardım dosyasına bakın.
12. Tamam düğmesine tıklayın.
13. URL ve sayfa değişkenleri algılandığında veya girildiğinde, web sitesi verileri Form Tabanlı panelde görüntülenir.
14. IIS kimlik doğrulaması yapılandırmasını tamamlamak için doğrudan aşağıdaki Azure Multi-Factor Authentication Sunucusu için IIS Eklentilerini Etkinleştirme bölümüne bakın. 

## Azure Multi-Factor Authentication Sunucusu ile Tümleşik Windows Kimlik Doğrulaması kullanma

Tümleşik Windows HTTP kimlik doğrulaması kullanan bir IIS web uygulamasını güvenli hale getirmek için, IIS web sunucusuna Azure Multi-Factor Authentication Sunucusu yükleyin ve Sunucu’yu aşağıdaki yordama göre yapılandırın. 

1. Azure Multi-Factor Authentication Sunucusu’nda, soldaki menüde IIS Kimlik Doğrulaması simgesine tıklayın.
2. HTTP sekmesine tıklayın. Form Tabanlı sekmesine tıklayın.
3. Ekle düğmesine... tıklayın.
4. Taban URL’si Ekle iletişim kutusuna, Taban URL’si alanına HTTP kimlik doğrulamasının gerçekleştirileceği web sitesi için URL’yi (örneğin, http://localhost/owa) girin ve Uygulama adının girin (isteğe bağlı). Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
5. Varsayılan yeterli değilse, Boşta kalma zaman aşımı ve Maksimum oturum sürelerini ayarlayın.
6. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, Multi-Factor Authentication İste kullanıcı eşleşme kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın. Bu özellik hakkında ek bilgi için yardım dosyasına bakın. 
7. İsterseniz tanımlama bilgisi önbellek kutusunu işaretleyin.
8. Tamam düğmesine tıklayın.
9. IIS kimlik doğrulaması yapılandırmasını tamamlamak için doğrudan aşağıdaki [Azure Multi-Factor Authentication Sunucusu için IIS Eklentilerini Etkinleştirme](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) bölümüne bakın. 


## Azure Multi-Factor Authentication Sunucusu için IIS Eklentilerini Etkinleştirme

Form Tabanlı ya da HTTP kimlik doğrulaması URL’lerini ve ayarlarını yapılandırdığınızda, Azure Multi-Factor Authentication IIS eklentilerinin IIS’de yüklenmesi ve etkinleştirilmesi gerektiği konumları seçmelisiniz. Aşağıdaki yordamı kullanın:

1. IIS 6’da çalıştırılıyorsa, bu site için Azure Multi-Factor Authentication ISAPI filtresi eklentisini etkinleştirmek üzere ISAPI sekmesine tıklayın ve web uygulamasının altında çalıştığı web sitesini seçin (örneğin, Varsayılan Web Sitesi).
2. IIS 7 veya üst sürümlerinde çalıştırılıyorsa, istenen düzeylerde IIS eklentisini etkinleştirmek için Yerel Modül sekmesine tıklayın ve sunucuyu, web sitelerini ya da uygulamaları seçin.
3. Ekranın üst kısmındaki IIS kimlik doğrulamasını etkinleştir kutusuna tıklayın. Azure Multi-Factor Authentication artık seçilen IIS uygulamasını güvenli hale getirir. Kullanıcıların sunucuya aktarıldığından emin olun. Bu konumlardan web sitesinde oturum açarken iki öğeli kimliklik doğrulamasının gerekli olmamasını sağlayacak şekilde iç IP adreslerini güvenilir listeye almak istiyorsanız, aşağıda Güvenilen IP’ler bölümüne bakın 


## Güvenilen IP'ler

Güvenilen IP'ler kullanıcıların belirli IP adresleri veya alt ağlardan kaynaklanan web sitesi istekleri için Azure Multi-Factor Authentication’ı atlamasına olanak tanır. Örneğin, ofisten oturum açarken kullanıcıların Azure Multi-Factor Authentication’dan muaf tutulmasını isteyebilirsiniz. Bunun için ofis alt ağını Güvenilen IP’ler girişi olarak belirtebilirsiniz. Güvenilen IP’leri yapılandırmak için aşağıdaki yordamı kullanın:

1. IIS Kimlik Doğrulaması bölümünde, Güvenilen IP'ler sekmesine tıklayın. 
2. Ekle düğmesine... tıklayın.
3. Güvenilen IP'leri Ekle iletişim kutusu göründüğünde, Tek bir IP, IP aralığı veya Alt ağ radyo düğmesini seçin.
4. Güvenilir listesinde olması gereken IP adresini, IP adresleri aralığını ya da alt ağı girin. Bir alt ağ giriyorsanız, uygun Ağ maskesini seçin ve Tamam düğmesine tıklayın. Güvenilir listesi şimdi eklendi.



<!----HONumber=Jun16_HO2-->


