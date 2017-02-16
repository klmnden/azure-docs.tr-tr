---
title: "Azure MFA Sunucusu kullanıcı portalı | Microsoft Belgeleri"
description: "Bu, nasıl Azure MFA ve kullanıcı portalını kullanmaya başlayacağınızı açıklayan Azure Multi-factor authentication sayfasıdır."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/04/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 1222223f8c45249402bfdd04c8754074f877e132
ms.openlocfilehash: 1236489212b2a9c421972599a12511d5bc42efdf


---
# <a name="deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu için kullanıcı portalını dağıtma
Kullanıcı portalı yöneticinin Azure Multi-Factor Authentication Kullanıcı Portalı’nı yüklemesine ve yapılandırmasına olanak tanır. Kullanıcı portalı, kullanıcıların Azure Multi-Factor Authentication’a kaydolmasını ve hesaplarını korumalarını sağlayan bir IIS web sitesidir. Bir kullanıcı, sonraki oturum açışı sırasında telefon numarasını, PIN’ini değiştirebilir ya da iki aşamalı doğrulamayı atlayabilir.

Kullanıcılar kendi normal kullanıcı adı ve parolalarını kullanarak Kullanıcı Portalı’nda oturum açar ve kendi kimlik doğrulamalarını tamamlamak için iki aşamalı doğrulama çağrısını tamamlar veya güvenlik sorularını yanıtlar. Kullanıcı kaydına izin veriliyorsa, kullanıcı ilk kez kullanıcı portalında oturum açtığında kendi telefon numarasını ve PIN’ini yapılandırır.

Kullanıcı Portalı Yöneticileri yeni kullanıcı eklemek ve mevcut kullanıcıları güncelleştirmek üzere ayarlanabilir ve izin verilebilir.

