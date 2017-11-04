---
title: "Azure MFA yapılandırma | Microsoft Docs"
description: "Bu yapılacaklar sonraki MFA ile açıklayan Azure multi-Factor authentication sayfasıdır.  Bu raporlar, sahtekarlık Uyarısı, bir kerelik atlama, özel sesli mesajları, önbelleğe alma, güvenilen IP'leri ve uygulama parolaları içerir."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: yossib
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: joflore
ms.openlocfilehash: 527bdd492561ab11784a0b23384d17e055cb3f5c
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure çok faktörlü kimlik doğrulama ayarlarını yapılandırın
Bu makalede Azure çok faktörlü kimlik doğrulaması ve çalışıyor olduğundan göre yönetmenize yardımcı olur.  En Azure multi-Factor Authentication dışında elde size yardımcı çeşitli konuları kapsar.  Tüm bu özellikler Azure çok faktörlü kimlik doğrulaması her sürümünde kullanılabilir.

| Özellik | Açıklama | 
|:--- |:--- |
| [Sahtekarlık Uyarısı](#fraud-alert) |Sahtekarlık uyarısı yapılandırılmış ve böylece kullanıcılarınızın kaynaklarına erişmek için sahte denemeleri raporlayabilirsiniz ayarlayın. |
| [Bir kerelik atlama](#one-time-bypass) |Bir kerelik geçiş "çok faktörlü kimlik doğrulamasını atlayarak" bir kereliğine kimlik doğrulaması sağlar. |
| [Özel sesli mesajları](#custom-voice-messages) |Özel sesli mesajları kendi kayıtları veya Tebrikler çok faktörlü kimlik doğrulama kullanmanıza olanak sağlar. |
| [Önbelleğe alma](#caching-in-azure-multi-factor-authentication) |Önbelleğe alma, sonraki kimlik doğrulama girişimleri otomatik olarak başarılı belirli bir zaman aralığı belirlemenizi sağlar. |
| [Güvenilen IP'leri](#trusted-ips) |Federe veya yönetilen bir Kiracı Yöneticiler güvenilen IP'leri şirketin yerel intranetten oturum açtığında, kullanıcı için iki aşamalı doğrulamayı atlamak için kullanabilirsiniz. |
| [Uygulama parolaları](#app-passwords) |Bir uygulama parolası MFA çok faktörlü kimlik doğrulamasını atlamak ve çalışmaya devam etmek için kullanmayan bir uygulama sağlar. |
| [Anımsanan cihazlar ve tarayıcılar için çok faktörlü kimlik doğrulaması unutmayın](#remember-multi-factor-authentication-for-devices-that-users-trust) |MFA kullanarak bir kullanıcı başarıyla oturum sonra gün sayısı kümesini için cihazları anımsamasını sağlar. |
| [Seçilebilir doğrulama yöntemleri](#selectable-verification-methods) |Kullanmak kullanıcılar için kullanılabilir kimlik doğrulama yöntemlerini seçmenize olanak sağlar. |

## <a name="access-the-azure-mfa-management-portal"></a>Azure MFA yönetim portalına erişme

Bu makalede ele alınan özelliklerin Azure multi-Factor Authentication Yönetim Portalı'nda yapılandırılır. MFA Yönetim Portalı üzerinden klasik Azure portalına erişmek için iki yol vardır. İlk multi-Factor Auth sağlayıcısı yöneterek Yapılandır. İkinci MFA hizmet ayarları aracılığıyla Yapılandır. 

### <a name="use-an-authentication-provider"></a>Bir kimlik doğrulama sağlayıcısı kullanın

Tüketim tabanlı MFA için çok faktörlü yetki sağlayıcı kullanırsanız, Yönetim Portalı'na erişmek için bu yöntemi kullanın.

MFA Yönetim Portalı Azure multi-Factor Auth sağlayıcısı üzerinden erişmek için Klasik Azure portalında yönetici olarak oturum açın ve Active Directory seçeneğini seçin. Tıklatın **çok faktörlü kimlik doğrulama sağlayıcıları** sekmesinde, ardından dizininizi seçin ve tıklatın **Yönet** altındaki düğmesini.

### <a name="use-the-mfa-service-settings-page"></a>MFA Hizmeti Ayarları sayfası kullanın 

Aşağıdaki lisanstan varsa, MFA Hizmeti Ayarları sayfası kullanabilirsiniz:
- Azure MFA
- Azure AD Premium 
- Enterprise Mobility + Security

MFA Yönetim Portalı MFA Hizmeti Ayarları sayfası yoluyla erişmek için Klasik Azure portalında yönetici olarak oturum açın ve Active Directory seçeneğini seçin. Dizininize tıklayın ve sonra da **Yapılandır** sekmesine tıklayın. Çok faktörlü kimlik doğrulaması bölümünün altında **Hizmet ayarlarını yönet**'i seçin. MFA Hizmet Ayarları sayfasının en altındaki **Portala git** bağlantısına tıklayın.


## <a name="fraud-alert"></a>Sahtekarlık Uyarısı
Sahtekarlık uyarısı yapılandırılmış ve böylece kullanıcılarınızın kaynaklarına erişmek için sahte denemeleri raporlayabilirsiniz ayarlayın.  Kullanıcıların sahtekarlık mobil uygulama ile ya da telefon üzerinden bildirebilirsiniz.

### <a name="set-up-fraud-alert"></a>Sahtekarlık uyarısı ayarla
1. Bu sayfanın üst kısmındaki yönergeleri başına MFA yönetim portalına gidin.
2. Azure multi-Factor Authentication Yönetim Portalı'nda tıklatın **ayarları** yapılandırma bölümünün altında.
3. Sahtekarlık uyarısı bölümü ayarları sayfasının altında denetleyin **kullanıcıların sahtekarlık uyarısı göndermesine izin ver** onay kutusu.
4. Seçin **kaydetmek** değişikliklerinizi uygulamak için. 

### <a name="configuration-options"></a>Yapılandırma seçenekleri

- **Sahtekarlık bildirildiğinde kullanıcıyı engelle** - bir kullanıcı raporları sahtekarlık hesaplarında engellenir.
- **Kod için rapor sahtekarlık sırasında ilk selamlama** -kullanıcıların normalde iki aşamalı doğrulamayı onaylamak için # tuşuna basın. Sahtekarlığı bildir istiyorsanız, # basmadan önce bir kod girin. Bu kodu **0** varsayılan olarak, ancak özelleştirebilirsiniz.

> [!NOTE]
> Microsoft'un varsayılan sesli karşılamalar kullanıcıların sahtekarlık uyarısı göndermek için &#0; tuşlarına isteyin. 0 dışında bir kod kullanmak istiyorsanız, kaydedin ve kendi özel sesli karşılamalar uygun yönergeleri ile karşıya yükleyin.

![Sahtekarlık uyarısı seçenekleri - ekran görüntüsü](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="how-users-report-fraud"></a>Kullanıcıların sahtekarlık raporu 
Sahtekarlık uyarısı iki yolla bildirilebilir.  Ya da mobil uygulama veya telefon üzerinden.  

#### <a name="report-fraud-with-the-mobile-app"></a>Mobil uygulama ile sahtekarlığı bildir
1. Telefonunuza bir doğrulama gönderildiğinde Microsoft Authenticator uygulamasını açmak için seçin.
2. Seçin **reddetme** bildirim. 
3. Seçin **rapor sahtekarlık**.
4. Uygulamayı kapatın.

#### <a name="report-fraud-with-a-phone"></a>Bir telefon sahtekarlığı bildir
1. Bir doğrulama araması telefonunuza geldiğinde, yanıtlayın.  
2. Sahtekarlık bildirmek için (varsayılan değer 0) sahtekarlık kodu ve # işareti girin. Ardından, bir sahtekarlık uyarısı gönderildi bildirilir.
3. Çağrıyı sonlandırmak.

### <a name="view-fraud-reports"></a>Sahtekarlık raporlarını görüntüle
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Solda, Active Directory'yi seçin.
3. En üstte seçin **çok faktörlü kimlik doğrulama sağlayıcıları** , çok faktörlü kimlik doğrulama sağlayıcıları listesini gösterme.
4. Çok faktörlü yetki sağlayıcı seçin ve tıklatın **Yönet** sayfanın sonundaki. Azure multi-Factor Authentication Yönetim Portalı açar.
5. Azure multi-Factor Authentication Yönetim Portalı, altında bir raporu görüntüle'yi tıklatın **sahtekarlık Uyarısı**.
6. Raporda görüntülemek istediğiniz tarih aralığını belirtin. Ayrıca, kullanıcı adları, telefon numaraları ve kullanıcı durumunu da belirtebilirsiniz.
7. Tıklatın **çalıştırmak** sahtekarlık uyarısı raporunu görüntülemek için. Tıklatın **CSV'ye aktar** raporu dışarı aktarmak istiyorsanız.

## <a name="one-time-bypass"></a>Bir kerelik atlama
Bir kerelik geçiş kullanıcının iki aşamalı doğrulamayı gerçekleştirme olmadan bir seferliğine kimlik doğrulaması sağlar. Atlama geçicidir ve belirtilen sayıda saniye geçtikten sonra süresi dolar. Burada mobil uygulaması ya da telefon bildirim veya telefon görüşmesi almıyor durumlarda, kullanıcının istenen kaynak erişebilmesi için bir kerelik geçiş etkinleştirebilirsiniz.

### <a name="create-a-one-time-bypass"></a>Bir kerelik geçiş oluşturmak
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Bu sayfanın üst kısmındaki yönergeleri başına MFA yönetim portalına gidin.
3. Sol ile Azure MFA sağlayıcınızı adını görüyorsanız, bir  **+**  yanındaki tıklatın  **+** . Farklı MFA sunucusu çoğaltma gruplarına ve Azure varsayılan grup gösterilir. Uygun grubu seçin.
4. Kullanıcı Yönetim'in altında seçin **bir kerelik geçiş**.
5. Bir kerelik geçiş sayfasında, tıklatın **yeni bir kerelik geçiş**.

  ![Bulut](./media/multi-factor-authentication-whats-next/create1.png)

6. Kullanıcı süresi atlama ve atlama nedenini girin. Tıklatın **atlama**.
7. Kullanıcının bir kerelik atlama süresi dolmadan önce oturum açmak gereken şekilde süre yürürlüğe hemen gider. 

### <a name="view-the-one-time-bypass-report"></a>Bir kerelik atlama raporunu görüntüle
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Solda, Active Directory'yi seçin.
3. En üstte seçin **çok faktörlü kimlik doğrulama sağlayıcıları** , çok faktörlü kimlik doğrulama sağlayıcıları listesini gösterme.
4. Çok faktörlü yetki sağlayıcı seçin ve tıklatın **Yönet** sayfanın sonundaki. Azure multi-Factor Authentication Yönetim Portalı açar.
5. Azure multi-Factor Authentication Yönetim Portalı, solda bir raporu görüntüle'nin altında tıklayın **bir kerelik geçiş**.
6. Raporda görüntülemek istediğiniz tarih aralığını belirtin. Ayrıca, kullanıcı adları, telefon numaraları ve kullanıcı durumunu da belirtebilirsiniz.
7. Tıklatın **çalıştırmak** atlamaların raporunu görüntülemek için. Tıklatın **CSV'ye aktar** raporu dışarı aktarmak istiyorsanız.

## <a name="custom-voice-messages"></a>Özel sesli mesajları
Özel sesli mesajları kendi kayıtları veya Tebrikler için iki aşamalı doğrulamayı kullanmanızı sağlar. Ek olarak özel sesli mesajları kullanabilir veya Microsoft değiştirmek için kaydeder.

Başlamadan önce bunlardan dikkat edin:

* Desteklenen dosya biçimleri .wav ve .mp3 olacaktır.
* Dosya boyutu sınırını 5 MB'tır.
* Kimlik doğrulama iletileri 20 saniyeden daha kısa olmalıdır. 20 saniyeden daha uzun iletileri kullanıcıları doğrulama süresi dolmadan önce yanıtlamak için yeterli süre vermeyin.

### <a name="set-up-a-custom-message"></a>Özel ileti ayarlayın

Özel ileti oluşturmanın iki bölümü vardır. İlk olarak, ileti karşıya yükleyin ve ardından, kullanıcılarınız için açın.

Özel ileti karşıya yüklemek için:

1. Desteklenen dosya biçimleri birini kullanarak bir özel sesli ileti oluşturun.
2. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
3. Bu sayfanın üst kısmındaki yönergeleri başına MFA yönetim portalına gidin.
4. Azure multi-Factor Authentication Yönetim Portalı'nda tıklatın **sesli mesajlar** yapılandırma bölümünün altında.
5. Yapılandırma üzerinde: iletileri sayfa sesli, tıklatın **yeni bir sesli mesajı**.
   ![Bulut](./media/multi-factor-authentication-whats-next/custom1.png)
6. Yapılandırma üzerinde: yeni ses iletileri sayfasında, tıklatın **ses dosyalarını yönetme**.
   ![Bulut](./media/multi-factor-authentication-whats-next/custom2.png)
7. Yapılandırma üzerinde: ses dosyalarını sayfasında, tıklatın **ses dosyasını karşıya yükle**.
   ![Bulut](./media/multi-factor-authentication-whats-next/custom3.png)
8. Yapılandırma üzerinde: ses dosyasını karşıya yükle'ye tıklayın **Gözat** ve ses iletinizi gidin, tıklatın **açık**.
9. Bir açıklama ekleyin ve tıklatın **karşıya**.
10. Sesli iletiyi yüklendikten sonra bir ileti dosyası başarıyla karşıya yüklediğiniz onaylar.

Kullanıcılarınız için ileti etkinleştirmek için:

1. Sol bölmede, tıklatın **sesli mesajlar**.
2. Sesli mesaj bölümü altında tıklatın **yeni bir sesli mesajı**.
3. Dil açılan listeden, bir dil seçin.
4. Belirli bir uygulama için bu iletiyi uygulama kutusuna belirtin.
5. İleti türü açılır, yeni özel iletisiyle geçersiz kılınacak ileti türünü seçin.
6. Ses dosyası açılan listeden, ilk kısımda karşıya yüklediğiniz ses dosyası seçin.
7. **Oluştur**'a tıklayın. Bir ileti, sesli iletiyi başarıyla oluşturdunuz onaylar.
    ![Bulut](./media/multi-factor-authentication-whats-next/custom5.png)</center>

## <a name="caching-in-azure-multi-factor-authentication"></a>Azure çok faktörlü kimlik doğrulaması önbelleğe alma
Önbelleğe alma, bu süre içinde sonraki kimlik doğrulama girişimleri otomatik olarak başarılı belirli bir zaman aralığı belirlemenizi sağlar. İlk istek hala devam ederken VPN gibi şirket içi sistemlerde fazla doğrulama isteği gönderdiğinizde, önbelleğe alma kullanılır. Kullanıcı ilk doğrulama devam ediyor başarılı olduktan sonra otomatik olarak başarılı olması sonraki istekleri izin verme. 

Önbelleğe alma, Azure ad oturum açma işlemleri için kullanılmak üzere tasarlanmamıştır.

### <a name="set-up-caching"></a>Önbelleğe almayı kurmak 
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Bu sayfanın üst kısmındaki yönergeleri başına MFA yönetim portalına gidin.
3. Azure multi-Factor Authentication Yönetim Portalı'nda tıklatın **önbelleğe alma** yapılandırma bölümünün altında.
4. Tıklatın **yeni önbellek** sayfa önbelleğe almayı yapılandır üzerinde.
5. Önbellek türü ve saniye cinsinden önbellek süresi seçin. **Oluştur**'a tıklayın.

<center>![Bulut](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Güvenilen IP'ler
Güvenilen IP'leri federe veya yönetilen bir Kiracı Yöneticiler iki aşamalı doğrulamayı atlamak için kullanabileceğiniz bir Azure mfa bir özelliktir. Hangi şirket yerel intranetten imzalama kullanıcılar için kullanılır. Bu özellik, Azure multi-Factor Authentication, ücretsiz sürümünü Yöneticiler için değil tam sürümü ile kullanılabilir. Azure çok faktörlü kimlik doğrulaması'nın tam sürümünü alma hakkında daha fazla bilgi için bkz: [Azure çok faktörlü kimlik doğrulaması](multi-factor-authentication.md).

| Azure AD Kiracı türü | Kullanılabilir güvenilen IP Seçenekleri |
|:--- |:--- |
| Yönetilen |<li>Belirli IP adresi aralıkları – yöneticileri şirketin intranetten imzalama kullanıcılar için iki aşamalı doğrulamayı atlayan bir IP adresi aralığı belirtebilirsiniz.</li> |
| Federasyon |<li>Tüm federe kullanıcılar - gelen kuruluş içinde oturum açan tüm Federasyon kullanıcıları AD FS tarafından verilen bir talep kullanan iki aşamalı doğrulamayı atlayacaktır.</li><br><li>Belirli IP adresi aralıkları – yöneticileri şirketin intranetten imzalama kullanıcılar için iki aşamalı doğrulamayı atlayan bir IP adresi aralığı belirtebilirsiniz. |

Bu yalnızca gelen works bir şirket intranetindeki atlama. 

**Son kullanıcı deneyimi corpnet içinde:**

Güvenilen IP'ler devre dışı bırakıldığında, iki aşamalı doğrulamayı tarayıcı akışları için gereklidir ve uygulama parolaları eski zengin istemci uygulamaları için gereklidir. 

Güvenilen IP'ler etkinleştirildiğinde, iki aşamalı doğrulamayı olan *değil* tarayıcı akışları için gereklidir. Uygulama parolaları *değil* kullanıcı zaten bir uygulama parolası oluşturulmuş kurmadı koşuluyla eski zengin istemci uygulamaları için gereklidir. Bir uygulama parolası kullanımda olduğunda, gerekli kalır. 

**Son kullanıcı deneyimi dış corpnet:**

Güvenilen IP'ler veya etkinleştirilip etkinleştirilmeyeceğini iki aşamalı doğrulamayı tarayıcı akışları için gereklidir ve uygulama parolaları eski zengin istemci uygulamaları için gereklidir. 

### <a name="to-enable-trusted-ips"></a>Güvenilen IP'ler etkinleştirmek için
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Bu makalenin başlangıcında yönergeleri başına MFA hizmeti ayarları sayfasına gidin.
3. Güvenilen IP'ler altında hizmet ayarları sayfasında iki seçeneğiniz vardır:
   
   * **My intranetten gelen Federasyon kullanıcılarının istekleri için** – onay kutusunu işaretleyin. AD FS tarafından verilen bir talep kullanarak şirket ağına atlama iki aşamalı doğrulamayı gelen oturum açan tüm Federasyon kullanıcıları.
   * **Belirli bir ortak IP aralığını gelen istekleri için** – IP adreslerini sağlanan CIDR gösterimini kullanarak metin kutusuna girin. Örneğin: xxx.xxx.xxx.0/24 IP adresleri aralığı xxx.xxx.xxx.1 – xxx.xxx.xxx.254 veya xxx.xxx.xxx.xxx/32 tek bir IP adresi. En fazla 50 IP adres aralıklarını girebilirsiniz. Bu IP adreslerinden oturum açan kullanıcıların iki aşamalı Doğrulamayı atla.
4. **Kaydet** düğmesine tıklayın.
5. Güncelleştirmeler uygulandıktan sonra tıklayın **Kapat**.

![Güvenilen IP'ler](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Uygulama parolaları
Bazı uygulamalar, Office 2010 gibi eski veya ve Apple Mail iki aşamalı doğrulamayı desteklemez. İkinci doğrulama kabul edecek şekilde yapılandırılmamışlardır. Bu uygulamaları kullanmak için "uygulama parolaları" kullanmak geleneksel parolanız yerine gerekir. Uygulama parolası iki aşamalı Doğrulamayı atla ve çalışmaya devam etmek uygulama izin verir.

> [!NOTE]
> Office 2013 istemcilerin için modern kimlik doğrulaması
> 
> Office 2013 istemciler (Outlook gibi) ve yeni destek modern kimlik doğrulama protokolleri ve iki aşamalı doğrulamayla birlikte çalışmak üzere etkinleştirilebilir. Etkinleştirildikten sonra uygulama parolaları bu istemciler için gerekli değildir.  Daha fazla bilgi için bkz: [Office 2013 modern kimlik doğrulaması genel önizlemesi Duyuruldu](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-to-know-about-app-passwords"></a>Uygulama parolaları hakkında bilinmesi gereken önemli şeyler
Uygulama parolaları hakkında bilinmesi önemli şeyler listesi aşağıda verilmiştir.

* Her uygulama için bir kez giriş kutusuna uygulama parolaları yeniden girilmesi gerekir. Kullanıcıların bunları izlemek ve bunları her zaman girmeniz gerekmez.
* Gerçek parola otomatik olarak oluşturulur ve kullanıcı tarafından sağlanmaz. Otomatik olarak oluşturulan parolanın bir saldırgan tahmin edilmesi daha zor ve daha güvenlidir.
* Kullanıcı başına 40 parolalık bir sınırı yoktur. 
* Parolaları önbelleğe ve şirket içi senaryolarda kullanan uygulamalar, uygulama parolası Kurumsal kimlik dışında bilinen değil bu yana başarısız olan başlayabilir. Şirket içi Exchange e-postaları örneğidir ancak arşivlenen postanın bulutta olduğu. Aynı parola çalışmaz.
* Çok faktörlü kimlik doğrulaması başlatıldığında, bazı tarayıcı olmayan istemciler ile parolanızı kullanabilirsiniz. Yönetim görevleri için uygulama parolaları kullanamazsınız. Bir hizmet hesabı PowerShell komut dosyalarını çalıştırmak için güçlü bir parola oluşturun ve o hesap için iki aşamalı doğrulamayı etkinleştirmeyin emin olun.

> [!WARNING]
> Uygulama parolaları burada istemcileri hem şirket içi ile iletişim kurmak ve Otomatik bulma uç noktaları bulut Karma ortamlarda çalışmıyor. Etki alanı parolalarının şirket içi kimlik doğrulaması için gereklidir ve uygulama parolaları ile bulut kimlik doğrulaması için gereklidir.

### <a name="naming-guidance-for-app-passwords"></a>Uygulama parolaları için adlandırma Kılavuzu
Uygulama parolası adlarının üzerinde kullanıldıkları aygıt yansıtmalıdır. Örneğin, tarayıcı olmayan uygulamalara sahip bir dizüstü bilgisayarınız varsa, dizüstü adlı bir uygulama parolası oluşturmanız ve bu uygulama parolasını kullanın. Ardından, masaüstü bilgisayarınızda aynı uygulamaları için Masaüstü adlı başka bir uygulama parolası oluşturun. 

Microsoft, cihaz, uygulama başına değil bir uygulama parolası başına bir uygulama parolası oluşturma önerir.

### <a name="federated-sso-app-passwords"></a>Federasyon (SSO) uygulama parolaları
Azure AD şirket içi Windows Server Active Directory etki alanı Hizmetleri (AD DS) ile Federasyon (çoklu oturum açma) destekler. Kuruluşunuz Azure AD ile birleştirildiyse ve Azure multi-Factor Authentication kullanarak olacak, uygulama parolaları hakkında bilgi sizin için önemli olan. Bu bölüm, yalnızca federasyon (SSO) müşterileri için geçerlidir.

* Uygulama parolaları Azure AD tarafından doğrulanır ve bu nedenle Federasyon atlayabilir. Federasyon yalnızca etkin bir şekilde uygulama parolaları ayarlarken kullanılır.
* Federasyon (SSO) kullanıcılar için biz hiçbir zaman pasif akış aksine kimlik sağlayıcıyı (IDP) gidin. Parolalar kuruluş kimliği depolanır. Kullanıcının şirketten ayrılması durumunda, bu bilgileri gerçek zamanında DirSync kullanılarak kurumsal kimliğe akmasını vardır. Hesap devre dışı bırakma/silme, Azure AD'de uygulama parolasını devre dışı bırakma/silme geciktirme eşitlemek için üç saat sürebilir.
* Şirket için İstemci Erişimi Denetimi ayarları Uygulama Parolası tarafından onaylanmaz.
* Günlüğe kaydetme/denetleme yeteneği şirket içi kimlik doğrulaması uygulama parolası için kullanılabilir değildir.
* Bazı gelişmiş mimari tasarımları kuruluş kullanıcı adı, parola ve uygulama parolaları bileşimini iki aşamalı doğrulamayı istemcileriyle kullanırken gerektirebilir. Ancak burada kimlik doğrulamasında üzerinde bağlıdır. Bir şirket içi altyapı karşı kimlik doğrulaması istemcileri için bir kuruluş kullanıcı adı ve parola kullanırsınız. Azure AD karşı kimlik doğrulaması istemcileri için uygulama parolası kullanırsınız.

  Örneğin, bu örnekleri oluşan bir mimari olduğunu varsayalım:

  * Şirket içi örneğinizi Azure AD ile Active Directory federasyonunu
  * Exchange online kullanıyorsanız
  * Özel olarak şirket içi Lync kullanma
  * Azure multi-Factor Authentication kullanma

  ![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

  Bu durumlarda, şu adımları izlemelisiniz:

  * İmzalama için Lync bileşenini, kuruluşların kullanıcı adı ve parola kullanın.
  * Exchange online bağlayan bir Outlook istemcisi adres defteri erişmeye çalışırken bir uygulama parolasını kullanın.

### <a name="allow-app-password-creation"></a>Uygulama parolası oluşturmaya izin ver
Varsayılan olarak, kullanıcılar uygulama parolaları oluşturamaz, ancak kendiniz özelliğini etkinleştirebilirsiniz. Kullanıcıların uygulama parolaları oluşturma olanağı izin vermek için aşağıdaki yordamı kullanın:

1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Bu makalenin başlangıcında yönergeleri başına MFA hizmeti ayarları sayfasına gidin.
3. Radyo düğmesini işaretleyin **kullanıcıların tarayıcı olmayan uygulamalarda oturum açmak için uygulama parolaları oluşturmasına izin**.

![Uygulama parolaları oluşturma](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Uygulama parolaları oluşturma
Kullanıcıların uygulama parolaları, ilk kaydı sırasında oluşturabilirsiniz. Bunlar uygulama parolaları oluşturma izni veren kayıt işleminin sonunda seçeneği sunulur.

Kullanıcıların uygulama parolaları kayıttan sonra oluşturabilirsiniz. Azure portalında veya Office 365 portalı ayarlarını değiştirerek uygulama parolaları oluşturabilirsiniz. Daha fazla bilgi ve kullanıcılarınız için ayrıntılı adımlar için bkz: [Azure çok faktörlü kimlik doğrulaması'ndaki uygulama parolaları nedir](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Kullanıcıların güven cihazlar için çok faktörlü kimlik doğrulaması unutmayın
Cihazlar ve kullanıcılar güven tüm MFA kullanıcılar için boş bir özelliktir; tarayıcılar için çok faktörlü kimlik doğrulaması anımsama. Çok faktörlü kimlik doğrulaması atlama MFA kullanıcılara oturum açtıktan sonra gün sayısı kümesini sağlar. Bu özellik, bir kullanıcı iki aşamalı doğrulama aynı cihaza gerçekleştirebilir sayısını en aza indirerek kullanılabilirlik iyileştirir.

Bir hesap veya aygıt aşılırsa, ancak, güvenilen cihazlar için MFA hatırlama güvenliği etkileyebilir. Bir kurumsal hesap güvenliği tehlikeye girdiğinde veya güvenilir bir cihaz kaybolur veya çalınırsa, gerek [tüm cihazlarda çok faktörlü kimlik doğrulama geri](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Bu işlem tüm cihazlar güvenilen durumundan iptal eder ve kullanıcı yeniden iki aşamalı doğrulamayı gerçekleştirmek için gereklidir. MFA'ndaki yönergeleri ile kendi cihazlarda geri yüklemek için kullanıcılar ayrıca söyleyebilirsiniz [iki aşamalı doğrulama için ayarlarınızı yönetme](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Nasıl çalışır?

Çok faktörlü kimlik doğrulaması çalışır bir kullanıcı ettiğinde tarayıcıda kalıcı bir tanımlama bilgisi ayarlayarak hatırlamak "için sorma **X** gün" oturum açma sırasında kutusu. Tanımlama bilgisinin süresinin kadar kullanıcı MFA için yeniden bu tarayıcıdan istenmez. Kullanıcı aynı aygıtta farklı bir tarayıcı açar veya kendi tanımlama bilgilerini temizler, tekrar sorulur. 

"İçin sorma **X** gün" modern kimlik doğrulama desteği olup olmadığına bakılmaksızın, onay kutusu tarayıcı olmayan uygulamaları üzerinde gösterilen değil. Bu uygulamalar her saat yeni erişim belirteçleri sağlamak yenileme belirteçleri kullanın. Bir yenileme belirteci doğrulandığında, Azure AD son zaman iki aşamalı doğrulamayı yapılandırılan gün sayısının içinde gerçekleştirilen denetler. 

Conclusion, güvenilen cihazlara MFA hatırlama kimlik doğrulamaları (hangi normalde her zaman sor) web uygulamalarını sayısını azaltır. Ancak aynı zamanda kimlik doğrulama (hangi normalde her 90 günde isteyin) modern kimlik doğrulama istemcilerinin sayısını artırır.

> [!NOTE]
>Kullanıcıların AD FS ile Azure MFA sunucusu veya bir üçüncü taraf MFA çözümü için iki aşamalı doğrulamayı gerçekleştirdiğinizde bu özellik, AD FS'nin "Oturumumu açık bırak" özelliğiyle uyumlu değil. Kullanıcılarınızın "Oturumumu açık bırak" AD FS seçerseniz ve ayrıca MFA için cihazlarını güvenilir olarak işaretlemek, "MFA unutmayın" kaç gün süresi dolduktan sonra doğrulayamazsınız olmayacaktır. Azure AD yeni iki aşamalı doğrulama isteklerini, ancak AD FS yeniden iki aşamalı doğrulamayı gerçekleştirmek yerine tarih ve özgün MFA talep belirteciyle döndürür. Bu işlem Azure arasında doğrulama döngüsü kapalı ayarlar AD ve AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Anımsa çok faktörlü kimlik doğrulamasını etkinleştir
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Bu makalenin başlangıcında yönergeleri başına MFA hizmeti ayarları sayfasına gidin.
3. Hizmet Ayarları sayfasında, kullanıcı cihaz ayarları altında yönetmek denetleyin **kullanıcıların güvendikleri cihazlarda çok faktörlü kimlik doğrulaması unutmayın izin ver** kutusu.
   ![Aygıtları unutmayın](./media/multi-factor-authentication-whats-next/remember.png)
4. İki aşamalı doğrulamayı atlamak güvenilen cihazları izin vermek istediğiniz gün sayısını ayarlayın. Varsayılan değer 14 gündür.
5. **Kaydet** düğmesine tıklayın.
6. **Kapat**’a tıklayın.

### <a name="mark-a-device-as-trusted"></a>Bir aygıt güvenilen olarak işaretle

Bu özelliği etkinleştirdiğinizde, kullanıcılar bir aygıtın ne zaman güvenilir olarak işaretleyebilir denetleyerek oturum **bir daha sorma**.

![-Ekran görüntüsü sorma](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Seçilebilir doğrulama yöntemleri
Hangi doğrulama yöntemleri, kullanıcılarınız için kullanılabilir seçebilirsiniz. Aşağıdaki tabloda her bir yöntemin kısa bir genel bakış sağlar.

Kullanıcılarınız için MFA hesaplarına kaydettiğinizde, bunlar, etkin seçenekleri dışında tercih edilen doğrulama yöntemi seçin. İçinde kayıt işlemi için kılavuz ele [Hesabımı iki aşamalı doğrulama için ayarlama](multi-factor-authentication-end-user-first-time.md)

| Yöntem | Açıklama |
|:--- |:--- |
| Telefonu arama |Otomatik bir sesli aramayla yerleştirir. Kullanıcı telefonu yanıtladığında ve # kimliğini doğrulamak için telefon basar. Bu telefon numarası şirket içi Active Directory ile eşitlenmemiş. |
| Telefona kısa mesaj |Doğrulama kodunu içeren bir kısa mesaj gönderir. Kullanıcı yanıt vermesi metin iletisi doğrulama kodunu veya oturum açma arabirimine doğrulama kodunu girmeniz istenir. |
| Mobil uygulama üzerinden bildirim |Telefonunuza veya kayıtlı cihazınıza anında iletme bildirimi gönderir. Kullanıcı bildirimi görünümleri ve seçer **doğrula** doğrulamayı tamamlamak için. <br>Microsoft Authenticator uygulaması [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), ve [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Mobil uygulamadan alınan doğrulama kodu |Microsoft Authenticator uygulamasını her 30 saniyede yeni bir OATH doğrulama kodu oluşturur. Kullanıcı oturum açma arabirimine bu doğrulama kodu girer.<br>Microsoft Authenticator uygulaması [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), ve [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-to-enabledisable-authentication-methods"></a>Nasıl kimlik doğrulama yöntemleri etkinleştir/devre dışı bırak
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Bu makalenin başlangıcında yönergeleri başına MFA hizmeti ayarları sayfasına gidin.
3. Hizmet Ayarları sayfasında, doğrulama seçeneklerini seçin/kullanmak istediğiniz seçenekleri seçimini kaldırın.
   ![Doğrulama seçenekleri](./media/multi-factor-authentication-whats-next/authmethods.png)
4. **Kaydet** düğmesine tıklayın.
5. **Kapat**’a tıklayın.
