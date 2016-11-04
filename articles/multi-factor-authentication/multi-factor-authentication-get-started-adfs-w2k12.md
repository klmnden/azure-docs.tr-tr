---
title: Windows Server 2012 R2 AD FS ile MFA Sunucusu | Microsoft Docs
description: Bu makale Windows Server 2012 R2’de Azure Multi-Factor Authentication ve AD FS’yi kullanmaya başlama işlemi açıklanmaktadır.
services: multi-factor-authentication
documentationcenter: ''
author: kgremban
manager: femila
editor: curtland

ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/22/2016
ms.author: kgremban

---
# Windows Server 2012 R2’de AD FS ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme
Kuruluşunuz Active Directory Federasyon Hizmetleri (AD FS) kullanıyorsa ve bulut ya da şirket içi kaynaklarınızı güvenli hale getirmek istiyorsanız Azure Multi-Factor Authentication Sunucusunu dağıtıp AD FS ile çalışmak üzere yapılandırabilirsiniz. Bu yapılandırma yüksek değerli uç noktalar için çok faktörlü kimlik doğrulamasını tetikler.

Bu makale Windows Server 2012 R2’de AD FS ile Multi-Factor Authentication Sunucusu kullanmayı ele alır. Daha fazla bilgi için [AD FS 2.0 ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme](multi-factor-authentication-get-started-adfs-adfs2.md) konusunu okuyun.

## Azure Multi-Factor Authentication Sunucusu ile Windows Server 2012 R2 AD FS’yi güvenli hale getirme
Azure Multi-Factor Authentication Sunucusu’nu yüklerken aşağıdaki seçeneklere sahip olursunuz:

* Azure Multi-Factor Authentication Sunucusu’nu yerel olarak AD FS ile aynı sunucuya yükleme
* Azure Multi-Factor Authentication Bağdaştırıcısı’nı yerel olarak AD FS Sunucusu’na yükleme ve ardından farklı bir bilgisayara Multi-Factor Authentication Sunucusu yükleme

Başlamadan önce, aşağıdaki bilgileri unutmayın:

* Azure Multi-Factor Authentication Sunucusunu AD FS sunucusuna yüklemeniz gerekli değildir. Ancak, AD FS için Multi-Factor Authentication bağdaştırıcısını AD FS çalıştıran bir Windows Server 2012 R2’ye yüklemeniz gerekir. Desteklenen bir sürüm olduğu sürece sunucuyu farklı bir bilgisayara yükleyebilirsiniz ve AD FS bağdaştırıcısını ayrı olarak AD FS federasyon sunucusuna yükleyebilirsiniz. Bağdaştırıcıyı ayrıca yükleme hakkında bilgi edinmek için aşağıdaki yordamlara bakın.
* Multi-Factor Authentication Sunucusu’nun AD FS bağdaştırıcısı tasarlanırken, AD FS’nin bir uygulama adı olarak kullanılabilecek bağlı olan taraf adını bağdaştırıcıya geçirebileceği öngörülmüştür. Ancak, bu durumun gerçekleşmediği görülmüştür. Kuruluşunuz metin iletisi ya da mobil uygulama kimlik doğrulama yöntemlerini kullanıyorsa, Şirket Ayarları’nda tanımlanan dizeler <$*application_name*$> şeklinde bir yer tutucu içerir. AD FS bağdaştırıcısı kullandığınızda bu yer tutucu otomatik olarak değiştirilmez. AD FS’yi güvenli hale getirdiğinizde yer tutucunun uygun dizelerden kaldırılması önerilir.
* Oturum açmak için kullandığınız hesap, Active Directory hizmetinde güvenlik grupları oluşturmak için kullanıcı haklarına sahip olmalıdır.
* Multi-Factor Authentication AD FS bağdaştırıcısı yükleme sihirbazı, Active Directory örneğinizde PhoneFactor Admins adlı bir güvenlik grubu oluşturur ve ardından federasyon hizmetinizin AD FS hizmet hesabını bu gruba ekler. PhoneFactor Admins grubunun gerçekten oluşturulduğunu ve AD FS hizmet hesabının bu gruba üye olduğunu doğrulamanız önerilir. Gerekirse, AD FS hizmeti hesabınızı etki alanı denetleyicinizde PhoneFactor Admins grubuna el ile ekleyin.
* Kullanıcı portalıyla Hizmet SDK’sını yükleme hakkında bilgi edinmek için bkz. [Azure Multi-Factor Authentication Sunucusu için kullanıcı portalını dağıtma.](multi-factor-authentication-get-started-portal.md)