<center>![Kurulum](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploy-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile aynı sunucuda kullanıcı portalını dağıtma
Aşağıdaki ön koşullar kullanıcı portalını Azure Multi-Factor Authentication Sunucusu ile aynı sunucuya yüklemek için gereklidir:

* asp.net ve IIS 6 meta tabanı uyumluluğu (IIS 7 ya da üst sürümü için) dahil IIS
* Oturum açmış kullanıcının, varsa bilgisayar ve Etki Alanı yönetici hakları olması gerekir.  Bunun nedeni hesabın Active Directory güvenlik grupları oluşturmak için izin gerektirmesidir.

### <a name="to-deploy-the-user-portal"></a>Kullanıcı portalını dağıtmak için
1. Azure Multi-Factor Authentication Sunucusu’nda: soldaki menüde **Kullanıcı Portalı** simgesine ve ardından **Kullanıcı Portalını Yükle**’ye tıklayın.
2. **Next (İleri)** düğmesine tıklayın.
3. **Next (İleri)** düğmesine tıklayın.
4. Bilgisayar bir etki alanına katıldıysa ve kullanıcı portalı ile Azure Multi-Factor Authentication hizmeti arasındaki hizmeti güvenli hale getirmek üzere Active Directory yapılandırması yapılmamışsa, Active Directory adımı görüntülenir. Otomatik olarak bu yapılandırmayı tamamlamak için **İleri** düğmesine tıklayın.
5. **İleri**’ye tıklayın.
6. **Next (İleri)** düğmesine tıklayın.
7. **Kapat**’a tıklayın.
8. Herhangi bir bilgisayarda web tarayıcısını açın ve kullanıcı portalının yüklendiği URL’ye gidin (örn. https://www.publicwebsite.com/MultiFactorAuth). Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.

<center>![Kurulum](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploy-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>Azure Multi-Factor Authentication Sunucusu Kullanıcı Portalını Farklı Sunucuda dağıtma
Microsoft Authenticator uygulamasının kullanıcı portalıyla iletişim kurmasına izin vermek için aşağıdaki gereksinimleri tamamlayın: 

* Azure Multi-Factor Authentication Sunucusu’nun v6.0 ya da üst sürümünü kullanıyor olmalısınız.
* Kullanıcı portalının, Microsoft® Internet Information Services (IIS) 6.x, IIS 7.x veya üst sürümünü çalıştıran bir İnternete yönelik web sunucusunda yüklü olması gerekir.
* IIS 6.x kullanırken, ASP.NET v2.0.50727’nin yüklü, kayıtlı ve **İzinli** olarak ayarlandığından emin olun.
* IIS 7.x ya da üst sürümünü kullanırken gerekli rol hizmetleri ASP.NET ve IIS 6 Metatabanı Uyumluluğu’nu içerir.
* Kullanıcı portalı bir SSL sertifikası ile güvenli hale getirilmelidir.
* Azure Multi-Factor Authentication Web Hizmeti SDK’sı, Azure Multi-Factor Authentication Sunucusu’nun yüklendiği sunucuda IIS 6.x, IIS 7.x ya da üst sürümünde yüklenmelidir.
* Azure Multi-Factor Authentication Web Hizmeti SDK’sı bir SSL sertifikası ile güvenli hale getirilmelidir.
* Kullanıcı portalı SSL üzerinden Azure Multi-Factor Authentication Web Hizmeti SDK’sına bağlanabilmelidir.
* Kullanıcı portalı, "PhoneFactor Admins" adlı güvenlik grubundaki bir hizmet hesabının kimlik bilgilerini kullanarak Azure Multi-Factor Authentication Web Hizmeti SDK’sının kimliğini doğrulayabilmelidir. Azure Multi-Factor Authentication Sunucusu etki alanı ile birleşik bir sunucuda çalışıyorsa, bu hizmet hesabı ve grubu Active Directory’de yer alır. Bir etki alanı ile birleştirilmediyse, bu hizmet hesabı ve grubu yerel olarak Azure Multi-Factor Authentication Sunucusu’nda yer alır.

Azure Multi-Factor Authentication Sunucusu dışında bir sunucuya kullanıcı portalını yüklemek aşağıdaki üç adımı gerektirir:

1. Web hizmeti SDK’sını yükleme
2. Kullanıcı portalını yükleme
3. Azure Multi-Factor Authentication Sunucusu’nda Kullanıcı Portalı Ayarlarını yapılandırma

### <a name="step-1-install-the-web-service-sdk"></a>1. Adım: Web hizmeti SDK’sını yükleme
Azure Multi-Factor Authentication Web Hizmeti SDK’sı Azure Multi-Factor Authentication Sunucusu’nda halihazırda yüklü değilse, bu sunucuya gidin ve Azure Multi-Factor Authentication Sunucusu’nu açın. **Web Hizmeti SDK’sı** simgesine ve ardından **Web Hizmeti SDK’sını Yükle**’ye tıklayın. Verilen talimatları uygulayın. 

Web Hizmeti SDK’sı bir SSL sertifikası ile güvenli hale getirilmelidir. Kendinden imzalı bir sertifika bu amaç doğrultusunda kabul edilebilir ancak sunucudaki Yerel Bilgisayar hesabının "Güvenilen Kök Sertifika Yetkilileri" deposuna aktarılmalıdır. Web Hizmeti SDK’sı artık SSL bağlantısını başlatırken bu sertifikaya güvenebilir.

<center>![Kurulum](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="step-2-install-the-user-portal"></a>2. Adım: Kullanıcı portalını yükleme
Kullanıcı portalını ayrı bir sunucuda yüklemeden önce, aşağıdaki en iyi yöntemleri inceleyin:

* İnternet'e yönelik web sunucusunda bir web tarayıcısı açmak ve web.config dosyasına girilen Web hizmeti SDK’sının URL’sine gitmek faydalıdır. Tarayıcı web hizmetine başarıyla gidebilirse, sizden kimlik bilgilerinizi ister. Aynen dosyada göründüğü gibi web.config dosyasına girilen parola girilen kullanıcı adını ve parolayı girin. Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.
* Kullanıcı portalı web sunucusunun önünde ters proxy ya da güvenlik duvarı yer alıyorsa ve SSL boşaltma gerçekleştiriyorsa, Kullanıcı Portalı’nın https yerine http kullanabilmesi için, kullanıcı portalı web.config dosyasını düzenleyebilir ve aşağıdaki anahtarı `<appSettings>` bölümüne ekleyebilirsiniz:

    `<add key="SSL_REQUIRED" value="false"/>`

#### <a name="to-install-the-user-portal"></a>Kullanıcı portalını yüklemek için
1. Azure Multi-Factor Authentication Sunucusu’nda Windows Gezgini'ni açın ve Azure Multi-Factor Authentication Sunucusu’nun yüklü olduğu klasöre gidin (örneğin, :\Program Files\Multi-Factor Authentication Server). Kullanıcı Portalı’nın yüklü olduğu sunucu için uygun şekilde MultiFactorAuthenticationUserPortalSetup yükleme dosyasının 32-bit veya 64-bit sürümünü seçin. Yükleme dosyasını İnternet’e yönelik sunucuya kopyalayın.
2. İnternet’e yönelik sunucuda, kurulum dosyası yönetici haklarıyla çalıştırılmalıdır. Bunu yapmanın en kolay yolu, yönetici olarak bir komut istemi açmak ve yükleme dosyasının kopyalandığı konuma gitmektir.
3. MultiFactorAuthenticationUserPortalSetup64 yükleme dosyasını çalıştırın, isterseniz Site’yi ve Sanal Dizin’i değiştirin.
4. Kullanıcı Portalı yüklenmesi tamamlandıktan, sonraki C:\inetpub\wwwroot\MultiFactorAuth (veya sanal dizin adını temel alarak uygun dizin) gidin ve web.config dosyasını düzenleyin.
5. USE_WEB_SERVICE_SDK anahtarını bulun ve değeri false iken true olarak değiştirin. WEB_SERVICE_SDK_AUTHENTICATION_USERNAME ve WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD anahtarlarını bulun ve değerleri PhoneFactor Admins güvenlik grubundaki hizmet hesabının kullanıcı adı ve parolası olarak ayarlayın (Gereksinimler bölümüne bakın). Satırın sonundaki tırnak işaretlerinin arasına, (value=””/>) Kullanıcı Adı ve Parolayı girdiğinizden emin olun. Tam kullanıcı adı kullanmanız gerekir (örneğin, etki alanı\kullanıcı adı veya makine\kullanıcı adı)
6. pfup_pfwssdk_PfWsSdk ayarını bulun ve “http://localhost:4898/PfWsSdk.asmx” değerini Azure Multi-Factor Authentication Sunucusu’nda çalışan Web Hizmeti SDK’sının URL’si ile değiştirin (örneğin, https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Bu bağlantı için SSL kullanıldığından ve SSL sertifikası sunucu adına düzenlendiğinden Web Hizmeti SDK’sına IP adresi değil sunucu adıyla başvurun. Sunucu, İnternet’e yönelik sunucudan gelen bir IP adresini çözümlemezse, Azure Multi-Factor Authentication Sunucusu’nun adını bu IP adresine eşlemek için bu sunucudaki hosts dosyasına bir giriş ekleyin. Değişiklikler yapıldıktan sonra web.config dosyasını kaydedin.

    config dosyasını düzenleme hakkında daha fazla bilgi için [Azure Multi-Factor Authentication Sunucusunu AD FS ile birlikte kullanarak kaynaklarınızın güvenliğini sağlama](multi-factor-authentication-get-started-adfs-w2k12.md#edit-the-multifactorauthenticationadfsadapterconfig-file) sayfasına gidin.

7. Kullanıcı portalının altında yüklendiği web sitesi (örneğin Varsayılan Web Sitesi) halihazırda ortak olarak imzalanmış bir sertifikayla bağlanmadıysa, sertifikayı sunucuya yükleyin, IIS Yöneticisi’ni açın ve sertifikayı web sitesine bağlayın.
8. Herhangi bir bilgisayarda web tarayıcısını açın ve Kullanıcı Portalı’nın yüklendiği URL’ye gidin (örn. https://www.publicwebsite.com/MultiFactorAuth). Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.

### <a name="step-3-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>3. Adım: Azure Multi-Factor Authentication Sunucusu’nda kullanıcı portalı ayarlarını yapılandırma
Artık kullanıcı portalı yüklendiğine göre, portal ile çalışmak için Azure Multi-Factor Authentication Sunucusu’nu yapılandırmalısınız.

Azure Multi-Factor Authentication Sunucusu kullanıcı portalı için çeşitli seçenekler sunar.  Aşağıdaki tabloda bu seçeneklerin ve ne için kullanıldıklarının açıklamasının bir listesi verilmiştir.

| Kullanıcı Portalı Ayarları | Açıklama |
|:--- |:--- |
| Kullanıcı Portalı URL’si | Portalın barındırıldığı URL’yi girin. |
| Birincil kimlik doğrulama | Portalda oturum açarken kullanılacak kimlik doğrulama türünü belirtin.  Windows, Radius veya LDAP kimlik doğrulaması. |
| Kullanıcıların oturum açmasına izin verir | Kullanıcıların, Kullanıcı portalı oturum açma sayfasında kullanıcı adı ve parola girmesini sağlar.  Bu seçilmezse, kutuları gri görünür. |
| Kullanıcı kaydına izin ver | Onları telefon numarası gibi ek bilgi isteyen bir kurulum ekranına getirerek kullanıcıların Multi-Factor Authentication’a kaydolmasını sağlar. Yedek telefon numarası istemi kullanıcıların ikincil bir telefon numarası belirtmesine izin verir. Üçüncü taraf OATH belirteci istemi kullanıcıların bir üçüncü tarafa OATH belirteci belirtmesine izin verir. |
| Kullanıcıların Bir Kerelik geçiş başlatmasına izin ver | Kullanıcıların bir kerelik geçiş başlatmasına izin verir.  Kullanıcı bunu ayarlarsa, kullanıcının sonraki oturum açışında etkili olur. Atlama (saniye) kullanıcıya 300 saniye olan varsayılan değeri değiştirebilecekleri bir kutu sağlar. Aksi halde, bir kerelik atlama yalnızca 300 saniye geçerlidir. |
| Kullanıcıların yöntemi seçmesine izin ver | Kullanıcıların kendi birincil bağlantı kurma yöntemini belirtmesine olanak tanır.  Bu telefon araması, SMS, mobil uygulama veya OATH belirteci olabilir. |
| Kullanıcıların dili seçmesine izin ver | Kullanıcının telefon araması, SMS, mobil uygulama veya OATH belirteci için kullanılan dili seçmesine olanak sağlar. |
| Kullanıcıların mobil uygulama etkinleştirmesine izin ver | Kullanıcıların sunucuyla birlikte kullanılan mobil uygulama etkinleştirme işlemini tamamlamak üzere bir etkinleştirme kodu seçmesini sağlar.  Ayrıca bunu etkinleştirebilecekleri cihaz sayısını da (1-10 arası) ayarlayabilirsiniz. |
| Geri dönüş için güvenlik sorularını kullan | İki aşamalı doğrulamanın başarısız olması halinde güvenlik sorularının kullanılmasını sağlar.  Başarılı bir şekilde yanıtlanması gereken güvenlik sorusu sayısını belirtebilirsiniz. |
| Kullanıcıların üçüncü taraf OATH belirteci ilişkilendirmesine izin ver | Kullanıcıların üçüncü taraf OATH belirteci ilişkilendirmesine izin verir. |
| Geri dönüş için OATH belirtecini kullan | İki aşamalı doğrulamanın başarısız olması halinde OATH belirtecinin kullanılmasını sağlar. Dakika cinsinden oturum zaman aşımı süresini de belirtebilirsiniz. |
| Günlü kaydını etkinleştir | Kullanıcı portalında günlük kaydını etkinleştirir. Günlük dosyaları şu adreste bulunabilir: C:\Program Files\Multi-Factor Authentication Server\Logs. |

Bu ayarların çoğu, etkinleştirildiklerinde ve kullanıcı, kullanıcı portalında oturum açtığında görünür.

![Kullanıcı portalı ayarları](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu’nda kullanıcı portalı ayarlarını yapılandırmak için
1. Azure Multi-Factor Authentication Sunucusu’nda **Kullanıcı Portalı** simgesine tıklayın. Ayarlar sekmesinde, **Kullanıcı Portalı URL’si** metin kutusuna kullanıcı portalı URL’sini girin. Bu URL, e-posta işlevi etkinleştirildiğinde, Azure Multi-Factor Authentication Sunucusu’na aktarıldıkları zaman kullanıcılara gönderilen e-postalara eklenir.
2. Kullanıcı Portalı'nda kullanmak istediğiniz ayarları seçin. Örneğin, kullanıcıların kendi kimlik doğrulama yöntemlerini denetlemesine izin vermek için, seçim yapabilecekleri yöntemlerle birlikte** Kullanıcıların yöntemi seçmesine izin ver** seçeneğinin seçili olduğundan emin olun.
3. Görüntülenen ayarları anlamaya ilişkin yardım için sağ üst köşedeki **Yardım** bağlantısına tıklayın.

<center>![Kurulum](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Yöneticiler sekmesi
Bu sekme yalnızca yönetici ayrıcalıklarına sahip olacak kullanıcıları eklemenizi sağlar.  Bir yönetici eklerken, aldıkları izinleri ayrıntılı olarak ayarlayabilirsiniz. **Ekle** düğmesine tıklayın, kullanıcı ve izinlerini seçin ve ardından **Ekle**’ye tıklayın.

![Kullanıcı portalı yöneticileri](./media/multi-factor-authentication-get-started-portal/admin.png)

## <a name="security-questions"></a>Güvenlik Soruları
Bu sekme, **Geri dönüş için güvenlik sorularını kullan** seçeneği seçili ise, kullanıcıların yanıtları sağlaması gereken güvenlik sorularını belirtmenizi sağlar.  Azure Multi-Factor Authentication Sunucusu kullanabileceğiniz varsayılan sorularla birlikte gelir. Soruların sırasını değiştirebilir veya kendi sorularınızı ekleyebilirsiniz.  Kendi sorularınızı eklerken, bu soruların görünmesini istediğiniz dili de belirtebilirsiniz.

![Kullanıcı portalı güvenlik soruları](./media/multi-factor-authentication-get-started-portal/secquestion.png)

## <a name="saml"></a>SAML
SAML kullanarak bir kimlik sağlayıcısından gelen talepleri kabul etmek için kullanıcı portalını yapılandırma amacıyla bu sekmeyi kullanabilirsiniz.  Oturum zaman aşımını, doğrulama sertifikasını ve Oturumu kapatma yönlendirme URL’sini belirtebilirsiniz.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Güvenilen IP'ler
Bu sekme, bir kullanıcı bu adreslerden birinden oturum açarsa, iki aşamalı doğrulamayı tamamlaması gerekmeyecek şekilde eklenebilecek tek bir IP adresi ya da IP adresleri aralığı belirtmenize olanak tanır.

![Kullanıcı portalı güvenilen IP'leri](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Self Servis Kullanıcı Kaydı
Kullanıcılarının oturum açmasını ve kaydolmasını istiyorsanız, Ayarlar sekmesinde **Kullanıcıların oturum açmasına izin ver** ve **Kullanıcı kaydına izin** ver seçeneklerini belirlemeniz gerekir. Seçtiğiniz ayarların kullanıcı oturum açma deneyimini etkileyeceğini unutmayın.

Örneğin bir kullanıcı, kullanıcı portalında ilk kez oturum açtığında Azure Multi-Factor Authentication Kullanıcı Kurulumu sayfasına yönlendirilir.  Azure Multi-Factor Authentication’ı nasıl yapılandırdığınıza bağlı olarak, kullanıcı kendi kimlik doğrulama yöntemini seçebilir.  

Sesli Arama doğrulama yöntemini seçerse ya da bu yöntemi kullanmak üzere önceden yapılandırıldıysa, sayfa kullanıcıdan birincil telefonu numarasını ve varsa dahili numarayı girmesini ister.  Ayrıca kullanıcının yedek telefon numarası girmesine de izin verilir.  

![Kullanıcı portalı güvenilen IP'leri](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Kimlik doğrulaması sırasında kullanıcının PIN kullanması gerekiyorsa, sayfa kullanıcıdan PIN oluşturmasını da ister.  Kendi telefon numaralarını ve PIN’i (varsa) girdikten sonra, kullanıcı **Kimlik Doğrulaması için şimdi Beni Ara** düğmesine tıklar.  Azure Multi-Factor Authentication kullanıcının birincil telefon numarasına doğrulama amaçlı bir telefon araması yapar.  Kullanıcı aramayı yanıtlamalı ve PIN’ini girmeli (varsa) ve self servis kayıt işleminin sonraki adımına geçmek için # tuşuna basmalıdır.   

Kullanıcı Kısa Mesaj doğrulama yöntemini seçerse ya da bu yöntemi kullanmak üzere önceden yapılandırıldıysa, sayfa kullanıcıdan birincil telefonu numarasını ister.  Kimlik doğrulaması sırasında kullanıcının PIN kullanması gerekiyorsa, sayfa kullanıcıdan PIN’i girmesini de ister.  Kendi telefon numarasını ve PIN’ini (varsa) girdikten sonra, kullanıcı **Kimlik Doğrulaması için şimdi Bana SMS gönder** düğmesine tıklar.  Azure Multi-Factor Authentication kullanıcısının mobil uygulamasına bir SMS kimlik doğrulaması yapar.  Kullanıcı bir kerelik geçiş kodu (OTP) içeren kısa mesaj alır ve bu mesajı OTP artı PIN kodu (varsa) ile yanıtlar.

![Kullanıcı portalı SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

Kullanıcı, Mobil Uygulama ile doğrulama yöntemini seçerse sayfa, kullanıcıdan cihazına Microsoft Authenticator uygulamasını yüklemesini ve bir etkinleştirme kodu oluşturmasını ister.  Kullanıcı uygulamayı yükledikten sonra Etkinleştirme Kodu Oluştur düğmesine tıklar.    

> [!NOTE]
> Microsoft Authenticator uygulamasını kullanmak için kullanıcının kendi cihazı için anında iletme bildirimlerini etkinleştirmesi gerekir.

Böylece sayfada bir barkod resmi ile birlikte, etkinleştirme kodu ve bir URL görüntülenir.  Kimlik doğrulaması sırasında kullanıcının PIN kullanması gerekiyorsa, sayfa kullanıcıdan PIN’i girmesini de ister.  Kullanıcı Microsoft Authenticator uygulamasına etkinleştirme kodunu ve URL’yi girer ya da barkod resmini taramak için barkod tarayıcısını kullanır ve Etkinleştir düğmesine tıklar.    

Etkinleştirme tamamlandıktan sonra, kullanıcı **Şimdi Kimliğimi Doğrula** düğmesine tıklar.  Azure Multi-Factor Authentication, kullanıcının mobil uygulaması için doğrulama işlemi gerçekleştirir.  Kullanıcı PIN’ini girmeli (varsa) ve self servis kayıt işleminin sonraki adımına geçmek için mobil uygulamasında Kimliği Doğrula düğmesini basmalıdır.  

Yöneticiler Azure Multi-Factor Authentication Sunucusu’nu güvenlik soruları ve yanıtları toplamak üzere yapılandırmışsa, kullanıcı Güvenlik Soruları sayfasına yönlendirilir.  Kullanıcı dört güvenlik sorusu seçmeli ve seçtiği sorulara yanıt vermelidir.    

![Kullanıcı portalı güvenlik soruları](./media/multi-factor-authentication-get-started-portal/secq.png)  

Kullanıcı self servis kayıt işlemi artık tamamlanmış ve kullanıcı, kullanıcı portalında oturum açmıştır.  Kullanıcılar, yöneticileri izin vermişse, gelecekte istedikleri zaman telefon numarası, PIN, kimlik doğrulama yöntemi ve güvenlik sorularını değiştirmek için kullanıcı portalına dönebilirler.




<!--HONumber=Feb17_HO3-->


