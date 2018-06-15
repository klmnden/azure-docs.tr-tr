---
title: Azure MFA Sunucusu Mobil Uygulama Web Hizmeti | Microsoft Belgeleri
description: MFA sunucusunu Microsoft Authenticator Uygulaması ile kullanıcılara anında iletme bildirimleri göndermek için yapılandırın.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: f6b50554e00e247d564f2be5bf579977f3bea9cf
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33869929"
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile mobil uygulama kimlik doğrulamasını etkinleştirme

Microsoft Authenticator uygulaması ek bir bant dışı doğrulama seçeneği sunar. Oturum açma sırasında kullanıcıya otomatik telefon çağrısı yapmak veya SMS göndermek yerine, Azure Multi-Factor Authentication, kullanıcının akıllı telefonu ya da tabletindeki Microsoft Authenticator uygulamasına anında iletme bildirimi gönderir. Kullanıcının oturum açma işlemini tamamlamak için uygulamada **Doğrula** seçeneğine dokunması (ya da PIN’i girip “Kimliği Doğrula” seçeneğine dokunması ) yeterlidir.

Şebeke sinyal gücünün güvenilir olmadığı durumlarda iki aşamalı doğrulama için bir mobil uygulama kullanmak tercih edilir. Uygulamayı bir OATH belirteci oluşturucu olarak kullanıyorsanız ağ veya İnternet bağlantısı gerekmez.

Ortamınıza bağlı olarak, mobil uygulama web hizmetini Azure Multi-Factor Authentication sunucusu ile aynı sunucuya veya İnternet'e yönelik başka bir sunucuya dağıtmak isteyebilirsiniz.

> [!IMPORTANT]
> MFA Sunucusu 8.0 veya üzeri bir sürümü yüklediyseniz aşağıdaki adımların çoğu gerekmez. [Mobil uygulamayı yapılandırma](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server) bölümündeki adımlar izlenerek mobil uygulama kimlik doğrulaması ayarlanabilir.

## <a name="requirements"></a>Gereksinimler

Microsoft Authenticator uygulamasını kullanmak için, uygulamanın Mobil Uygulama Web Hizmeti ile başarıyla iletişim kurabilmesini sağlamak amacıyla aşağıdakiler gereklidir:

