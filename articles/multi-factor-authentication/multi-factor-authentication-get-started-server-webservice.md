<properties 
    pageTitle="MFA Sunucusu Mobil Uygulama Web Hizmeti’ni kullanmaya başlama" 
    description="Azure Multi-Factor Authentication Uygulaması ek bir bant dışı kimlik doğrulama seçeneği sunar.  MFA sunucusunun kullanıcılar için anında iletme bildirimleri kullanmasına olanak tanır." 
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

# MFA Sunucusu Mobil Uygulama Web Hizmeti’ni kullanmaya başlama

Azure Multi-Factor Authentication Uygulaması ek bir bant dışı kimlik doğrulama seçeneği sunar. Oturum açma sırasında kullanıcıya otomatik telefon çağrısı veya SMS uygulamak yerine, Azure Multi-Factor Authentication, kullanıcının akıllı telefonu ya da tabletindeki Azure Multi-Factor Authentication Uygulamasına anında iletme bildirimi gönderir. Oturum açmak için kullanıcının uygulamada “Kimliği Doğrula” seçeneğine dokunması (ya da PIN’i girip “Kimliği Doğrula” seçeneğine dokunması ) yeterlidir. 

Azure Multi-Factor Authentication Uygulamasını kullanmak için, uygulamanın Mobil Uygulama Web Hizmeti ile başarıyla iletişim kurabilmesini sağlamak amacıyla aşağıdakiler gereklidir: 

