<properties 
    pageTitle="RADIUS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu" 
    description="Bu, RADIUS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır." 
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



# RADIUS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu

RADIUS Kimlik Doğrulaması bölümü Azure Multi-Factor Authentication Sunucusu için RADIUS kimlik doğrulamasını etkinleştirmenizi ve yapılandırmanızı sağlar. RADIUS, kimlik doğrulama isteklerini kabul etmek ve bu istekleri işlemek için standart bir protokoldür. Azure Multi-Factor Authentication Sunucusu RADIUS sunucusu olarak hareket eder ve Azure Multi-Factor Authentication eklemek amacıyla RADIUS istemciniz (örneğin, VPN gereci) ile Active Directory (AD), LDAP dizini ya da başka bir RADIUS sunucusu olabilecek kimlik doğrulama hedefiniz arasına eklenir. Azure Multi-Factor Authentication’ın çalışması için Azure Multi-Factor Authentication Sunucusu’nu, hem istemci sunucuları hem de kimlik doğrulama hedefi ile iletişim kurabileceği şekilde yapılandırmalısınız. Azure Multi-Factor Authentication Sunucusu, RADIUS istemcisinden gelen istekleri kabul eder, kimlik bilgilerini kimlik doğrulama hedefine göre doğrular, Azure Multi-Factor Authentication ekler ve RADIUS istemcisine geri yanıt gönderir. Yalnızca birincil kimlik doğrulaması ve Azure Multi-Factor Authentication başarılı olursa, tüm kimlik doğrulama işlemi başarılı olur.

>[AZURE.NOTE]
>MFA sunucusu, RADIUS sunucusu olarak hareket ederken, yalnızca PAP (parola kimlik doğrulama protokolü) ve MSCHAPv2 (Microsoft Karşılıklı Kimlik Doğrulama Protokolü ) RADIUS protokollerini destekler.  MFA sunucusu, Microsoft NPS gibi bu protokolü destekleyen başka bir RADIUS sunucusuna RADIUS proxy olarak hareket ettiğinde, EAP (genişletilebilir kimlik doğrulama protokolü) gibi diğer protokoller kullanılabilir.
></br>
>Bu yapılandırmada diğer protokoller kullanılırken, MFA Sunucusu bu protokolü kullanarak başarılı bir RADIUS Sınama yanıtı başlatamayacağından, RADIUS tek yönlü SMS ve OATH belirteçleri çalışmaz


