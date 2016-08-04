<properties 
    pageTitle="Azure Multi-Factor Authentication Sunucusu’nu kullanmaya başlama" 
    description="Bu, nasıl Azure MFA Sunucusu kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır." 
    services="multi-factor-authentication"
    keywords="authentication server, azure multi factor authentication app activation page, authentication server download" 
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

# Azure Multi-Factor Authentication Sunucusu’nu kullanmaya başlama




<center>![Bulut](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Artık şirket içi multi-factor authentication kullanıp kullanmayacağımıza karar verdiğimize göre, devam edebiliriz. Bu sayfa yeni bir sunucu yüklemeyi ve şirket içi Active Directory’de kurulumunu yapmayı ele alır. Zaten yüklü PhoneFactor sunucunuz varsa ve yükseltmek istiyorsanız, bkz. [Azure Multi-Factor Sunucusu’na yükseltme](multi-factor-authentication-get-started-server-upgrade.md) ya da yalnızca web hizmetini yüklemeye ilişkin bilgi arıyorsanız, bkz. [Azure Multi-Factor Authentication Sunucusu Mobil Uygulama Web Hizmeti’ni dağıtma](multi-factor-authentication-get-started-server-webservice.md).


## Azure Multi-Factor Authentication Sunucusu’nu indirme



Azure Multi-Factor Authentication Sunucusu’nu indirmenin iki farklı yolu vardır. Her ikisi de Azure portal aracılığıyla yapılır. Birinci yol Multi-Factor Auth Sağlayıcısı’nı doğrudan yöneterek yapılandır. İkinci yol hizmet ayarları aracılığıyla yapılandır. İkinci seçenek Multi-Factor Auth Sağlayıcısı ya da Azure MFA, Azure AD Premium veya Enterprise Mobility Suite lisansı gerektirir.


### Azure portaldan Azure Multi-Factor Authentication Sunucusu’nu indirmek için
--------------------------------------------------------------------------------

1. Azure Portal’da yönetici olarak oturum açın.
2. Solda, Active Directory'yi seçin.
3. Active Directory sayfasında, en üstte **Multi-Factor Auth Sağlayıcıları**’na tıklayın.
4. Alt kısımda, **Yönet**’e tıklayın.
5. Bu yeni bir sayfa açar.  **İndirmeler**’e tıklayın
![İndir](./media/multi-factor-authentication-sdk/download.png)
6. **Etkinleştirme Kimlik Bilgileri Oluştur**’un üstünde, **İndir**’e tıklayın.
![İndir](./media/multi-factor-authentication-get-started-server/download4.png)
7. İndirmeyi kaydedin.



### Hizmet ayarları aracılığıyla Azure Multi-Factor Authentication Sunucusu’nu indirmek için


1. Azure Portal’da yönetici olarak oturum açın.
2. Solda, Active Directory'yi seçin.
3. Azure AD örneğinize çift tıklayın.
4. En üstte, **Yapılandır**’a tıklayın.
![İndir](./media/multi-factor-authentication-sdk/download2.png)
5. Multi-factor authentication altında, **Hizmet ayarlarını yönet**'i seçin.
6. Hizmetleri ayarları sayfasında, ekranın alt kısmında **Portal'a git**’e tıklayın.
![İndir](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Bu yeni bir sayfa açar. **İndirmeler**’e tıklayın,
8. **Etkinleştirme Kimlik Bilgileri Oluştur**’un üstünde, **İndir**’e tıklayın.
9. İndirmeyi kaydedin.




## Azure Multi-Factor Authentication Sunucusu’nu Yükleme ve Yapılandırma
Artık sunucuyu indirdiğinize göre, yükleyebilir ve yapılandırabilirsiniz.  Yükleme yaptığınız sunucunun aşağıdaki gereksinimleri karşıladığından emin olun:



Azure Multi-Factor Authentication Sunucusu Gereksinimleri|Açıklama|
:------------- | :------------- | 
Donanım|<li>200 MB boş sabit disk alanı</li><li>x32 veya x64 özellikli işlemci</li><li>1 GB veya daha fazla RAM</li>
Yazılım|<li>Windows Server 2008 veya üst sürümü, ana bilgisayar sunucu işletim sistemi ise</li><li>Windows 7 veya üst sürümü, ana bilgisayar istemci işletim sistemi ise</li><li>Microsoft .NET 4.0 Framework</li><li>IIS 7.0 veya üst sürümü, kullanıcı portalı veya web hizmeti SDK’sı yüklüyorsanız</li>

### Azure Multi-Factor Authentication Sunucusu güvenlik duvarı gereksinimleri
--------------------------------------------------------------------------------
Her MFA sunucusunun bağlantı noktası 443’te aşağıdakilere giden iletişim kurabilmesi gerekir:

- https://pfd.phonefactor.net
- https://pfd2.phonefactor.net
- https://css.phonefactor.net

Giden güvenlik duvarları bağlantı noktası 443’te kısıtlı ise, aşağıdaki IP adresi aralıklarının açılması gerekir:

IP Alt ağı|Ağ maskesi|IP aralığı
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Azure Multi-Factor Authentication Olay Onayı özelliğini kullanmıyorsanız ve kullanıcılar kurumsal ağdaki cihazlarda multi-Factor Auth mobile apps ile kimlik doğrulaması yapmıyorsa, IP aralıkları aşağıdakilere düşürülebilir:


IP Alt ağı|Ağ maskesi|IP aralığı
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### Azure Multi-Factor Authentication Sunucusu’nu yüklemek ve yapılandırma k için
--------------------------------------------------------------------------------


1. Yürütülebilir dosya üzerine çift tıklayın. Bu yükleme işlemini başlatır.
2. Yükleme Klasörünü Seçin ekranında, klasörün doğru olduğundan emin olun ve İleri’ye tıklayın.
3. Yükleme tamamlandığında, Son'a tıklayın.  Bu yapılandırma sihirbazını başlatır.
4. Yapılandırma sihirbazı karşılama ekranında, **Kimlik Doğrulaması Yapılandırma Sihirbazı kullanmayı atla** seçeneği işaretleyin ve **İleri**’ye tıklayın.  Bu sihirbazı kapatır ve sunucuyu başlatır.
![Bulut](./media/multi-factor-authentication-get-started-server/skip2.png)

5. Sunucuyu indirdiğimiz sayfaya dönerek, **Etkinleştirme Kimlik Bilgileri Oluştur** düğmesine tıklayın.  Bu bilgileri verilen kutularda Azure MFA Sunucusu’na kopyalayın ve **Etkinleştir**’e tıklayın.


Yukarıdaki adımlar yapılandırma sihirbazı ile hızlı kurulumu gösterir.  Sunucudaki Araçlar menüsünde seçerek kimlik doğrulama sihirbazını yeniden çalıştırabilirsiniz.



##Kullanıcıları Active Directory'den içeri aktarma

Artık sunucu yüklendiğine ve yapılandırıldığına göre, hızlı bir şekilde kullanıcıları Azure MFA Sunucusu’na aktarabilirsiniz 

### Kullanıcıları Active Directory'den içeri aktarmak için
--------------------------------------------------------------------------------


1. Azure MFA Sunucusu’nda, solda, **Kullanıcılar**’ı seçin.
2. Alt kısımda, **Active Directory’den içeri aktar**’ı seçin.
3. Artık kullanıcıları tek tek arayabilir ya da içindeki kullanıcılarla birlikte OU’lar için AD dizininde arama yapabilirsiniz.  Bu durumda, biz kullanıcıların OU’sunu belirtiriz.
4. Sağdaki tüm kullanıcıları vurgulayın ve **İçeri Aktar**’a tıklayın.  Başarılı olduğunuzu belirten bir açılır pencere görmeniz gerekir.  İçeri aktarma penceresini kapatın.

![Bulut](./media/multi-factor-authentication-get-started-server/import2.png)

## Kullanıcılara e-posta gönderme
Artık kullanıcılarınızı Azure Multi-Factor Authentication sunucusuna aktardığınıza göre, multi-factor authentication için kaydedildikleri konusunda kullanıcılarınızı bilgilendirmek için onlara e-posta göndermeniz tavsiye edilir.

Azure Multi-Factor Authentication Sunucusu ile multi-factor authentication için kullanıcılarınızı yapılandırmanın çeşitli yolları vardır.  Örneğin, kullanıcıların telefon numarasını biliyorsanız ya da telefon numaralarını şirket dizinlerinden Azure Multi-Factor Authentication Sunucusu’na aktarabildiyseniz, e-posta kullanıcılara Azure Multi-Factor Authentication kullanmak üzere yapılandırıldıklarını bildirir, Azure Multi-Factor Authentication kullanımına ilişkin bazı yönergeler verir ve telefon numarası kullanıcısını kimlik doğrulamalarını alacakları konusunda bilgilendirir.  

E-posta içeriği kullanıcı için ayarlanan kimlik doğrulama yöntemine bağlı olarak farklılık gösterir (örneğin telefon araması, SMS, mobil uygulama).  Örneğin, kullanıcının kimlik doğrularken PIN kullanması gerekiyorsa, e-posta kullanıcıya ilk PIN’ini bildirir.  Kullanıcıların ilk kimlik doğrulaması sırasında genellikle kendi PIN'lerini değiştirmesi gerekir.

Kullanıcıların telefon numaraları yapılandırılmamış ya da Azure Multi-Factor Authentication Sunucusu’na aktarılmamış ise veya kullanıcılar kimlik doğrulama için mobil uygulama kullanmak üzere önceden yapılandırılmışsa, kullanıcılara Azure Multi-Factor Authentication’ı kullanmak üzere yapılandırıldıklarını ve bunun kendilerini Azure Multi-Factor Authentication Kullanıcı Portalı aracılığıyla hesap kaydını tamamlamak üzere yönlendireceği konusunda bilgilendiren bir e-posta gönderebilirsiniz.  Kullanıcı Portalı’na erişim için kullanıcının tıklayacağı bir köprü de e-postaya eklenir. Kullanıcı köprüye tıkladığında, web tarayıcısı açılır ve kendisini şirkete ait Azure Multi-Factor Authentication Kullanıcı Portalı’na götürür.   


### E-posta ve e-posta şablonlarını yapılandırma

Soldaki e-posta simgesine tıklayarak bu e-postaları gönderme ayarlarını yapabilirsiniz.  Burada, posta sunucunuzun SMTP bilgilerini girebilirsiniz, bu da Kullanıcılara e-posta gönder onay kutusuna bir işaret ekleyerek geniş kapsamlı e-posta göndermenizi sağlar.

![E-posta Ayarları](./media/multi-factor-authentication-get-started-server/email1.png)

E-posta İçeriği sekmesinde, seçim yapabileceğiniz çeşitli e-posta şablonlarının tümünü görürsünüz.  Böylece, kullanıcılarınızı multi-factor authentication kullanmak üzere nasıl yapılandırdığınıza bağlı olarak, size en uygun şablonu seçebilirsiniz.

![E-posta şablonları](./media/multi-factor-authentication-get-started-server/email2.png)

## Azure Multi-Factor Authentication Sunucusu kullanıcı verileri nasıl işler?

Şirket içi Multi-Factor Authentication (MFA) Sunucusu kullandığınızda, kullanıcının verileri şirket içi sunucularda depolanır. Kalıcı kullanıcı verileri bulutta depolanmaz. Kullanıcı iki öğeli kimlik doğrulaması gerçekleştirdiğinde, MFA Sunucusu kimlik doğrulama gerçekleştirmek üzere verileri Azure MFA bulut hizmetine gönderir. Bu kimlik doğrulama istekleri bulut hizmetine gönderildiğinde, aşağıdaki alanlar istekte ve günlüklerde gönderilir, böylece bunlar müşterinin kimlik doğrulama/kullanım raporlarında kullanılabilir. Multi-Factor Authentication Sunucusu’nda etkinleştirilebilecek ya da devre dışı bırakılabilecek şekilde, bazı alanlar isteğe bağıldır. MFA Sunucusu’ndan MFA bulut hizmetlerine iletişim 443 giden bağlantı noktası üzerinden SSL/TLS kullanır. Bu alanlar aşağıdaki gibidir:

- Benzersiz Kimlik - kullanıcı adı veya iç MFA sunucusu kimliği
- Ad ve Soyadı - isteğe bağlı
- E-posta Adresi - isteğe bağlı
- Telefon Numarası - sesli arama veya SMS kimlik doğrulaması yapılırken
- Cihaz belirteci - Mobil uygulama kimlik doğrulaması yaparken
- Kimlik Doğrulaması Modu 
- Kimlik Doğrulaması Sonucu 
- MFA Sunucusu Adı 
- MFA Sunucusu IP’si 
- İstemci IP’si - varsa



Yukarıdaki alanlara ek olarak, kimlik doğrulaması sonucu (başarılı/reddedildi) ve reddetme nedeni kimlik doğrulama verileriyle birlikte depolanır ve kimlik doğrulama/kullanım raporlarıyla kullanıma sunulur.


## Gelişmiş Azure Multi-Factor Authentication Sunucusu Yapılandırmaları
Gelişmiş kurulum ve yapılandırma bilgisi hakkında ek bilgiler için aşağıdaki tabloyu kullanın.

Yöntem|Açıklama
:------------- | :------------- | 
[Kullanıcı Portalı](multi-factor-authentication-get-started-portal.md)|  Dağıtım ve kullanıcı self servis dahil Kullanıcı portalı kurulumu ve yapılandırması hakkında bilgiler.
[Active Directory Federasyon Hizmeti](multi-factor-authentication-get-started-adfs.md)|AD FS ile Azure Multi-Factor Authentication kurulumu hakkında bilgiler
[RADIUS Kimlik Doğrulaması](multi-factor-authentication-get-started-server-radius.md)|  RADIUS ile Azure MFA Sunucusu kurulumu ve yapılandırması hakkında bilgiler.
[IIS Kimlik Doğrulaması](multi-factor-authentication-get-started-server-iis.md)|IIS ile Azure MFA Sunucusu kurulumu ve yapılandırması hakkında bilgiler.
[Windows Kimlik Doğrulaması](multi-factor-authentication-get-started-server-windows.md)|  Windows Kimlik Doğrulaması ile Azure MFA Sunucusu kurulumu ve yapılandırması hakkında bilgiler.
[LDAP Kimlik Doğrulaması](multi-factor-authentication-get-started-server-ldap.md)|LDAP Kimlik Doğrulaması ile Azure MFA Sunucusu kurulumu ve yapılandırması hakkında bilgiler.
[RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](multi-factor-authentication-get-started-server-rdg.md)|  RADIUS kullanan Uzak Masaüstü Ağ Geçidi ile Azure MFA Sunucusu kurulumu ve yapılandırması hakkında bilgiler.
[Windows Server Active Directory ile eşitleme](multi-factor-authentication-get-started-server-dirint.md)|Active Directory ile Azure MFA Sunucusu arasındaki eşitleme kurulumu ve yapılandırması hakkında bilgiler.
[Azure Multi-Factor Authentication Sunucusu Mobil Uygulama Web Hizmeti’ni dağıtma](multi-factor-authentication-get-started-server-webservice.md)|Azure MFA sunucusu web hizmeti kurulumu ve yapılandırması hakkında bilgiler.



<!----HONumber=Jun16_HO2-->


