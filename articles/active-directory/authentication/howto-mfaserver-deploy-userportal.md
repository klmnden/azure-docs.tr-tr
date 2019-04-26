---
title: Azure MFA sunucusu - Azure Active Directory Kullanıcı Portalı
description: Azure MFA ve kullanıcı portalını kullanmaya başlayın.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66a75ee7746d0ab04b505544f91f2905fa392902
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60358659"
---
# <a name="user-portal-for-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu için kullanıcı portalını kullanma

Kullanıcı portalı, kullanıcıların Azure Multi-Factor Authentication’a (MFA) kaydolmasını ve hesaplarını korumalarını sağlayan bir IIS web sitesidir. Bir kullanıcı, sonraki oturum açışı sırasında telefon numarasını, PIN’ini değiştirebilir ya da iki aşamalı doğrulamayı atlayabilir.

Kullanıcılar kendi normal kullanıcı adı ve parolaları ile kullanıcı portalında oturum açar ve kendi kimlik doğrulamalarını tamamlamak için iki aşamalı doğrulama çağrısını tamamlar veya güvenlik sorularını yanıtlar. Kullanıcı kaydına izin veriliyorsa, kullanıcı ilk kez kullanıcı portalında oturum açtığında kendi telefon numarasını ve PIN’ini yapılandırır.

Kullanıcı portalı Yöneticileri yeni kullanıcı eklemek ve mevcut kullanıcıları güncelleştirmek üzere ayarlanabilir ve izin verilebilir.

Ortamınıza bağlı olarak, kullanıcı portalını Azure Multi-Factor Authentication sunucusu ile aynı sunucuya veya İnternet'e yönelik başka bir sunucuya dağıtmak isteyebilirsiniz.

![MFA sunucusu kullanıcı portalı oturum açma sayfasında](./media/howto-mfaserver-deploy-userportal/portal.png)

> [!NOTE]
> Kullanıcı portalı yalnızca Multi-Factor Authentication Sunucusu ile kullanılabilir. Multi-Factor Authentication’ı bulutta kullanıyorsanız, kullanıcılarınızı [İki aşamalı doğrulama için hesabınızı ayarlama](../user-help/multi-factor-authentication-end-user-first-time.md) veya [İki adımlı doğrulama ayarlarınızı yönetme](../user-help/multi-factor-authentication-end-user-manage-settings.md) bölümlerine yönlendirin.

## <a name="install-the-web-service-sdk"></a>Web hizmeti SDK’sını yükleme

İki senaryoda da Azure Multi-Factor Authentication Web Hizmeti SDK’sı Azure Multi-Factor Authentication (MFA) Sunucusu’nda halihazırda yüklü **değilse**, aşağıdaki adımları tamamlayın.

1. Multi-Factor Authentication Sunucusu konsolunu açın.
2. **Web Hizmeti SDK’sı** altından **Web Hizmeti SDK’sını Yükle**’yi seçin.
3. Herhangi bir nedenden dolayı değiştirmeniz gerekmiyorsa varsayılan değerleri kullanarak yüklemeyi tamamlayın.
4. IIS'de siteye bir SSL sertifikası bağlayın.