* Azure Multi-Factor Authentication Sunucusu v6.x veya üzeri
* Microsoft® [Internet Information Services (IIS) IIS 7.x veya üzerini](http://www.iis.net/) çalıştıran İnternet’e yönelik bir web sunucusuna Mobil Uygulama Web Hizmeti’ni yükleme
* ASP.NET v4.0.30319’un yüklenmesi, kaydedilmesi ve İzinli olarak ayarlanması
* Gerekli rol hizmetleri ASP.NET ve IIS 6 Metatabanı Uyumluluğu’nu içerir.
* Mobil Uygulama Web Hizmeti’nin genel bir URL ile erişilebilir olması
* Mobil Uygulama Web Hizmeti’nin bir SSL sertifikası ile güvenli hale getirilmesi.
* **Azure Multi-Factor Authentication Sunucusu’nun yüklendiği sunucudaki** IIS 7.x ya da üzeri bir sürüme Azure Multi-Factor Authentication Web Hizmeti SDK’sını yükleme
* Azure Multi-Factor Authentication Web Hizmeti SDK’sının bir SSL sertifikası ile güvenli hale getirilmesi.
* Mobil Uygulama Web Hizmeti’nin SSL üzerinden Azure Multi-Factor Authentication Web Hizmeti SDK’sına bağlanabilmesi
* Mobil Uygulama Web Hizmeti’nin "PhoneFactor Admins" güvenlik grubunun üyesi olan bir hizmet hesabının kimlik bilgilerini kullanarak Azure MFA Web Hizmeti SDK’sında kimliğini doğrulayabilmesi. Azure Multi-Factor Authentication Sunucusu etki alanı ile birleşik bir sunucudaysa, bu hizmet hesabı ve grubu Active Directory’de yer alır. Bir etki alanı ile birleştirilmediyse, bu hizmet hesabı ve grubu yerel olarak Azure Multi-Factor Authentication Sunucusu’nda yer alır.

## <a name="install-the-mobile-app-web-service"></a>Mobil uygulama web hizmetini yükleme

Mobil uygulama web hizmetini yüklemeden önce aşağıdaki ayrıntılara dikkat edin:

* v8.0 veya üzeri için mobil uygulama web hizmetinin yüklenmesi gerekmez. Yalnızca [Mobil uygulamayı yapılandırma](#configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server) bölümündeki adımları tamamlayın.
* "PhoneFactor Admins" grubunun bir parçası olan bir Hizmet Hesabınız olması gerekir. Bu hesap, Kullanıcı Portalı yüklemesinde kullanılanla aynı olabilir.
* İnternet'e yönelik web sunucusunda bir web tarayıcısı açmak ve web.config dosyasına girilen Web hizmeti SDK’sının URL’sine gitmek faydalıdır. Tarayıcı web hizmetine başarıyla gidebilirse, sizden kimlik bilgilerinizi ister. Aynen dosyada göründüğü gibi web.config dosyasına girilen parola girilen kullanıcı adını ve parolayı girin. Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.
* Mobil Uygulama Web Hizmeti web sunucusunun önünde ters bir proxy ya da güvenlik duvarı yer alıyorsa ve SSL boşaltma gerçekleştiriyorsa, Mobil Uygulama Web Hizmeti’nin https yerine http kullanabilmesi için Mobil Uygulama Web Hizmeti web.config dosyasını düzenleyebilirsiniz. Güvenlik duvarı/ters proxy’ye yönelik Mobil Uygulamadan alınan SSL hala gereklidir. Aşağıdaki anahtarları \<appSettings\> bölümüne ekleyin:

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-the-web-service-sdk"></a>Web hizmeti SDK’sını yükleme

İki senaryoda da Azure Multi-Factor Authentication Web Hizmeti SDK’sı Azure Multi-Factor Authentication (MFA) Sunucusu’nda halihazırda yüklü **değilse**, aşağıdaki adımları tamamlayın.

1. Multi-Factor Authentication Sunucusu konsolunu açın.
2. **Web Hizmeti SDK’sı** altından **Web Hizmeti SDK’sını Yükle**’yi seçin.
3. Herhangi bir nedenden dolayı değiştirmeniz gerekmiyorsa varsayılan değerleri kullanarak yüklemeyi tamamlayın.
4. IIS'de siteye bir SSL sertifikası bağlayın.

IIS sunucusunda bir SSL sertifikası yapılandırma hakkında sorularınız varsa bkz. [IIS'de SSL ayarlama](https://docs.microsoft.com/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

Web Hizmeti SDK’sı bir SSL sertifikası ile güvenli hale getirilmelidir. Bu amaç için otomatik olarak imzalanan bir sertifika kullanılabilir. Kullanıcı Portalı web sunucusunun SSL bağlantısı başlatırken bu sertifikaya güvenebilmesi için sertifikayı sunucudaki Yerel Bilgisayar hesabının “Güvenilen Kök Sertifika Yetkilileri” deposuna aktarın.

![MFA Sunucusu yapılandırma kurulum Web hizmeti SDK'sı](./media/howto-mfaserver-deploy-mobileapp/sdk.png)

### <a name="install-the-service"></a>Hizmeti yükleme

1. **MFA sunucusunda**, yükleme yoluna gidin.
2. Azure MFA Sunucusunun yüklü olduğu klasöre gidin. Varsayılan olarak **C:\Program Files\Azure multi-Factor Authentication** klasörüdür.
3. **MultiFactorAuthenticationMobileAppWebServiceSetup64** yükleme dosyasını bulun. Sunucu İnternet’e yönelik **değilse** yükleme dosyasını İnternet’e yönelik sunucuya kopyalayın.
4. MFA sunucusu İnternet’e yönelik **değilse** **İnternet'e yönelik sunucuya** geçin.
5. **MultiFactorAuthenticationMobileAppWebServiceSetup64** yükleme dosyasını yönetici olarak çalıştırın, isterseniz Site'yi değiştirin ve sanal dizini kısa bir adla değiştirin.
6. Yüklemeyi tamamladıktan sonra, **C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService** (veya sanal dizin adını temel alarak uygun dizin) konumuna gidin ve Web.Config dosyasını düzenleyin.

   * **"WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"** anahtarını bulun ve **value=""** değerini **value="DOMAIN\User"** değeriyle değiştirin. Burada DOMAIN\User, "PhoneFactor Admins" grubunun parçası olan bir Hizmet Hesabıdır.
   * **"WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD"** anahtarını bulun ve **value=""** değerini **value="Password"** ile değiştirin. Burada Password, önceki satırda girdiğiniz Hizmet Hesabının parolasıdır.
   * **pfMobile App Web Service_pfwssdk_PfWsSdk** ayarını bulun ve **http://localhost:4898/PfWsSdk.asmx** olan değeri, Web Hizmeti SDK’sı URL’sine (Örnek: https://mfa.contoso.com/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx)) değiştirin.
   * Web.Config dosyasını kaydedin ve Not Defteri'ni kapatın.

   > [!NOTE]
   > Bu bağlantı için SSL kullanıldığından, Web Hizmeti SDK'sına **IP adresi** ile değil **tam etki alanı adı (FQDN)** ile başvurmanız gerekir. SSL sertifikası FQDN için verilmiş olacaktır ve kullanılan URL’nin sertifikadaki adla eşleşmesi gerekir.

7. Mobil Uygulama Web Hizmeti’nin altında yüklendiği web sitesi halihazırda ortak olarak imzalanmış bir sertifikayla bağlanmadıysa sertifikayı sunucuya yükleyin, IIS Yöneticisi’ni açın ve sertifikayı web sitesine bağlayın.
8. Herhangi bir bilgisayarda web tarayıcısını açın ve Mobil Uygulama Web Hizmeti’nin yüklendiği URL’ye (Örnek: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService)) gidin. Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.
9. Web hizmetleri SDK’da kullanılabilecek metotlar hakkında daha fazla ilgi için MFA sunucusu yardım dosyasına bakın.
10. Artık mobil uygulama web hizmeti yüklendiğine göre, portal ile çalışmak için Azure Multi-Factor Authentication Sunucusu’nu yapılandırmalısınız.

## <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nda mobil uygulama ayarlarını yapılandırma

1. Multi-Factor Authentication Sunucusu konsolunda Kullanıcı Portalı simgesine tıklayın. Kullanıcıların kendi kimlik doğrulama yöntemlerini denetlemesine izin veriliyorsa, Ayarlar sekmesindeki **Kullanıcıların yöntemi seçmesine izin ver** bölümünden **Mobil Uygulama**’yı işaretleyin. Bu özellik etkinleştirilmeden, Mobil Uygulama için etkinleştirme işlemini tamamlamak üzere son kullanıcıların Yardım Masanızla iletişim kurması gerekir.
2. **Kullanıcıların Mobil Uygulamaları etkinleştirmesine izin ver** kutusunu işaretleyin.
3. **Kullanıcı Kaydına İzin Ver** kutusunu işaretleyin.
4. **Mobil Uygulama** simgesine tıklayın.
5. v8.0 veya üzerini kullanıyorsanız şu adımı atlayın: **Mobil Uygulama Web Hizmeti URL'si:** alanına MultiFactorAuthenticationMobileAppWebServiceSetup64 yüklenirken oluşturulan sanal dizinle kullanılan URL’yi (Örnek: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService/)) girin.
6. **Hesap adı** alanına bu hesabın mobil uygulamasında görüntülenecek şirket veya kuruluş adını girin.
   ![MFA Sunucusu yapılandırması Mobil Uygulama ayarları](./media/howto-mfaserver-deploy-mobileapp/mobile.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication ve üçüncü taraf VPN’ler ile gelişmiş senaryolar](howto-mfaserver-nps-vpn.md).