- Donanım ve yazılım gereksinimleri için lütfen Donanım ve Yazılım Gereksinimleri’ne bakın
- Azure Multi-Factor Authentication Sunucusu’nun v6.0 ya da üst sürümünü kullanıyor olmalısınız
- Mobil Uygulama Web Hizmeti’nin, Microsoft® Internet Information Services (IIS) IIS 7.x veya üst sürümünü çalıştıran bir İnternet’e yönelik web sunucusunda yüklü olması gerekir.  IIS hakkında daha fazla bilgi için bkz. [IIS.NET](http://www.iis.net/).
- ASP.NET v4.0.30319’un yüklü, kayıtlı ve İzinli olarak ayarlandığından emin olun.
- Gerekli rol hizmetleri ASP.NET ve IIS 6 Metatabanı Uyumluluğu’nu içerir.
- Mobil Uygulama Web Hizmeti genel bir URL ile erişilebilir olmalıdır.
- Mobil Uygulama Web Hizmeti bir SSL sertifikası ile güvenli hale getirilmelidir.
- Azure Multi-Factor Authentication Web Hizmeti SDK’sı, Azure Multi-Factor Authentication Sunucusu’nun yüklendiği sunucuda IIS 7.x ya da üst sürümünde yüklenmelidir.
- Azure Multi-Factor Authentication Web Hizmeti SDK’sı bir SSL sertifikası ile güvenli hale getirilmelidir.
- Mobil Uygulama Web Hizmeti SSL üzerinden Azure Multi-Factor Authentication Web Hizmeti SDK’sına bağlanabilmelidir.
- Mobil Uygulama Web Hizmeti, “PhoneFactor Admins” adlı bir güvenlik grubunun üyesi olan hizmetin kimlik bilgilerini kullanarak Azure Multi-Factor Authentication Web Hizmeti SDK’sının kimliğini doğrulayabilmelidir. Azure Multi-Factor Authentication Sunucusu etki alanı ile birleşik bir sunucuda çalışıyorsa, bu hizmet hesabı ve grubu Active Directory’de yer alır. Bir etki alanı ile birleştirilmediyse, bu hizmet hesabı ve grubu yerel olarak Azure Multi-Factor Authentication Sunucusu’nda yer alır.


Azure Multi-Factor Authentication Sunucusu dışında bir sunucuya kullanıcı portalını yüklemek aşağıdaki üç adımı gerektirir:

1. Web hizmeti SDK’sını yükleme
2. Mobil uygulama web hizmetini yükleme
3. Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırma
4. Son kullanıcılar için Azure Multi-Factor Authentication uygulamasını etkinleştirme

## Web hizmeti SDK’sını yükleme

Azure Multi-Factor Authentication Web Hizmeti SDK’sı Azure Multi-Factor Authentication Sunucusu’nda halihazırda yüklü değilse, bu sunucuya gidin ve Azure Multi-Factor Authentication Sunucusu’nu açın. Web Hizmeti SDK’sı simgesine tıklayın, Web Hizmeti SDK’sını yükle düğmesine... tıklayın ve sunulan yönergeleri izleyin. Web Hizmeti SDK’sı bir SSL sertifikası ile güvenli hale getirilmelidir. Kendinden imzalı bir sertifika bu amaç doğrultusunda kabul edilebilir, ancak SSL bağlantısı başlatıldığında bu sertifikaya güvenmesi için, Kullanıcı Portalı web sunucusundaki Yerel Bilgisayar hesabının “Güvenilen Kök Sertifika Yetkilileri” deposuna aktarılmalıdır. 

<center>![Kurulum](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## Mobil uygulama web hizmetini yükleme
Mobil uygulama web hizmetini yüklemeden önce aşağıdakilere dikkat edin:

- Azure Multi-Factor Authentication Kullanıcı Portalı İnternet’e yönelik sunucuda zaten yüklüyse, Web Hizmeti SDK’sına ilişkin kullanıcı adı, parola ve URL Kullanıcı Portalı’nın web.config dosyasından kopyalanabilir. 
- İnternet'e yönelik web sunucusunda bir web tarayıcısı açmak ve web.config dosyasına girilen Web hizmeti SDK’sının URL’sine gitmek faydalıdır. Tarayıcı web hizmetine başarıyla gidebilirse, sizden kimlik bilgilerinizi ister. Aynen dosyada göründüğü gibi web.config dosyasına girilen parola girilen kullanıcı adını ve parolayı girin. Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.
- Mobil Uygulama Web Hizmeti web sunucusunun önünde ters proxy ya da güvenlik duvarı yer alıyorsa ve SSL boşaltma gerçekleştiriyorsa, Mobil Uygulama Web Hizmeti’nn https yerine http kullanabilmesi için, Mobil Uygulama Web Hizmeti web.config dosyasını düzenleyebilir ve aşağıdaki anahtarı <appSettings> bölümüne ekleyebilirsiniz. Ancak, güvenlik duvarı/ters proxy’ye yönelik Mobil Uygulamadan alınan SSL hala gereklidir. <add key="SSL_REQUIRED" value="false"/> 

### Mobil uygulama web hizmetini yüklemek için

<ol>
<li>Azure Multi-Factor Authentication Sunucusu’nda Windows Gezgini'ni açın ve Azure Multi-Factor Authentication Sunucusu’nun yüklü olduğu klasöre gidin (örneğin, C:\Program Files\Azure Multi-Factor Authentication). Mobil Uygulama Web Hizmeti’nin yükleneceği sunucu için, uygun şekilde, Azure Multi-Factor AuthenticationPhoneAppWebServiceSetup yükleme dosyasının 32-bit ya da 64-bit sürümünü seçin. Yükleme dosyasını İnternet’e yönelik sunucuya kopyalayın.</li> 

<li>İnternet’e yönelik sunucuda, kurulum dosyası yönetici haklarıyla çalıştırılmalıdır. Bunu yapmanın en kolay yolu, yönetici olarak bir komut istemi açmak ve yükleme dosyasının kopyalandığı konuma gitmektir.</li>  

<li>Multi-Factor AuthenticationMobileAppWebServiceSetup yükleme dosyasını çalıştırın, isterseniz Site’yi değiştirin ve Sanal dizini “PA” gibi kısa bir adla değiştirin. Kullanıcıların Mobil Uygulama Web Hizmeti URL’sini etkinleştirme sırasında mobil cihaza girmesi gerektiğinden, kısa bir sanal dizin adın önerilir.</li> 

<li>Azure Multi-Factor AuthenticationMobileAppWebServiceSetup yüklenmesi tamamlandıktan sonra, C:\inetpub\wwwroot\PA (veya sanal dizin adını temel alarak uygun dizin) gidin ve web.config dosyasını düzenleyin.</li>  

<li>WEB_SERVICE_SDK_AUTHENTICATION_USERNAME ve WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD anahtarlarını bulun ve değerleri PhoneFactor Admins güvenlik grubunun bir üyesi olan hizmet hesabının kullanıcı adı ve parolası olarak ayarlayın (Yukarıdaki Gereksinimler bölümüne bakın). Daha önce yüklenmişse, bu Azure Multi-Factor Authentication Kullanıcı Portalı’nın Kimliği olarak kullanılanla aynı hesap olabilir. Satırın sonundaki tırnak işaretlerinin arasına, (value=””/>) Kullanıcı Adı ve Parolayı girdiğinizden emin olun. Tam kullanıcı adı kullanmanız önerilir (örneğin, etki alanı\kullanıcı adı veya makine\kullanıcı adı) kullanmak için önerilir.</li>  

<li>pfMobile App Web Service_pfwssdk_PfWsSdk ayarını bulun ve “http://localhost:4898/PfWsSdk.asmx” değerini Azure Multi-Factor Authentication Sunucusu’nda çalışan Web Hizmeti SDK’sının URL’si ile değiştirin (örneğin, https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Bu bağlantı için SSL kullanılması nedeniyle, SSL sertifikası sunucu adı için kullanılacağı ve kullanılan URL’nin sertifikadaki adla eşleşmesi gerektiği için Web Hizmeti SDK’sına IP adresi ile değil sunucu adıyla başvurmalısınız. Sunucu, İnternet’e yönelik sunucudan gelen bir IP adresini çözümlemezse, Azure Multi-Factor Authentication Sunucusu’nun adını bu IP adresine eşlemek için bu sunucudaki hosts dosyasına bir giriş ekleyin. Değişiklikler yapıldıktan sonra web.config dosyasını kaydedin.</li>  

<li>Mobil Uygulama Web Hizmeti’nin altında yüklendiği web sitesi (örneğin Varsayılan Web Sitesi) halihazırda ortak olarak imzalanmış bir sertifikayla bağlanmadıysa, henüz yüklü değilse sertifikayı sunucuya yükleyin, IIS Yöneticisi’ni açın ve sertifikayı web sitesine bağlayın.</li>  

<li>Herhangi bir bilgisayarda web tarayıcısını açın ve Mobil Uygulama Web Hizmeti’nin yüklendiği URL'ye gidin (örn. https://www.publicwebsite.com/PA). Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.</li> 

### Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırma
Artık mobil uygulama web hizmeti yüklendiğine göre, portal ile çalışmak için Azure Multi-Factor Authentication Sunucusu’nu yapılandırmalısınız.

#### Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırmak için

1. Azure Multi-Factor Authentication Sunucusu’nda Kullanıcı Portalı simgesine tıklayın. Kullanıcıların kendi kimlik doğrulama yöntemlerini denetlemesine izin veriliyorsa, Ayarlar sekmesinde, Kullanıcıların yöntemi seçmesine izin ver altında Mobil Uygulama’yı işaretleyin. Bu özellik etkinleştirilmeden, Mobil Uygulama için etkinleştirme işlemini tamamlamak üzere son kullanıcıların Yardım Masanızla iletişim kurması gerekir.
2. Kullanıcıların Mobil Uygulama etkinleştirmesine izin ver kutusunu işaretleyin.
3. Kullanıcı Kaydına İzin Ver kutusunu işaretleyin.
4. Mobil uygulama simgesine tıklayın.
5. Azure Multi-Factor AuthenticationMobileAppWebServiceSetup yüklenirken oluşturulan sanal dizinle kullanılan URL’yi girin. Sağlanan alana bir Hesap Adı girilebilir. Bu şirket adı mobil uygulamada görüntülenir. Boş bırakılırsa, Azure Yönetim Portalı'nda oluşturulan Multi-Factor Auth sağlayıcınızın adı görüntülenir. 



<center>![Kurulum](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
 


<!---HONumber=Jun16_HO2-->


