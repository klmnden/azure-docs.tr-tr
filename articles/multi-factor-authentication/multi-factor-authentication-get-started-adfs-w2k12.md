<properties 
    pageTitle="Windows Server 2012 R2 AD FS ile Azure MFA Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme" 
    description="Bu, Windows Server 2012 R2’de nasıl Azure MFA ve AD FS kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır." 
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
    ms.date="05/12/2016" 
    ms.author="billmath"/>


# Windows Server 2012 R2 AD FS ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme

Kuruluşunuz Azure AD ile birleştirildiyse ve bulutta ya da şirket içi güvenli hale getirmek istediğiniz kaynaklarınız varsa, bunu Azure Multi-Factor Authentication Sunucusu kullanarak ve onu yüksek değerli uç noktalar için multi-factor authentication tetiklenecek şekilde yapılandırarak yapabilirsiniz.

Bu belge Windows Server 2012 R2’de AD FS ile Multi-Factor Authentication Sunucusu kullanmayı ele alır.  AD FS 2.0 ile Azure Multi-Factor Authentication kullanma hakkında bilgi edinmek için, bkz. [AD FS 2.0 ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme](multi-factor-authentication-get-started-adfs-adfs2.md)

## Azure Multi-Factor Authentication Sunucusu ile Windows Server 2012 R2 AD FS’yi güvenli hale getirme

Azure Multi-Factor Authentication Sunucusu’nu yüklerken aşağıdaki iki seçeneğe sahip olursunuz:

- Azure Multi-Factor Authentication Sunucusu’nu yerel olarak AD FS ile aynı sunucuya yükleme 
- Azure Multi-Factor Authentication Bağdaştırıcısı’nı yerel olarak AD FS Sunucusu’na yükleme ve farklı bir bilgisayara MFA Sunucusu yükleme

Başlamadan önce, aşağıdaki bilgileri unutmayın:

- Azure Multi-Factor Authentication Sunucusu’nun AD FS federasyon sunucunuza yüklenmesi gerekli değildir ancak AD FS için Multi-Factor Authentication Bağdaştırıcısı’nın AD FS çalıştıran bir Windows Server 2012 R2’ye yüklenmesi gerekir. Desteklenen bir sürüm olduğu sürece sunucuyu farklı bir bilgisayara yükleyebilirsiniz ve AD FS bağdaştırıcısını ayrı olarak AD FS federasyon sunucusuna yükleyebilirsiniz. Bağdaştırıcıyı ayrı olarak yükleme yönergeleri için aşağıdaki yordama bakın.
- Multi-Factor Authentication Sunucusu’nun AD FS Bağdaştırıcısı tasarlanırken, AD FS’nin bir uygulama adı olarak kullanılabilecek bağlı olan taraf adını bağdaştırıcıya geçirebileceği öngörülmüştür.  Ancak, bu durumun gerçekleşmediği görülmüştür.  Metin mesajı ya da mobil uygulama kimlik doğrulama yöntemlerini kullanıyorsanız, Şirket Ayarları’nda tanımlanan dizeler "<$application_name$>" şeklinde bir yer tutucu içerir.  Bu yer tutucu, AD FS bağdaştırıcısı kullanılırken değiştirilmez.  Bu nedenle, AD FS güvenli hale getirilirken uygun dizelerden yer tutucunun kaldırılması önerilir.

- Oturum açılan hesabın Active Directory’de güvenlik grupları açmak için ayrıcalıkları olmalıdır.

- Multi-Factor Authentication AD FS Bağdaştırıcısı yükleme sihirbazı Active Directory’nizde PhoneFactor Admins adlı bir güvenlik grubu oluşturur ve sonra federasyon hizmetinizin AD FS hizmeti hesabını bu gruba ekler. Etki alanı denetleyicinizde PhoneFactor Admins grubunun gerçekten oluşturulduğunu ve AD FS hizmeti hesabının bu grubun bir üyesi olduğunu doğrulamanız önerilir. Gerekirse, AD FS hizmeti hesabınızı etki alanı denetleyicinizde PhoneFactor Admins grubuna el ile ekleyin.
- Kullanıcı portalıyla Hizmet SDK’sını yükleme hakkında bilgi edinmek için, bkz. [ Azure Multi-Factor Authentication Sunucusu için kullanıcı portalını dağıtma.](multi-factor-authentication-get-started-portal.md)
  

### Azure Multi-Factor Authentication Sunucusu’nu yerel olarak AD FS ile aynı sunucuya yüklemek için