IIS sunucusunda bir SSL sertifikası yapılandırma hakkında sorularınız varsa bkz. [IIS'de SSL ayarlama](https://docs.microsoft.com/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

Web Hizmeti SDK’sı bir SSL sertifikası ile güvenli hale getirilmelidir. Bu amaç için otomatik olarak imzalanan bir sertifika kullanılabilir. Kullanıcı Portalı web sunucusunun SSL bağlantısı başlatırken bu sertifikaya güvenebilmesi için sertifikayı sunucudaki Yerel Bilgisayar hesabının “Güvenilen Kök Sertifika Yetkilileri” deposuna aktarın.

![MFA Sunucusu yapılandırma kurulum Web hizmeti SDK'sı](./media/howto-mfaserver-deploy-userportal/sdk.png)

## <a name="deploy-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile aynı sunucuda kullanıcı portalını dağıtma

Aşağıdaki ön koşullar kullanıcı portalını Azure Multi-Factor Authentication Sunucusu ile **aynı sunucuya** yüklemek için gereklidir:

* ASP.NET ve IIS 6 meta tabanı uyumluluğu (IIS 7 ya da üst sürümü için) dahil IIS
* Varsa bilgisayar ve Etki Alanı için yönetici haklarına sahip bir hesap. Hesabın Active Directory güvenlik grupları oluşturmak izinlerine sahip olması gerekir.
* Kullanıcı portalını bir SSL sertifikası ile güvenli hale getirin.
* Azure Multi-Factor Authentication Web Hizmeti SDK’sını bir SSL sertifikası ile güvenli hale getirin.

Kullanıcı portalını dağıtmak için aşağıdaki adımları izleyin:

1. Azure Multi-Factor Authentication Sunucusu konsolunu açın, sol menüdeki **Kullanıcı Portalı** simgesine ve ardından **Kullanıcı Portalını Yükle**’ye tıklayın.
2. Herhangi bir nedenden dolayı değiştirmeniz gerekmiyorsa varsayılan değerleri kullanarak yüklemeyi tamamlayın.
3. IIS'de siteye bir SSL sertifikası bağlama

   > [!NOTE]
   > Bu SSL Sertifikası çoğunlukla genel olarak imzalanmış bir SSL sertifikasıdır.

4. Herhangi bir bilgisayarda web tarayıcısını açın ve kullanıcı portalının yüklendiği URL’ye (Örnek: https://mfa.contoso.com/MultiFactorAuth)) gidin. Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.

![MFA Sunucusu Kullanıcı Portalını yükleme](./media/howto-mfaserver-deploy-userportal/install.png)

IIS sunucusunda bir SSL sertifikası yapılandırma hakkında sorularınız varsa bkz. [IIS'de SSL ayarlama](https://docs.microsoft.com/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="deploy-the-user-portal-on-a-separate-server"></a>Kullanıcı portalını ayrı bir sunucuya dağıtma

Azure Multi-Factor Authentication Sunucusu’nun çalıştığı sunucu İnternet’e yönelik değilse, kullanıcı portalını **İnternet’e yönelik ayrı** bir sunucuya yüklemeniz gerekir.

Kuruluşunuz doğrulama yöntemlerinden biri olarak Microsoft Authenticator uygulamasını kullanıyorsa ve kullanıcı portalını kendi sunucusuna dağıtmak istiyorsa, aşağıdaki gereksinimleri tamamlayın:

* Azure Multi-Factor Authentication Sunucusu’nun v6.0 ya da daha yüksek bir sürümünü kullanın.
* Kullanıcı portalını Microsoft Internet Information Services (IIS) 6.x veya daha yüksek bir sürümü çalıştıran, İnternet’e yönelik bir web sunucusuna yükleyin.
* IIS 6.x kullanırken, ASP.NET v2.0.50727’nin yüklü, kayıtlı ve **İzinli** olarak ayarlandığından emin olun.
* IIS 7.x veya sonrasını kullanırken, Temel Kimlik Doğrulaması, ASP.NET ve IIS 6 Metatabanı Uyumluluğu dahil IIS.
* Kullanıcı portalını bir SSL sertifikası ile güvenli hale getirin.
* Azure Multi-Factor Authentication Web Hizmeti SDK’sını bir SSL sertifikası ile güvenli hale getirin.
* Kullanıcı portalının SSL üzerinden Azure Multi-Factor Authentication Web Hizmeti SDK’sına bağlanabildiğinden emin olun.
* Kullanıcı portalının, "PhoneFactor Admins" adlı güvenlik grubundaki bir hizmet hesabının kimlik bilgilerini kullanarak Azure Multi-Factor Authentication Web Hizmeti SDK’sının kimliğini doğrulayabildiğinden emin olun. Azure Multi-Factor Authentication Sunucusu etki alanı ile birleşik bir sunucuda çalışıyorsa, bu hizmet hesabı ve grubu Active Directory’de yer alır. Bir etki alanı ile birleştirilmediyse, bu hizmet hesabı ve grubu yerel olarak Azure Multi-Factor Authentication Sunucusu’nda yer alır.

Azure Multi-Factor Authentication Sunucusu dışında bir sunucuya kullanıcı portalını yüklemek aşağıdaki adımları gerektirir:

1. **MFA sunucusunda**, yükleme yoluna göz atın (örneğin: C:\Program Files\Multi-Factor Authentication Server) ve dosyayı **MultiFactorAuthenticationUserPortalSetup64** yükleyeceğiniz bu konuma internet'e yönelik sunucuya erişilebilir.
2. **İnternet'e yönelik web sunucusunda**, MultiFactorAuthenticationUserPortalSetup64 yükleme dosyasını bir yönetici olarak çalıştırın, isterseniz Site'yi değiştirin ve sanal dizini kısa bir adla değiştirin.
3. IIS'de siteye bir SSL sertifikası bağlayın.

   > [!NOTE]
   > Bu SSL Sertifikası çoğunlukla genel olarak imzalanmış bir SSL sertifikasıdır.

4. **C:\inetpub\wwwroot\MultiFactorAuth** dizinine gidin.
5. Web.Config dosyasını Not Defteri'nde düzenleme

    * **"USE_WEB_SERVICE_SDK"** anahtarını bulun ve **value="false"** değerini **value="true"** değeriyle değiştirin.
    * **"WEB_SERVICE_SDK_AUTHENTICATION_USERNAME"** anahtarını bulun ve **value=""** değerini **value="DOMAIN\User"** değeriyle değiştirin. Burada DOMAIN\User, "PhoneFactor Admins" grubunun parçası olan bir Hizmet Hesabıdır.
    * **"WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD"** anahtarını bulun ve **value=""** değerini **value="Password"** ile değiştirin. Burada Password, önceki satırda girdiğiniz Hizmet Hesabının parolasıdır.
    * **https://www.contoso.com/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx** değerini bulun ve bu yer tutucu URL’yi 2. adımda yüklediğimiz Web Hizmeti SDK URL’siyle değiştirin.
    * Web.Config dosyasını kaydedin ve Not Defteri'ni kapatın.

6. Herhangi bir bilgisayarda web tarayıcısını açın ve kullanıcı portalının yüklendiği URL’ye (Örnek: https://mfa.contoso.com/MultiFactorAuth)) gidin. Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.

IIS sunucusunda bir SSL sertifikası yapılandırma hakkında sorularınız varsa bkz. [IIS'de SSL ayarlama](https://docs.microsoft.com/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="configure-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nda kullanıcı portalı ayarlarını yapılandırma

Artık kullanıcı portalı yüklendiğine göre, portal ile çalışmak için Azure Multi-Factor Authentication Sunucusu’nu yapılandırmalısınız.

1. Azure Multi-Factor Authentication Sunucusu konsolunda **Kullanıcı Portalı** simgesine tıklayın. Ayarlar sekmesinde, **Kullanıcı Portalı URL’si** metin kutusuna kullanıcı portalı URL’sini girin. E-posta işlevi etkinleştirilmişse, bu URL Azure Multi-Factor Authentication Sunucusu’na aktarıldıkları zaman kullanıcılara gönderilen e-postalara eklenir.
2. Kullanıcı Portalı'nda kullanmak istediğiniz ayarları seçin. Örneğin, kullanıcıların kendi kimlik doğrulama yöntemlerini seçmesine izin vermek için, seçim yapabilecekleri yöntemlerle birlikte **Kullanıcıların yöntemi seçmesine izin ver** seçeneğinin işaretli olduğundan emin olun.
3. Kimlerin yönetici olacağını **Yöneticiler** sekmesinde belirleyin. Ekle/Düzenle kutularında onay kutularını ve açılan menüleri kullanarak ayrıntılı yönetim izinlerini oluşturabilirsiniz.

İsteğe bağlı yapılandırma:

- **Güvenlik Soruları** -Ortamınız için güvenlik sorularını ve bu soruların hangi dilde olacağını belirleyin.
- **Geçmiş Oturumlar** -MFA kullanarak bir form tabanlı web sitesi ile kullanıcı portalı tümleştirmesini yapılandırın.
- **Güvenilen IP'ler** -Kullanıcıların güvenilen IP'ler veya aralıklar listesinden kimlik doğrulaması yapıldığı sırada MFA’yı atlamasına olanak verir.

![MFA Sunucusu Kullanıcı Portalı yapılandırması](./media/howto-mfaserver-deploy-userportal/config.png)

Azure Multi-Factor Authentication Sunucusu kullanıcı portalı için çeşitli seçenekler sunar. Aşağıdaki tabloda bu seçeneklerin ve ne için kullanıldıklarının açıklamasının bir listesi verilmiştir.

| Kullanıcı Portalı Ayarları | Açıklama |
|:--- |:--- |
| Kullanıcı Portalı URL’si | Portalın barındırıldığı URL’yi girin. |
| Birincil kimlik doğrulama | Portalda oturum açarken kullanılacak kimlik doğrulama türünü belirtin. Windows, Radius veya LDAP kimlik doğrulaması. |
| Kullanıcıların oturum açmasına izin verir | Kullanıcıların, Kullanıcı portalı oturum açma sayfasında kullanıcı adı ve parola girmesini sağlar. Bu seçenek seçilmezse, kutuları gri görünür. |
| Kullanıcı kaydına izin ver | Onları telefon numarası gibi ek bilgi isteyen bir kurulum ekranına getirerek kullanıcıların Multi-Factor Authentication’a kaydolmasını sağlar. Yedek telefon numarası istemi kullanıcıların ikincil bir telefon numarası belirtmesine izin verir. Üçüncü taraf OATH belirteci istemi kullanıcıların bir üçüncü tarafa OATH belirteci belirtmesine izin verir. |
| Kullanıcıların Bir Kerelik geçiş başlatmasına izin ver | Kullanıcıların bir kerelik geçiş başlatmasına izin verir. Kullanıcı bu seçeneği değiştirirse, kullanıcının sonraki oturum açışında etkili olur. Atlama (saniye) kullanıcıya 300 saniye olan varsayılan değeri değiştirebilecekleri bir kutu sağlar. Aksi halde, bir kerelik atlama yalnızca 300 saniye geçerlidir. |
| Kullanıcıların yöntemi seçmesine izin ver | Kullanıcıların kendi birincil bağlantı kurma yöntemini belirtmesine olanak tanır. Bu yöntem telefon araması, SMS, mobil uygulama veya OATH belirteci olabilir. |
| Kullanıcıların dili seçmesine izin ver | Kullanıcının telefon araması, SMS, mobil uygulama veya OATH belirteci için kullanılan dili seçmesine olanak sağlar. |
| Kullanıcıların mobil uygulama etkinleştirmesine izin ver | Kullanıcıların sunucuyla birlikte kullanılan mobil uygulama etkinleştirme işlemini tamamlamak üzere bir etkinleştirme kodu seçmesini sağlar.  Ayrıca bunu etkinleştirebilecekleri cihaz sayısını da (1-10 arası) ayarlayabilirsiniz. |
| Geri dönüş için güvenlik sorularını kullan | İki aşamalı doğrulamanın başarısız olması halinde güvenlik sorularının kullanılmasını sağlar. Başarılı bir şekilde yanıtlanması gereken güvenlik sorusu sayısını belirtebilirsiniz. |
| Kullanıcıların üçüncü taraf OATH belirteci ilişkilendirmesine izin ver | Kullanıcıların üçüncü taraf OATH belirteci ilişkilendirmesine izin verir. |
| Geri dönüş için OATH belirtecini kullan | İki aşamalı doğrulamanın başarısız olması halinde OATH belirtecinin kullanılmasını sağlar. Dakika cinsinden oturum zaman aşımı süresini de belirtebilirsiniz. |
| Günlü kaydını etkinleştir | Kullanıcı portalında günlük kaydını etkinleştirir. Günlük dosyaları şu adreste bulunabilir: C:\Program Files\Multi-Factor Authentication Server\Logs. |

> [!IMPORTANT]
> Telefon araması seçenekleri 2019'ın Mart başlangıç MFA sunucusu kullanıcıları Azure AD ücretsiz/deneme kiracılar için kullanılabilir olmayacak. SMS iletileri, bu değişiklikten etkilenmez. Telefon Araması'nda kullanıcılara kullanılabilir Ücretli Azure AD kiracılarıyla devam eder. Bu değişiklik, yalnızca Azure AD ücretsiz/deneme kiracıları etkiler.

Bu ayarlar, etkinleştirildiklerinde ve kullanıcı, kullanıcı portalında oturum açtığında portalda görünür.

![Kullanıcı Portalı'nı kullanarak MFA sunucusu hesabınızı yönetin](./media/howto-mfaserver-deploy-userportal/portalsettings.png)

### <a name="self-service-user-enrollment"></a>Self servis kullanıcı kaydı

Kullanıcılarının oturum açmasını ve kaydolmasını istiyorsanız, Ayarlar sekmesinde **Kullanıcıların oturum açmasına izin ver** ve **Kullanıcı kaydına izin** ver seçeneklerini belirlemeniz gerekir. Seçtiğiniz ayarların kullanıcı oturum açma deneyimini etkileyeceğini unutmayın.

Örneğin bir kullanıcı, kullanıcı portalında ilk kez oturum açtığında Azure Multi-Factor Authentication Kullanıcı Kurulumu sayfasına yönlendirilir. Azure Multi-Factor Authentication’ı nasıl yapılandırdığınıza bağlı olarak, kullanıcı kendi kimlik doğrulama yöntemini seçebilir.

Sesli Arama doğrulama yöntemini seçerse ya da bu yöntemi kullanmak üzere önceden yapılandırıldıysa, sayfa kullanıcıdan birincil telefonu numarasını ve varsa dahili numarayı girmesini ister. Ayrıca kullanıcının yedek telefon numarası girmesine de izin verilir.

![Birincil ve yedek telefon numaralarını kaydetme](./media/howto-mfaserver-deploy-userportal/backupphone.png)

Kimlik doğrulaması sırasında kullanıcının PIN kullanması gerekiyorsa, sayfa kullanıcıdan PIN oluşturmasını da ister. Kendi telefon numaralarını ve PIN’i (varsa) girdikten sonra, kullanıcı **Kimlik Doğrulaması için şimdi Beni Ara** düğmesine tıklar. Azure Multi-Factor Authentication kullanıcının birincil telefon numarasına doğrulama amaçlı bir telefon araması yapar. Kullanıcı aramayı yanıtlamalı ve PIN’ini girmeli (varsa) ve self servis kayıt işleminin sonraki adımına geçmek için # tuşuna basmalıdır.

Kullanıcı Kısa Mesaj doğrulama yöntemini seçerse ya da bu yöntemi kullanmak üzere önceden yapılandırıldıysa, sayfa kullanıcıdan birincil telefonu numarasını ister. Kimlik doğrulaması sırasında kullanıcının PIN kullanması gerekiyorsa, sayfa kullanıcıdan PIN oluşturmasını da ister.  Kendi telefon numarasını ve PIN’ini (varsa) girdikten sonra, kullanıcı **Kimlik Doğrulaması için şimdi Bana SMS gönder** düğmesine tıklar. Azure Multi-Factor Authentication kullanıcısının mobil uygulamasına bir SMS kimlik doğrulaması yapar. Kullanıcı bir kerelik geçiş kodu (OTP) içeren kısa mesaj alır ve bu mesajı OTP artı PIN kodu (varsa) ile yanıtlar.

![SMS kullanarak kullanıcı portalı doğrulama](./media/howto-mfaserver-deploy-userportal/text.png)

Kullanıcı, Mobil Uygulama ile doğrulama yöntemini seçerse sayfa, kullanıcıdan cihazına Microsoft Authenticator uygulamasını yüklemesini ve bir etkinleştirme kodu oluşturmasını ister. Kullanıcı uygulamayı yükledikten sonra Etkinleştirme Kodu Oluştur düğmesine tıklar.

> [!NOTE]
> Microsoft Authenticator uygulamasını kullanmak için kullanıcının kendi cihazı için anında iletme bildirimlerini etkinleştirmesi gerekir.

Böylece sayfada bir barkod resmi ile birlikte, etkinleştirme kodu ve bir URL görüntülenir. Kimlik doğrulaması sırasında kullanıcının PIN kullanması gerekiyorsa, sayfa kullanıcıdan PIN oluşturmasını da ister. Kullanıcı Microsoft Authenticator uygulamasına etkinleştirme kodunu ve URL’yi girer ya da barkod resmini taramak için barkod tarayıcısını kullanır ve Etkinleştir düğmesine tıklar.

Etkinleştirme tamamlandıktan sonra, kullanıcı **Şimdi Kimliğimi Doğrula** düğmesine tıklar. Azure Multi-Factor Authentication kullanıcısının mobil uygulamasına bir kimlik doğrulaması yapar. Kullanıcı PIN’ini girmeli (varsa) ve self servis kayıt işleminin sonraki adımına geçmek için mobil uygulamasında Kimliği Doğrula düğmesini basmalıdır.

Yöneticiler Azure Multi-Factor Authentication Sunucusu’nu güvenlik soruları ve yanıtları toplamak üzere yapılandırmışsa, kullanıcı Güvenlik Soruları sayfasına yönlendirilir. Kullanıcı dört güvenlik sorusu seçmeli ve seçtiği sorulara yanıt vermelidir.

![Kullanıcı portalı güvenlik soruları](./media/howto-mfaserver-deploy-userportal/secq.png)

Kullanıcı self servis kayıt işlemi artık tamamlanmış ve kullanıcı, kullanıcı portalında oturum açmıştır. Kullanıcılar, yöneticileri yöntemi değiştirmelerine izin vermişse, gelecekte istedikleri zaman telefon numarası, PIN, kimlik doğrulama yöntemi ve güvenlik sorularını değiştirmek için kullanıcı portalına dönebilirler.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication Sunucusu Mobil Uygulama Web Hizmeti’ni dağıtma](howto-mfaserver-deploy-mobileapp.md)
