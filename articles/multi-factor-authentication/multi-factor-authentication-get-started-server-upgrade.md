<properties 
    pageTitle="PhoneFactor Aracısı’nı Azure Multi-Factor Authentication Sunucusu’na yükseltme" 
    description="Bu belgede Azure MFA Sunucusu kullanmaya başlama ve eski phonefactor aracısından yükseltme açıklanır." 
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

# PhoneFactor Aracısı’nı Azure Multi-Factor Authentication Sunucusu’na yükseltme

PhoneFactor Agent v5.x ya da eski bir sürümünden Azure Multi-Factor Authentication Sunucusu’na yükseltmek PhoneFactor Aracısı ve bağlantılı bileşenlerin Multi-Factor Authentication Sunucusu ve bunun bağlı bileşenleri yüklenmeden önce kaldırılmasını gerektirir. 

## PhoneFactor Aracısı’nı Azure Multi-Factor Authentication Sunucusu’na yükseltmek için
<ol>
<li>İlk olarak, PhoneFactor veri dosyasını yedekleyin. Varsayılan yükleme konumu C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata şeklindedir.


<li>Kullanıcı Portalı yüklü ise:</li>
<ol>
<li>Yükleme klasörüne gidin ve web.config dosyasını yedekleyin. Varsayılan yükleme konumu C:\inetpub\wwwroot\PhoneFactor şeklindedir.</li>


<li>Portala özel temalar eklediyseniz, C:\inetpub\wwwroot\PhoneFactor\App_Themes dizini altındaki özel klasörünüzü yedekleyin.</li>


<li>Kullanıcı Portalı’nı, PhoneFactor Aracısı aracılığıyla (yalnızca PhoneFactor Aracısı ile aynı sunucuda yüklüyse kutlanılabilir) ya da Windows Programlar ve Özellikler aracılığıyla kaldırın.</li></ol>




<li>Mobil Uygulama Web Hizmeti yüklü ise:
<ol>
<li>Yükleme klasörüne gidin ve web.config dosyasını yedekleyin. Varsayılan yükleme konumu C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService şeklindedir.</li>
<li>Mobil Uygulama Web Hizmeti’ni Windows Programlar ve Özellikler aracılığıyla kaldırın.</li></ol>

<li>Web Hizmeti SDK’sı yüklü ise, PhoneFactor Aracısı aracılığıyla ya da Windows Programlar ve Özellikler aracılığıyla kaldırın.

<li>PhoneFactor Aracısı’nı Windows Programlar ve Özellikler aracılığıyla kaldırın.

<li>Multi-Factor Authentication Sunucusu’nu yükleyin. Yükleme yolunun önceki PhoneFactor Aracısı yüklemesinin kayıt defterinden alındığına dikkat edin, bu nedenle aynı konuma yükleme yapılmalıdır (örneğin, C:\Program Files\PhoneFactor). Yeni yüklemeler farklı bir varsayılan yükleme yoluna (örneğin C:\Program Files\Multi-Factor Authentication Server) sahip olacaktır. Önceki PhoneFactor Aracısı’ndan kalan veri dosyası yükleme sırasında yükseltilmelidir, bu nedenle kullanıcılarınız ve ayarlarınız yeni Multi-Factor Authentication Sunucusu yüklemesi sonrasında da burada kalmalıdır.

<li>İstenirse, Multi-Factor Authentication Sunucusu’nu etkinleştirin ve uygun çoğaltma grubuna atandığından emin olun.

<li>Web Hizmeti SDK’sı daha önce yüklendiyse, Multi-Factor Authentication Sunucusu Kullanıcı Arabirimi aracılığıyla yeni Web Hizmeti SDK’sını yükleyin. Artık varsayılan sanal dizin adının “PhoneFactorWebServiceSdk” yerine “MultiFactorAuthWebServiceSdk” olduğuna dikkat edin. Önceki adı kullanmak istiyorsanız, yükleme sırasında sanal dizin adını değiştirmeniz gerekir. Aksi takdirde, yeni varsayılan adı kullanacak şekilde yüklemeye izin verirseniz, doğru konuma yönlendirmek için, Kullanıcı Portalı ve Mobil Uygulama Web Hizmeti gibi Web Hizmeti SDK’sına başvuran tüm uygulamalarda URL’yi değiştirmeniz gerekecektir.