![Radius Kimlik Doğrulaması](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## RADIUS Kimlik Doğrulaması Yapılandırması

RADIUS kimlik doğrulamasını yapılandırmak için, bir Windows sunucusuna Azure Multi-Factor Authentication Sunucusu yükleyin. Bir Active Directory ortamınız varsa, sunucu ağ içindeki etki alanına eklenmelidir. Azure Multi-Factor Authentication Sunucusu’nu yapılandırmak için aşağıdaki yordamı uygulayın: 

1. Azure Multi-Factor Authentication Sunucusu’nda, soldaki menüde RADIUS Kimlik Doğrulaması simgesine tıklayın.
2. RADIUS kimlik doğrulamasını etkinleştir onay kutusunu işaretleyin.
3. Azure Multi-Factor Authentication RADIUS hizmetinin yapılandırılacak istemcilerden gelen RADIUS isteklerini dinlemek için standart olmayan bağlantı noktalarına bağlanması gerekliyse, İstemciler sekmesinde Kimlik Doğrulama bağlantı noktalarını ve Hesap bağlantı noktalarını değiştirin.
4. Ekle... düğmesine tıklayın.
5. RADIUS İstemcisi Ekle iletişim kutusuna, Azure Multi-Factor Authentication Sunucusu için kimlik doğrulaması yapacak gerecin/sunucunun IP adresini, bir Uygulama adı (isteğe bağlı) ve Paylaşılan gizlilik girin. Paylaşılan gizliliğin Azure Multi-Factor Authentication Sunucusu’nda ve gereçte/sunucuda aynı olması gerekir. Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
6. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve multi-factor authentication’a tabi olacaksa, Multi-Factor Authentication İste kullanıcı eşleşme kutusunu işaretleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın. Bu özellik hakkında ek bilgi için yardım dosyasına bakın.
7. Kullanıcılar Azure Multi-Factor Authentication mobil uygulama kimlik doğrulamasını kullanıyorsa ve bant dışı telefon araması, SMS ya da anında iletme bildirimine geri dönüş kimlik doğrulaması için OATH parolalarını kullanmak istiyorsanız, Yedek OATH belirtecini Etkinleştir kutusunu işaretleyin.
8. Tamam düğmesine tıklayın.
9. Ek RADIUS istemcileri eklemek için 4. adım-8. adıma kadarı tekrar edebilirsiniz.
10. Hedef sekmesine tıklayın.
11.  Azure Multi-Factor Authentication Sunucusu, Active Directory ortamında etki alanına katılmış bir sunucuda yüklüyse, Windows etki alanını seçin.
12. Kullanıcılara LDAP dizinine göre kimlik doğrulaması yapılması gerekiyorsa, LDAP bağlamayı seçin. LDAP bağlamayı kullanırken, Sunucu’nun dizininize bağlanabilmesi için Dizin Tümleştirme simgesine tıklamalı ve Ayarlar sekmesinde LDAP yapılandırmasını düzenlemeniz gerekir. LDAP yapılandırma yönergeleri LDAP Proxy yapılandırma kılavuzunda bulunabilir. 
13. Kullanıcılara başka bir RADIUS sunucusuna göre kimlik doğrulaması yapılması gerekiyorsa, RADIUS sunucularını seçin.
14. Sunucu’nun RADIUS isteklerini sunacağı sunucu yapılandırmak için Ekle düğmesine... tıklayın.
15. RADIUS Sunucusu Ekle iletişim kutusuna RADIUS sunucusu IP adresini ve bir paylaşılan gizlilik girin. Paylaşılan gizliliğin Azure Multi-Factor Authentication Sunucusu’nda ve RADIUS sunucusunda aynı olması gerekir. RADIUS sunucusu tarafından farklı bağlantı noktaları kullanılıyorsa, Kimlik Doğrulama bağlantı noktasını ve Hesap bağlantı noktasını değiştirin.
16. Tamam düğmesine tıklayın. 
17. Azure Multi-Factor Authentication Sunucusu’ndan buna gönderilen erişim isteklerini işleyecek şekilde, Azure Multi-Factor Authentication Sunucusu’nu başka bir RADIUS sunucusuna RADIUS istemcisi olarak eklemelisiniz. Azure Multi-Factor Authentication Sunucusu’nda yapılandırılanla aynı paylaşılan gizliliği kullanmalısınız.
18. Ek RADIUS sunucuları eklemek için bu adımı tekrarlayabilir ve Yukarı Taşı ve Aşağı Taşı düğmeleriyle Sunucu’nun bunları çağıracağı sırayı yapılandırabilirsiniz. Bu Azure Multi-Factor Authentication Sunucusu yapılandırmasını tamamlar. Sunucu artık yapılandırılan istemcilerden gelen RADIUS erişim istekleri için yapılandırılan bağlantı noktalarını dinler.   


## RADIUS İstemcisi Yapılandırması

RADIUS istemcisini yapılandırmak için yönergeleri kullanın:

- Gerecinizi/sunucunuzu, RADIUS sunucusu olarak hareket edecek Azure Multi-Factor Authentication Sunucusu’nun IP adresi için RADIUS aracılığıyla kimlik doğrulaması yapacak şekilde yapılandırın. 
- Yukarıda yapılandırılanla aynı paylaşılan gizliliği kullanın. 
- Kullanıcının kimlik bilgilerini doğrulama, multi-factor authentication gerçekleştirme, bunların yanıtını alma ve sonra RADIUS erişim isteğini yanıtlamaya zaman kalacak şekilde, RADIUS zaman aşımını 30-60 saniye olarak yapılandırın.




<!----HONumber=Jun16_HO2-->