1. Azure Multi-Factor Authentication Sunucusu’nı AD FS federasyon sunucunuza indirin ve yükleyin. Azure Multi-Factor Authentication sunucusunu yükleme hakkında bilgi edinmek için bkz. [Azure Multi-Factor Authentication Sunucusu’nu kullanmaya başlama](multi-factor-authentication-get-started-server.md).
2. Azure Multi-Factor Authentication Sunucusu kullanıcı arabiriminde, AD FS simgesini seçin ve Kullanıcı kaydına izin ver ve Kullanıcıların yöntemi seçmesine izin ver seçeneklerini seçin.
3. Ek seçenekleri seçin.
4. AD FS Bağdaştırıcısı’nı Yükle'ye tıklayın.
<center>![Bulut](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Bilgisayar bir etki alanına katıldıysa ve AD FS Bağdaştırıcısı ile Multi-Factor Authentication hizmeti arasındaki hizmeti güvenli hale getirmek üzere Active Directory yapılandırması tamamlanmamışsa, Active Directory adımı görüntülenir.  Bu yapılandırmayı otomatik olarak tamamlamak için İleri düğmesine tıklayın ya da Otomatik Active Directory yapılandırmasını atla ve ayarları el ile yapılandır onay kutusunu işaretleyin ve İleri’ye tıklayın.
6. Bilgisayar bir etki alanına katılmadıysa ve AD FS Bağdaştırıcısı ile Multi-Factor Authentication hizmeti arasındaki hizmeti güvenli hale getirmek üzere Yerel Grup yapılandırması tamamlanmamışsa, Yerel Grup adımı görüntülenir.  Bu yapılandırmayı otomatik olarak tamamlamak için İleri düğmesine tıklayın ya da Otomatik Yerel Grup yapılandırmasını atla ve ayarları el ile yapılandır onay kutusunu işaretleyin ve İleri’ye tıklayın.
7. Bu, yükleme sihirbazını getirir, Azure Multi-Factor Authentication Sunucusu’nun PhoneFactor Admins grubu oluşturmasına AD FS hizmeti hesabını PhoneFactor Admins grubuna eklemesine izin vermek için İleri’ye tıklayın.
<center>![Bulut](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Yükleyiciyi Başlat adımında, İleri’ye tıklayın.
9. Multi-Factor Authentication AD FS Bağdaştırıcısı yükleyicisinde, İleri’ye tıklayın.
10. Yükleme tamamlandığında Kapat'a tıklayın.
11. Artık bağdaştırıcı yüklendiğine göre, AD FS’ye kaydedilmelidir.  Windows PowerShell’i açın ve şunu çalıştırın: C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 <center>![Bulut](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Şimdi henüz kaydettiğimiz bağdaştırıcıyı kullanmak için AD FS’deki Genel Kimlik Doğrulama İlkesi’ni düzenlemeliyiz. AD FS Yönetim Konsolu'nda, Kimlik Doğrulama İlkeleri’ne gidin ve Multi-factor Authentication bölümünün altında, Genel Ayarlar alt bölümünün yanındaki Düzenle bağlantısına tıklayın. Genel Kimlik Doğrulama İlkesini Düzenle penceresinde, ek kimlik doğrulama yöntemi olarak Multi-Factor Authentication’ı seçin ve ardından Tamam'a tıklayın. Bağdaştırıcı WindowsAzureMultiFactorAuthentication olarak kaydedilir.  Kaydın etkili olması için AD FS hizmetini yeniden başlatmalısınız.

<center>![Bulut](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Bu noktada, Multi-Factor Authentication Sunucusu, AD FS ile birlikte kullanım amacıyla ek kimlik doğrulama sağlayıcısı olmak üzere kurulur.

## Web Hizmeti SDK’sını kullanarak AD FS Bağdaştırıcısı’nı tek başına yüklemek için
1. Web Hizmeti SDK’sını Multi-Factor Authentication Sunucusu çalıştıran sunucuya yükleyin.
2. \Program Files\Multi-Factor Authentication Server dizinindeki MultiFactorAuthenticationAdfsAdapterSetup64.msi, Register-MultiFactorAuthenticationAdfsAdapter.ps1, Unregister-MultiFactorAuthenticationAdfsAdapter.ps1 ve MultiFactorAuthenticationAdfsAdapter.config dosyalarını AD FS Bağdaştırıcısı’nı yüklemeyi planladığınız sunucuya kopyalayın.
3. MultiFactorAuthenticationAdfsAdapterSetup64.msi dosyasını çalıştırın.
4. Yükleme işlemini gerçekleştirmek için Multi-Factor Authentication AD FS Bağdaştırıcısı yükleyicisinde, İleri’ye tıklayın.
5. Yükleme tamamlandığında Kapat düğmesine tıklayın.
6. Aşağıdakileri yaparak MultiFactorAuthenticationAdfsAdapter.config dosyasını düzenleyin:

MultiFactorAuthenticationAdfsAdapter.config Adımı| Alt adım
:------------- | :------------- |
UseWebServiceSdk düğümünü true olarak ayarlayın.||
WebServiceSdkUrl’yi Multi-Factor Authentication Web Hizmeti SDK’sını URL’sine ayarlayın.||
1. Seçenek - Web Hizmeti SDK’sını bir kullanıcı adı ve parola kullanarak yapılandırın.|<ol><li>WebServiceSdkUsername’i, PhoneFactor Admins güvenlik grubunun üyesi olan bir hesaba ayarlayın.  <domain>\<kullanıcı adı> biçimini kullanın<li>WebServiceSdkPassword’u uygun hesap parolası olarak ayarlayın.</li></ol>
2. Seçenek - Web Hizmeti SDK’sını bir istemci sertifikası ile yapılandırın.|<ol><li>Web Hizmeti SDK’sı çalıştıran sunucu için sertifika yetkilisinden bir istemci sertifikası alın.  Sertifika alma hakkında bilgi için bkz. [İstemci Sertifikası Alma](https://technet.microsoft.com/library/cc770328(v=ws.10).aspx).</li><li>İstemci sertifikasını Web Hizmeti SDK’sı çalıştıran sunucudaki yerel bilgisayar Kişisel sertifika deposuna aktarın.  Not: sertifika yetkilisinin genel sertifikasının Güvenilen Kök Sertifikalarda olduğundan emin olun.</li><li>İstemci sertifikasının ortak ve özel anahtarlarını bir .pfx dosyasına aktarın.</li><li>Base-64 biçimindeki ortak anahtarı bir .cer dosyasına aktarın.</li><li>Sunucu Yöneticisi'nde, Web Server (IIS)\Web Server\Security\IIS İstemci Sertifikası Eşleme Kimlik doğrulaması özelliğinin yüklü olduğunu doğrulayın.</li><li>Yüklü değilse, bu özelliğe eklemek üzere Rol ve Özellik Ekle’yi seçin.</li><li>IIS Yöneticisi'nde, Web Hizmeti SDK’sı sanal dizinini içeren web sitesinde Yapılandırma Düzenleyicisi'ne çift tıklayın.  Not: Bunu sanal dizin düzeyinde değil web sitesi düzeyinde yapmanız çok önemlidir.</li><li>System.webServer/security/authentication/iisClientCertificateMappingAuthentication bölümüne gidin.</li><li>Etkin’i true olarak ayarlayın.</li><li>oneToOneCertificateMappingsEnabled’ı true olarak ayarlayın.</li><li>oneToOneMappings yanındaki ... düğmesine tıklayın.</li><li>Ekle bağlantısına tıklayın.</li><li>Önceden dışarı aktardığınız base-64 .cer dosyasını açın.  -----BEGIN CERTIFICATE-----, -----END CERTIFICATE----- ifadelerini ve tüm satır sonlarını kaldırın.  Sonuç dizesini kopyalayın.</li><li>Önceki adımda kopyaladığınız dizeye sertifikayı ayarlayın.</li><li>Etkin’i true olarak ayarlayın.</li><li>userName değerini PhoneFactor Admins güvenlik grubunun üyesi olan bir hesaba ayarlayın.  <domain>\<kullanıcı adı> biçimini kullanın</li><li>Parolayı uygun hesap parolası olarak ayarlayın.</li><li>Koleksiyon Düzenleyicisi’ni kapatın.</li><li>Uygula bağlantısına tıklayın.</li><li>Web Hizmeti SDK’sı sanal dizinine gidin.</li><li>Kimlik Doğrulaması'na çift tıklayın.</li><li>ASP.NET Kimliğe Bürünme ve Temel Kimlik Doğrulaması’nın Etkin olduğunu ve diğer tüm öğelerin Devre Dışı olduğunu doğrulayın.</li><li>Web Hizmeti SDK’sı sanal dizinine tekrar gidin.</li><li>SSL Ayarları'na çift tıklayın.</li><li>İstemci Sertifikaları’nı Kabul Et olarak ayarlayın ve Uygula’ya tıklayın.</li><li>Önceden dışarı aktarılan .pfx dosyasını AD FS Bağdaştırıcısı çalıştıran sunucuya kopyalayın.</li><li>.pfx dosyasını yerel bilgisayar Kişisel sertifika deposuna aktarın.</li><li>Sağ tıklama menüsünde Özel Anahtarları Yönet’i seçin ve Active Directory Federasyon Hizmetleri hizmeti olarak oturum açılmış hesaba okuma erişimi verin.</li><li>İstemci sertifikasını açın ve Ayrıntılar sekmesinden parmak izini kopyalayın.</li><li>MultiFactorAuthenticationAdfsAdapter.config dosyasında, WebServiceSdkCertificateThumbprint’i önceki adımda kopyalanan dizeye ayarlayın.</li></ol>
Register-MultiFactorAuthenticationAdfsAdapter.ps1 dizesini Register-AdfsAuthenticationProvider komutunun sonuna ConfigurationFilePath <path> ekleyerek düzenleyin, <path> MultiFactorAuthenticationAdfsAdapter.config dosyasının tam yoludur.|


Şimdi bağdaştırıcıyı kaydetmek için PowerShell’de \Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 betiğini Çalıştırın.  Bağdaştırıcı WindowsAzureMultiFactorAuthentication olarak kaydedilir.  Kaydın etkili olması için AD FS hizmetini yeniden başlatmalısınız. 




























 

 


 

 


 





 


 

























































































 


 

 






 


<!---HONumber=Jun16_HO2-->