### Azure Multi-Factor Authentication Sunucusu’nu yerel olarak AD FS sunucusuna yükleme
1. Azure Multi-Factor Authentication Sunucusu’nı AD FS federasyon sunucunuza indirin ve yükleyin. Yükleme bilgileri için bkz. [Azure Multi-Factor Authentication Sunucusunu kullanmaya başlama](multi-factor-authentication-get-started-server.md).
2. Azure Multi-Factor Authentication Sunucusu yönetim konsolunda **AD FS** simgesine tıklayın ve ardından **Kullanıcı kaydına izin ver** ve **Kullanıcıların yöntemi seçmesine izin ver** seçeneklerini belirleyin.
3. Kuruluşunuz için belirtmek istediğiniz ek seçenekleri belirleyin.
4. **AD FS Bağdaştırıcısı’nı Yükle**'ye tıklayın.
   <center>![Bulut](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Bilgisayar bir etki alanına katıldıysa ve AD FS Bağdaştırıcısı ile Multi-Factor Authentication hizmeti arasındaki hizmeti güvenli hale getirmek üzere Active Directory yapılandırması tamamlanmamışsa **Active Directory** penceresi görüntülenir. Bu yapılandırmayı otomatik olarak tamamlamak için **İleri** düğmesine tıklayın ya da **Otomatik Active Directory yapılandırmasını atla ve ayarları el ile yapılandır** onay kutusunu işaretleyin ve **İleri**’ye tıklayın.
6. Bilgisayar bir etki alanına katılmadıysa ve AD FS Bağdaştırıcısı ile Multi-Factor Authentication hizmeti arasındaki hizmeti güvenli hale getirmek üzere yerel grup yapılandırması tamamlanmamışsa, **Yerel Grup** penceresi görüntülenir. Bu yapılandırmayı otomatik olarak tamamlamak için **İleri** düğmesine tıklayın ya da **Otomatik Yerel Grup yapılandırmasını atla ve ayarları el ile yapılandır** onay kutusunu işaretleyin ve **İleri**’ye tıklayın.
7. Yükleme sihirbazında **İleri**’ye tıklayın. Azure Multi-Factor Authentication Sunucusu PhoneFactor Admins grubunu oluşturur ve AD FS hizmeti hesabını PhoneFactor Admins grubuna ekler.
   <center>![Bulut](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. **Yükleyiciyi Başlat** sayfasında **İleri**’ye tıklayın.
9. Multi-Factor Authentication AD FS bağdaştırıcısı yükleyicisinde **İleri**’ye tıklayın.
10. Yükleme tamamlandığında **Kapat**'a tıklayın.
11. Bağdaştırıcı yüklendiğinde AD FS’ye kaydetmeniz gerekir. Windows PowerShell’i açın ve aşağıdaki komutu çalıştırın:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
    <center>![Bulut](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Yeni kaydettiğiniz bağdaştırıcıyı kullanmak için AD FS’deki genel kimlik doğrulama ilkesini düzenleyin. AD FS yönetim konsolunda **Kimlik Doğrulama İlkeleri** düğümüne gidin. **Multi-factor Authentication** bölümünde **Genel Ayarlar** bölümünün yanındaki **Düzenle** bağlantısına tıklayın. **Genel Kimlik Doğrulama İlkesini Düzenle** penceresinde, ek kimlik doğrulama yöntemi olarak **Multi-Factor Authentication**’ı seçin ve ardından **Tamam**'a tıklayın. Bağdaştırıcı WindowsAzureMultiFactorAuthentication olarak kaydedilir. Kaydın etkili olması için AD FS hizmetini yeniden başlatmalısınız.

<center>![Bulut](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Bu noktada Multi-Factor Authentication Sunucusu, AD FS ile birlikte kullanım amacıyla ek kimlik doğrulama sağlayıcısı olmak üzere kurulur.

## Web Hizmeti SDK’sını kullanarak AD FS bağdaştırıcısının tek başına örneğini yükleme
1. Web Hizmeti SDK’sını Multi-Factor Authentication Sunucusu çalıştıran sunucuya yükleyin.
2. Aşağıdaki dosyaları \Program Files\Multi-Factor Authentication Server dizininden AD FS bağdaştırıcısını yüklemeyi planladığınız sunucuya kopyalayın:
   * MultiFactorAuthenticationAdfsAdapterSetup64.msi
   * Register-MultiFactorAuthenticationAdfsAdapter.ps1
   * Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
   * MultiFactorAuthenticationAdfsAdapter.config
3. MultiFactorAuthenticationAdfsAdapterSetup64.msi yükleme dosyasını çalıştırın.
4. Yükleme işlemini gerçekleştirmek için Multi-Factor Authentication AD FS bağdaştırıcısı yükleyicisinde **İleri**’ye tıklayın.
5. Yükleme tamamlandığında **Kapat**'a tıklayın.

## MultiFactorAuthenticationAdfsAdapter.config dosyasını düzenleme
MultiFactorAuthenticationAdfsAdapter.config dosyasını düzenlemek için aşağıdaki adımları izleyin:

1. **UseWebServiceSdk** düğümünü **true** olarak ayarlayın.  
2. **WebServiceSdkUrl** değerini Multi-Factor Authentication Web Hizmeti SDK URL’sine ayarlayın. Örneğin:  **https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx** Burada sertifika adı, sertifikanızın adıdır.  
3. *-ConfigurationFilePath &lt;path&gt;* dizesini `Register-AdfsAuthenticationProvider` komutunun sonuna ekleyerek Register-MultiFactorAuthenticationAdfsAdapter.ps1 komut dosyasını düzenleyin; burada *&lt;path&gt;*, MultiFactorAuthenticationAdfsAdapter.config dosyasının tam yoludur.

### Web Hizmeti SDK’sını bir kullanıcı adı ve parola kullanarak yapılandırma
Web Hizmeti SDK’sını yapılandırmaya yönelik iki seçenek vardır. Birincisi kullanıcı adı ve parola, ikincisi ise istemci sertifikası ile yapılır. Birinci seçenek için bu adımları izleyin veya ikinci seçenek için bu adımları atlayın.  

1. **WebServiceSdkUsername** değerini PhoneFactor Admins güvenlik grubunun üyesi olan bir hesaba ayarlayın. &lt;Etki alanı&gt;&#92;&lt;kullanıcı adı&gt; biçimini kullanın.  
2. **WebServiceSdkPassword** değerini uygun hesap parolası olarak ayarlayın.

### Web Hizmeti SDK’sını bir istemci sertifikası ile yapılandırma
Bir kullanıcı adı ve parola kullanmak istemiyorsanız Web Hizmeti SDK’sını bir istemci sertifikası ile yapılandırmak için aşağıdaki adımları izleyin.

1. Web Hizmeti SDK’sı çalıştıran sunucu için sertifika yetkilisinden bir istemci sertifikası alın. [İstemci sertifikalarını alma](https://technet.microsoft.com/library/cc770328.aspx) hakkında bilgi edinin.  
2. İstemci sertifikasını Web Hizmeti SDK’sı çalıştıran sunucudaki yerel bilgisayar kişisel sertifika deposuna aktarın. Not: sertifika yetkilisinin genel sertifikasının Güvenilen Kök Sertifikalar sertifika deposunda olduğundan emin olun.  
3. İstemci sertifikasının ortak ve özel anahtarlarını bir .pfx dosyasına aktarın.  
4. Base64 biçimindeki ortak anahtarı bir .cer dosyasına aktarın.  
5. Sunucu Yöneticisi'nde, Web Server (IIS)\Web Server\Security\IIS İstemci Sertifikası Eşleme Kimlik doğrulaması özelliğinin yüklü olduğunu doğrulayın. Yüklü değilse, bu özelliğe eklemek üzere **Rol ve Özellik Ekle**’yi seçin.  
6. IIS Yöneticisi'nde, Web Hizmeti SDK’sı sanal dizinini içeren web sitesinde **Yapılandırma Düzenleyicisi**'ne çift tıklayın. Not: Bunu sanal dizin düzeyinde değil web sitesi düzeyinde yapmanız çok önemlidir.  
7. **System.webServer/security/authentication/iisClientCertificateMappingAuthentication** bölümüne gidin.  
8. **Etkin**’i **true** olarak ayarlayın.  
9. **oneToOneCertificateMappingsEnabled** değerini **true** olarak ayarlayın.  
10. **oneToOneMappings** ifadesinin yanındaki **...** düğmesine ve ardından **Ekle** bağlantısına tıklayın.  
11. Önceden dışarı aktardığınız Base64 .cer dosyasını açın. *-----BEGIN CERTIFICATE-----*, *-----END CERTIFICATE-----* ifadelerini ve tüm satır sonlarını kaldırın. Sonuç dizesini kopyalayın.  
12. **Sertifika** değerini önceki adımda kopyaladığınız dizeye ayarlayın.  
13. **Etkin**’i **true** olarak ayarlayın.  
14. **userName** değerini PhoneFactor Admins güvenlik grubunun üyesi olan bir hesaba ayarlayın. &lt;Etki alanı&gt;&#92;&lt;kullanıcı adı&gt; biçimini kullanın.  
15. Parolayı uygun hesap parolasına ayarlayın ve ardından Yapılandırma Düzenleyicisi’ni kapatın.  
16. **Uygula** bağlantısına tıklayın.  
17. Web Hizmeti SDK’sı sanal dizininde **Kimlik doğrulama** öğesine çift tıklayın.  
18. **ASP.NET Kimliğe Bürünme** ve **Temel Kimlik Doğrulaması**’nın **Etkin** olarak ve diğer tüm öğelerin **Devre Dışı** olarak ayarlandığını doğrulayın.  
19. Web Hizmeti SDK’sı sanal dizininde **SSL Ayarları** öğesine çift tıklayın.  
20. **İstemci Sertifikaları**’nı **Kabul Et** olarak ayarlayın ve ardından **Uygula**’ya tıklayın.  
21. Önceden dışarı aktardığınız .pfx dosyasını AD FS bağdaştırıcısı çalıştıran sunucuya kopyalayın.  
22. .pfx dosyasını yerel bilgisayar kişisel sertifika deposuna aktarın.  
23. Sağ tıklayıp **Özel Anahtarları Yönet**’i seçin ve ardından AD FS hizmetinde oturum açmak için kullandığınız hesaba okuma erişimi verin.  
24. İstemci sertifikasını açın ve **Ayrıntılar** sekmesinden parmak izini kopyalayın.  
25. MultiFactorAuthenticationAdfsAdapter.config dosyasında, **WebServiceSdkCertificateThumbprint**’i önceki adımda kopyalanan dizeye ayarlayın.  

Son olarak, bağdaştırıcıyı kaydetmek için PowerShell’de \Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 betiğini çalıştırın. Bağdaştırıcı WindowsAzureMultiFactorAuthentication olarak kaydedilir. Kaydın etkili olması için AD FS hizmetini yeniden başlatmalısınız.

## İlgili konular
Sorun giderme yardımı için bkz. [Azure Multi-Factor Authentication SSS](multi-factor-authentication-faq.md)

<!--HONumber=Sep16_HO4-->