<li>Kullanıcı Portalı önceden PhoneFactor Aracısı Sunucusu’nda yüklüyse, yeni Multi-Factor Authentication Kullanıcı Portalı’nı Multi-Factor Authentication Sunucusu Kullanıcı Arabirimi aracılığıyla yükleyin. Varsayılan sanal dizin adının artık “PhoneFactor” yerine “MultiFactorAuth” olduğuna dikkat edin. Önceki adı kullanmak istiyorsanız, yükleme sırasında sanal dizin adını değiştirmeniz gerekir. Aksi takdirde, yeni varsayılan adı kullanacak şekilde yüklemeye izin verirseniz, Multi-Factor Authentication Sunucusu’nda Kullanıcı Portalı simgesine tıklamanız ve Ayarlar sekmesinde Kullanıcı Portalı URL’sini güncelleştirmeniz gerekir. 

<li>Kullanıcı Portalı ve/veya Mobil Uygulama Web hizmeti daha önce PhoneFactor Aracısı’ndan farklı bir sunucuya yüklendiyse:
<ol>
<li>Yükleme konumuna gidin (örneğin, C:\Program Files\PhoneFactor) ve uygun yükleyicileri diğer sunucuya kopyalayın. Kullanıcı Portalı ve Mobil Uygulama Web Hizmeti için 32-bit ve 64-bit yükleyiciler vardır. Bunlara sırasıyla MultiFactorAuthenticationUserPortalSetupXX.msi ve MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi adı verilir.</li>
<li>Web sunucusunda Kullanıcı Portalı'nı yüklemek için, yönetici olarak bir komut istemi açın ve MultiFactorAuthenticationUserPortalSetupXX.msi komutunu çalıştırın. Varsayılan sanal dizin adının artık “PhoneFactor” yerine “MultiFactorAuth” olduğuna dikkat edin. Önceki adı kullanmak istiyorsanız, yükleme sırasında sanal dizin adını değiştirmeniz gerekir. Aksi takdirde, yeni varsayılan adı kullanacak şekilde yüklemeye izin verirseniz, Multi-Factor Authentication Sunucusu’nda Kullanıcı Portalı simgesine tıklamanız ve Ayarlar sekmesinde Kullanıcı Portalı URL’sini güncelleştirmeniz gerekir. Mevcut kullanıcıların yeni URL konusunda bilgilendirilmesi gerekir.</li>
<li>Kullanıcı Portalı yükleme konumuna gidin (örneğin, C:\inetpub\wwwroot\MultiFactorAuth) ve web.config dosyasını düzenleyin. Yükseltme öncesi yedeklenen özgün web.config dosyasındaki appSettings ve applicationSettings bölümlerindeki değerleri yeni web.config dosyasına kopyalayın. Web Hizmeti SDK’sını yüklerken yeni varsayılan sanal dizin adını sakladıysanız, doğru konuma yönlendirmek için applicationSettings bölümünde URL’yi değiştirin. Önceki web.config dosyasında diğer varsayılan değerler değiştirildiyse, aynı değişiklikleri yeni web.config dosyasına uygulayın.</li>
<li>Web sunucusunda Mobil Uygulama Web Hizmeti’ni yüklemek için, yönetici olarak bir komut istemi açın ve MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi komutunun çalıştırın. Artık varsayılan sanal dizin adının “PhoneFactorPhoneAppWebService” yerine “MultiFactorAuthMobileAppWebService” olduğuna dikkat edin. Önceki adı kullanmak istiyorsanız, yükleme sırasında sanal dizin adını değiştirmeniz gerekir. Son kullanıcıların mobil cihazlarda yazmalarını kolaylaştırmak için kısa bir ad seçmek isteyebilirsiniz. Aksi takdirde, yeni varsayılan adı kullanacak şekilde yüklemeye izin verirseniz, Multi-Factor Authentication Sunucusu’nda Mobil Uygulama simgesine tıklamanız ve Mobil Uygulama Web Hizmeti URL’sini güncelleştirmeniz gerekir.</li>
<li>Mobil Uygulama Web Hizmeti yükleme konumuna gidin (örneğin, C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService) ve web.config dosyasını düzenleyin. Yükseltme öncesi yedeklenen özgün web.config dosyasındaki appSettings ve applicationSettings bölümlerindeki değerleri yeni web.config dosyasına kopyalayın. Web Hizmeti SDK’sını yüklerken yeni varsayılan sanal dizin adını sakladıysanız, doğru konuma yönlendirmek için applicationSettings bölümünde URL’yi değiştirin. Önceki web.config dosyasında diğer varsayılan değerler değiştirildiyse, aynı değişiklikleri yeni web.config dosyasına uygulayın.</li></ol>


 


 



<!--HONumber=Aug16_HO4-->


