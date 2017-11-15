---
title: "Azure MFA yapılandırma | Microsoft Docs"
description: "Bu yapılacaklar sonraki MFA ile açıklayan Azure multi-Factor authentication sayfasıdır.  Bu raporlar, sahtekarlık Uyarısı, bir kerelik atlama, özel sesli mesajları, önbelleğe alma, güvenilen IP'leri ve uygulama parolaları içerir."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2017
ms.author: joflore
ms.reviewer: alexwe
ms.openlocfilehash: 5da47bf2f48b0f5df5f7fa19f1f626fbdca2b8db
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="configure-azure-multi-factor-authentication-settings---public-preview"></a>Azure çok faktörlü kimlik doğrulama ayarlarını - genel Önizleme yapılandırın

Bu makalede Azure çok faktörlü kimlik doğrulaması ve çalışıyor olduğundan göre yönetmenize yardımcı olur.  En Azure multi-Factor Authentication dışında elde size yardımcı çeşitli konuları kapsar.  Tüm bu özellikler kullanılabilir olan her [Azure multi-Factor Authentication sürümü](/multi-factor-authentication-get-started.md#what-features-do-i-need).

>[!NOTE]
>Azure portalında genel önizlemede bu ayarlardır. Pfweb portalındaki Azure çok faktörlü kimlik doğrulama ayarları yönetme hakkında daha fazla belgeler için bkz: [Azure çok faktörlü kimlik doğrulaması yapılandırma ayarlarını](multi-factor-authentication-whats-next-pfweb.md).

| Özellik | Açıklama | 
|:--- |:--- |
| [Engelleme ve kullanıcıların Engellemeyi Kaldır](#block-and-unblock) |Kullanıcı engelle/Engellemeyi Kaldır kullanıcılar kimlik doğrulama isteklerini almasını engelleyebilirsiniz. |
| [Sahtekarlık Uyarısı](#fraud-alert) |Sahtekarlık uyarısı yapılandırılmış ve böylece kullanıcılarınızın kaynaklarına erişmek için sahte denemeleri raporlayabilirsiniz ayarlayın. |
| [Bir kerelik atlama](#one-time-bypass) |Bir kerelik geçiş "çok faktörlü kimlik doğrulamasını atlayarak" bir kereliğine kimlik doğrulaması sağlar. |
| [Özel sesli mesajları](#custom-voice-messages) |Özel sesli mesajları kendi kayıtları veya Tebrikler çok faktörlü kimlik doğrulama kullanmanıza olanak sağlar. |
| [Önbelleğe alma](#caching-in-azure-multi-factor-authentication) |Önbelleğe alma, sonraki kimlik doğrulama girişimleri otomatik olarak başarılı belirli bir zaman aralığı belirlemenizi sağlar. |
| [Güvenilen IP'leri](#trusted-ips) |Federe veya yönetilen bir Kiracı Yöneticiler güvenilen IP'leri şirketin yerel intranetten oturum açtığında, kullanıcı için iki aşamalı doğrulamayı atlamak için kullanabilirsiniz. |
| [Uygulama parolaları](#app-passwords) |Bir uygulama parolası MFA çok faktörlü kimlik doğrulamasını atlamak ve çalışmaya devam etmek için kullanmayan bir uygulama sağlar. |
| [Anımsanan cihazlar ve tarayıcılar için çok faktörlü kimlik doğrulaması unutmayın](#remember-multi-factor-authentication-for-devices-that-users-trust) |MFA kullanarak bir kullanıcı başarıyla oturum sonra gün sayısı kümesini için cihazları anımsamasını sağlar. |
| [Seçilebilir doğrulama yöntemleri](#selectable-verification-methods) |Kullanmak kullanıcılar için kullanılabilir kimlik doğrulama yöntemlerini seçmenize olanak sağlar. |

## <a name="block-and-unblock"></a>Engelleme ve engellemesini kaldırma
Kullanıcı engelle/Engellemeyi Kaldır, kullanıcıların kimlik doğrulama isteklerini almasını önlemek için kullanılabilir. Engellenen kullanıcılar için kimlik doğrulama girişimleri otomatik olarak reddedilir. Süre 90 gün bunlar engellenir engellenen kullanıcılara engellenen kalır.

### <a name="block-a-user"></a>Bir kullanıcı engelleme
1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gidin **Azure Active Directory** > **MFA sunucusu** > **engelle/Engellemeyi Kaldır kullanıcılar**.
3. Tıklatın **Ekle** bir kullanıcıyı engellemek için.
4. Seçin **çoğaltma grubu**, engellenen kullanıcı adı olarak giriş  **username@domain.com** ve bir açıklama girin **neden** alan.
5. Tıklatın **Ekle** engelleyen kullanıcı tamamlamak için.

### <a name="unblock-a-user"></a>Bir kullanıcının engelini kaldırma
1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gidin **Azure Active Directory** > **MFA sunucusu** > **engelle/Engellemeyi Kaldır kullanıcılar**.
3. Tıklatın **Engellemeyi Kaldır** içinde **eylem** engellemesini kaldırmak istediğiniz kullanıcı yanındaki sütuna.
4. Bir yorum girin **neden engellemelerini kaldırma için** alan.
5. Tıklatın **Engellemeyi Kaldır** kullanıcının engellemesini kaldırma tamamlamak için.

## <a name="fraud-alert"></a>Sahtekarlık Uyarısı
Sahtekarlık uyarısı yapılandırılmış ve böylece kullanıcılarınızın kaynaklarına erişmek için sahte denemeleri raporlayabilirsiniz ayarlayın.  Kullanıcıların sahtekarlık mobil uygulama ile ya da telefon üzerinden bildirebilirsiniz.

### <a name="turn-on-fraud-alert"></a>Sahtekarlık uyarısı Aç
1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gidin **Azure Active Directory** > **MFA sunucusu** > **sahtekarlık Uyarısı**.

   ![Sahtekarlık uyarısı](./media/multi-factor-authentication-whats-next/fraudalert.png)

3. Kapatma **kullanıcıların sahtekarlık uyarısı göndermesine izin ver** için **üzerinde**.
4. **Kaydet**’i seçin.

### <a name="configuration-options"></a>Yapılandırma seçenekleri

- **Sahtekarlık bildirildiğinde kullanıcıyı engelle** - 90 gün boyunca bir kullanıcı raporları sahtekarlık hesaplarında engelleniyorsa veya yönetici hesaplarına engelini kaldırır kadar. Bir yönetici oturum açma işlemleri kullanarak oturum açma rapor gözden geçirin ve gelecekteki sahtekarlığı önlemek için uygun eylemi gerçekleştirin. Yönetici böylece [engellemesini](#unblock-a-user) kullanıcının hesabı.
- **İlk Karşılama sırasında sahtekarlık bildirme kodu** - iki aşamalı doğrulamayı gerçekleştirmek için bir telefon araması aldığında, kullanıcıların Bunlar normalde # tuşuna basın, oturum açma onaylamak için. Sahtekarlığı bildir istiyorsanız, # basmadan önce bir kod girin. Bu kodu **0** varsayılan olarak, ancak özelleştirebilirsiniz.

> [!NOTE]
> Microsoft'un varsayılan sesli karşılamalar kullanıcıların sahtekarlık uyarısı göndermek için &#0; tuşlarına isteyin. 0 dışında bir kod kullanmak istiyorsanız, kaydedin ve kendi özel sesli karşılamalar uygun yönergeleri ile karşıya yükleyin.

### <a name="view-fraud-reports"></a>Sahtekarlık raporlarını görüntüle
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Yönetmek istediğiniz dizini seçin. 
4. Seçin **yapılandırın**
5. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarlarını Yönet**.
6. Hizmet ayarları sayfasının en altında seçin **Portal'a Git**.
7. Azure multi-Factor Authentication Yönetim Portalı, altında bir raporu görüntüle'yi tıklatın **sahtekarlık Uyarısı**.
8. Raporda görüntülemek istediğiniz tarih aralığını belirtin. Ayrıca, kullanıcı adları, telefon numaraları ve kullanıcı durumunu da belirtebilirsiniz.
9. **Çalıştır**’a tıklayın. Bu bir sahtekarlık uyarısı Raporu ayarlarken getirir. Tıklatın **CSV'ye aktar** raporu dışarı aktarmak istiyorsanız.

## <a name="one-time-bypass"></a>Bir kerelik atlama
Bir kerelik geçiş kullanıcının iki aşamalı doğrulamayı gerçekleştirme olmadan bir seferliğine kimlik doğrulaması sağlar. Atlama geçicidir ve belirtilen sayıda saniye geçtikten sonra süresi dolar. Burada mobil uygulaması ya da telefon bildirim veya telefon görüşmesi almıyor durumlarda, kullanıcının istenen kaynak erişebilmesi için bir kerelik geçiş etkinleştirebilirsiniz.

### <a name="create-a-one-time-bypass"></a>Bir kerelik geçiş oluşturmak

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gidin **Azure Active Directory** > **MFA sunucusu** > **bir kerelik atlama**.

   ![Bir kerelik atlama](./media/multi-factor-authentication-whats-next/onetimebypass.png)
3. **Add (Ekle)** seçeneğini belirleyin.
4. Gerekirse, bu geçiş için çoğaltma grubunu seçin.
5. Kullanıcı adı girin (biçiminde username@domain.com), atlama bulunacağı saniye ve atlama nedeni sayısı. 
6. **Add (Ekle)** seçeneğini belirleyin. Kullanıcının bir kerelik atlama süresi dolmadan önce oturum açmak gereken şekilde süre yürürlüğe hemen gider. 

### <a name="view-the-one-time-bypass-report"></a>Bir kerelik atlama raporunu görüntüle
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Yönetmek istediğiniz dizini seçin. 
4. Seçin **yapılandırın**
5. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarlarını Yönet**.
6. Hizmet ayarları sayfasının en altında seçin **Portal'a Git**.
7. Azure multi-Factor Authentication Yönetim Portalı, altında bir raporu görüntüle'yi tıklatın **bir kerelik geçiş**.
8. Raporda görüntülemek istediğiniz tarih aralığını belirtin. Ayrıca, kullanıcı adları, telefon numaraları ve kullanıcı durumunu da belirtebilirsiniz.
9. **Çalıştır**’a tıklayın. Bu bir atlamaların Raporu ayarlarken getirir. Tıklatın **CSV'ye aktar** raporu dışarı aktarmak istiyorsanız.

## <a name="custom-voice-messages"></a>Özel sesli mesajları
Özel sesli mesajları kendi kayıtları veya Tebrikler için iki aşamalı doğrulamayı kullanmanızı sağlar. Bunlara ek olarak kullanılabilir veya Microsoft değiştirmek için kaydeder.

Başlamadan önce aşağıdakilere dikkat edin:

* Desteklenen dosya biçimleri .wav ve .mp3 olacaktır.
* Dosya boyutu sınırını 5 MB'tır.
* Kimlik doğrulama iletileri 20 saniyeden daha kısa olmalıdır. Bu değerden daha uzun bir şey doğrulama ileti tamamlanmadan önce kullanıcı doğrulama zaman aşımına neden yanıt vermeyebilir başarısız olmasına neden olabilir.

### <a name="set-up-a-custom-message"></a>Özel ileti ayarlayın

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gidin **Azure Active Directory** > **MFA sunucusu** > **telefon araması ayarları**.

   ![Telefon araması ayarları](./media/multi-factor-authentication-whats-next/phonecallsettings.png)

3. Seçin **Ekle Tebrik**.
4. Karşılama ve dil türünü seçin.
5. Karşıya yüklemek için bir .mp3 veya .wav ses dosyası seçin.
6. **Add (Ekle)** seçeneğini belirleyin.

## <a name="caching-in-azure-multi-factor-authentication"></a>Azure çok faktörlü kimlik doğrulaması önbelleğe alma
Önbelleğe alma, bu süre içinde sonraki kimlik doğrulama girişimleri otomatik olarak başarılı belirli bir zaman aralığı belirlemenizi sağlar. İlk istek hala devam ederken VPN gibi şirket içi sistemlerini birden çok doğrulama isteği gönderdiğinizde, bu öncelikle kullanılır. Bu, kullanıcı ilk doğrulama devam ediyor başarılı olduktan sonra otomatik olarak başarılı olması sonraki isteklere izin verir. 

Önbelleğe alma, Azure ad oturum açma işlemleri için kullanılmak üzere tasarlanmamıştır.

### <a name="set-up-caching"></a>Önbelleğe almayı kurmak 
1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gidin **Azure Active Directory** > **MFA sunucusu** > **kuralları önbelleğe alma**.

   ![Önbelleğe alma kuralları](./media/multi-factor-authentication-whats-next/cachingrules.png)

4. **Add (Ekle)** seçeneğini belirleyin.
5. Önbellek türü açılır listeden seçin ve en fazla önbellek saniye sayısını belirtin. 
6. Gerekirse, bir kimlik doğrulama türü seçin ve bir uygulama belirtin. 
7. **Add (Ekle)** seçeneğini belirleyin.


## <a name="trusted-ips"></a>Güvenilen IP'ler
Güvenilen IP'leri federe veya yönetilen bir kiracı yöneticileri şirketin yerel intranetten imzalama kullanıcılar için iki aşamalı doğrulamayı atlamak için kullanabileceğiniz bir Azure MFA özelliğidir. Bu özellik, Azure multi-Factor Authentication, ücretsiz sürümünü Yöneticiler için değil tam sürümü ile kullanılabilir. Azure çok faktörlü kimlik doğrulaması'nın tam sürümünü alma hakkında daha fazla bilgi için bkz: [Azure çok faktörlü kimlik doğrulaması](multi-factor-authentication.md).

| Azure AD Kiracı türü | Kullanılabilir güvenilen IP Seçenekleri |
|:--- |:--- |
| Yönetilen |<li>Belirli IP adresi aralıkları – yöneticileri şirketin intranetten imzalama kullanıcılar için iki aşamalı doğrulamayı atlayan bir IP adresi aralığı belirtebilirsiniz.</li> |
| Federasyon |<li>Tüm federe kullanıcılar - gelen kuruluş içinde oturum açan tüm Federasyon kullanıcıları AD FS tarafından verilen bir talep kullanan iki aşamalı doğrulamayı atlayacaktır.</li><br><li>Belirli IP adresi aralıkları – yöneticileri şirketin intranetten imzalama kullanıcılar için iki aşamalı doğrulamayı atlayan bir IP adresi aralığı belirtebilirsiniz. |

Bu yalnızca gelen works bir şirket intranetindeki atlama. Örneğin, tüm Federasyon kullanıcıları seçili ve şirketin intranet dışından gelen bir kullanıcı açtığında, kullanıcının AD FS talep sunsa bile iki aşamalı doğrulama kullanarak kimlik doğrulaması kullanıcının sahip. 

**Son kullanıcı deneyimi corpnet içinde:**

Güvenilen IP'ler devre dışı bırakıldığında, iki aşamalı doğrulamayı tarayıcı akışları için gereklidir ve uygulama parolaları eski zengin istemci uygulamaları için gereklidir. 

Güvenilen IP'ler etkinleştirildiğinde, iki aşamalı doğrulamayı olan *değil* tarayıcı akışlar için gerekli ve uygulama parolaları *değil* eski zengin istemci uygulamaları için kullanıcı bir uygulama zaten oluşturulmuş kurmadı koşuluyla gerekli parola. Bir uygulama parolası kullanımda olduğunda, gerekli kalır. 

**Son kullanıcı deneyimi dış corpnet:**

Güvenilen IP'ler veya etkinleştirilip etkinleştirilmeyeceğini iki aşamalı doğrulamayı tarayıcı akışları için gereklidir ve uygulama parolaları eski zengin istemci uygulamaları için gereklidir. 

### <a name="to-enable-trusted-ips"></a>Güvenilen IP'ler etkinleştirmek için
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Yönetmek istediğiniz dizini seçin. 
4. Seçin **yapılandırın**
5. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarlarını Yönet**.
6. Güvenilen IP'ler altında hizmet ayarları sayfasında iki seçeneğiniz vardır:
   
   * **My intranetten gelen Federasyon kullanıcılarının istekleri için** – onay kutusunu işaretleyin. Kurumsal ağ üzerinden oturum açan tüm Federasyon kullanıcıları, AD FS tarafından verilen bir talep kullanan iki aşamalı doğrulamayı atlayacaktır. AD FS uygun trafiği intranet talep eklemek için bir kuralı bulunduğundan emin olun. Zaten yoksa, aşağıdaki kural AD FS'de oluşturmanız gerekir: "c: [türü"http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"==] = > issue(claim = c);"



   * **Belirli bir ortak IP aralığını gelen istekleri için** – IP adreslerini sağlanan CIDR gösterimini kullanarak metin kutusuna girin. Örneğin: xxx.xxx.xxx.0/24 IP adresleri aralığı xxx.xxx.xxx.1 – xxx.xxx.xxx.254 veya xxx.xxx.xxx.xxx/32 tek bir IP adresi. En fazla 50 IP adres aralıklarını girebilirsiniz. Bu IP adreslerinden oturum açan kullanıcıların iki aşamalı Doğrulamayı atla.
7. **Kaydet** düğmesine tıklayın.
8. Güncelleştirmeler uygulandıktan sonra tıklayın **Kapat**.

![Güvenilen IP'ler](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Uygulama Parolaları
Bazı uygulamalar, Office 2010 gibi eski veya ve Apple Mail iki aşamalı doğrulamayı desteklemez. İkinci doğrulama kabul edecek şekilde yapılandırılmamışlardır. Bu uygulamaları kullanmak için "uygulama parolaları" kullanmak geleneksel parolanız yerine gerekir. Uygulama parolası iki aşamalı Doğrulamayı atla ve çalışmaya devam etmek uygulama izin verir.

> [!NOTE]
> Office 2013 istemcilerin için modern kimlik doğrulaması
> 
> Office 2013 istemciler (Outlook gibi) ve yeni destek modern kimlik doğrulama protokolleri ve iki aşamalı doğrulamayla birlikte çalışmak üzere etkinleştirilebilir. Etkinleştirildikten sonra uygulama parolaları bu istemciler için gerekli değildir.  Daha fazla bilgi için bkz: [Office 2013 modern kimlik doğrulaması genel önizlemesi Duyuruldu](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-to-know-about-app-passwords"></a>Uygulama parolaları hakkında bilinmesi gereken önemli şeyler
Uygulama parolaları hakkında bilinmesi şeyleri önemli bir listesi verilmiştir.

* Uygulama parolaları yalnızca uygulama bir kez girilmesi gerekir. Kullanıcıların bunları izlemek ve bunları her zaman girmeniz gerekmez.
* Gerçek parola otomatik olarak oluşturulur ve kullanıcı tarafından sağlanmaz. Bu otomatik olarak oluşturulan parolanın bir saldırgan tahmin edilmesi daha zor olduğundan ve daha güvenlidir.
* Kullanıcı başına 40 parolalık bir sınırı yoktur. 
* Parolaları önbelleğe ve şirket içi senaryolarda kullanan uygulamalar, uygulama parolası Kurumsal kimlik dışında bilinen değil bu yana başarısız olan başlayabilir. Şirket içi Exchange e-postaları örneğidir ancak arşivlenen postanın bulutta olduğu. Aynı parola çalışmaz.
* Çok faktörlü kimlik doğrulamasını bir kullanıcı hesabında etkinleştirildiğinde, Outlook ve Lync gibi tarayıcı olmayan istemcilerinin çoğu ile uygulama parolaları kullanılabilir, ancak Windows gibi tarayıcı olmayan uygulamaları ile uygulama parolaları kullanarak yönetim eylemlerini gerçekleştirilemiyor Bu kullanıcı bir yönetici hesabı olsa bile PowerShell.  Bir hizmet hesabı PowerShell komut dosyalarını çalıştırmak için güçlü bir parola oluşturun ve o hesap için iki aşamalı doğrulamayı etkinleştirmeyin emin olun.

> [!WARNING]
> Uygulama parolaları burada istemcileri hem şirket içi ile iletişim kurmak ve Otomatik bulma uç noktaları bulut Karma ortamlarda çalışmıyor. Bu etki alanı parolalarının şirket içi kimlik doğrulaması için gereklidir ve uygulama parolaları ile bulut kimlik doğrulaması için gerekli kaynaklanır.

### <a name="naming-guidance-for-app-passwords"></a>Uygulama parolaları için adlandırma Kılavuzu
Uygulama parolası adlarının üzerinde kullanıldıkları aygıt yansıtmalıdır. Örneğin, Outlook, Word ve Excel gibi tarayıcı olmayan uygulamalara sahip bir dizüstü bilgisayarınız varsa, dizüstü adlı bir uygulama parolası oluşturabilir ve bu uygulamaların bu uygulama parolasını kullanabilirsiniz. Ardından, masaüstü bilgisayarınızda aynı uygulamaları için Masaüstü adlı başka bir uygulama parolası oluşturun. 

Microsoft, cihaz, uygulama başına değil bir uygulama parolası başına bir uygulama parolası oluşturma önerir.

### <a name="federated-sso-app-passwords"></a>Federasyon (SSO) uygulama parolaları
Azure AD şirket içi Windows Server Active Directory etki alanı Hizmetleri (AD DS) ile Federasyon (çoklu oturum açma) destekler. Kuruluşunuz Azure AD ile birleştirildiyse ve Azure multi-Factor Authentication kullanarak olacak, uygulama parolaları hakkında aşağıdaki bilgileri sizin için önemli olan. Bu bölüm, yalnızca federasyon (SSO) müşterileri için geçerlidir.

* Uygulama parolaları Azure AD tarafından doğrulanır ve bu nedenle Federasyon atlayabilir. Federasyon yalnızca etkin bir şekilde uygulama parolaları ayarlarken kullanılır.
* Federasyon (SSO) kullanıcılar için biz hiçbir zaman pasif akış aksine kimlik sağlayıcıyı (IDP) gidin. Parolalar kuruluş kimliği depolanır. Kullanıcının şirketten ayrılması durumunda, bu bilgileri gerçek zamanda DirSync kullanılarak kurumsal kimliğe akmasını vardır. Hesap devre dışı bırakma/silme, Azure AD'de uygulama parolasını devre dışı bırakma/silme geciktirme eşitlemek için üç saat sürebilir.
* Şirket için İstemci Erişimi Denetimi ayarları Uygulama Parolası tarafından onaylanmaz.
* Günlüğe kaydetme/denetleme yeteneği şirket içi kimlik doğrulaması uygulama parolası için kullanılabilir değildir.
* Bazı gelişmiş mimari tasarımları kuruluş kullanıcı adı ve parolaları ve uygulama parolaları bileşimini istemcilerle burada kimlik doğrulamasında bağlı olarak iki aşamalı doğrulamayı kullanırken gerektirebilir. Bir şirket içi altyapı karşı kimlik doğrulaması istemcileri için bir kuruluş kullanıcı adı ve parola kullanırsınız. Azure AD karşı kimlik doğrulaması istemcileri için uygulama parolası kullanırsınız.

  Örneğin, aşağıdakilerden oluşur bir mimari olduğunu varsayalım:

  * Şirket içi örneğinizi Azure AD ile Active Directory federasyonunu
  * Exchange online kullanıyorsanız
  * Özel olarak şirket içi Lync kullanma
  * Azure multi-Factor Authentication kullanma

  ![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

  Bu durumlarda, aşağıdakileri yapmalısınız:

  * İmzalama için Lync bileşenini, kuruluşların kullanıcı adı ve parola kullanın.
  * Exchange online bağlayan bir Outlook istemcisi adres defteri erişmeye çalışırken bir uygulama parolasını kullanın.

### <a name="allow-app-password-creation"></a>Uygulama parolası oluşturmaya izin ver
Varsayılan olarak, kullanıcıların uygulama parolaları oluşturulamıyor. Bu özelliği etkinleştirilmelidir. Kullanıcıların uygulama parolaları oluşturma olanağı izin vermek için aşağıdaki yordamı kullanın:

1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Yönetmek istediğiniz dizini seçin. 
4. Seçin **yapılandırın**
5. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarlarını Yönet**.
6. Radyo düğmesini işaretleyin **kullanıcıların tarayıcı olmayan uygulamalarda oturum açmak için uygulama parolaları oluşturmasına izin**.

![Uygulama parolaları oluşturma](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Uygulama parolaları oluşturma
Kullanıcıların uygulama parolaları, ilk kaydı sırasında oluşturabilirsiniz. Bunlar uygulama parolaları oluşturma izni veren kayıt işleminin sonunda seçeneği sunulur.

Kullanıcıların uygulama parolaları kayıttan sonra Azure portalında veya Office 365 portalı ayarlarını değiştirerek oluşturabilirsiniz. Daha fazla bilgi ve kullanıcılarınız için ayrıntılı adımlar için bkz: [Azure çok faktörlü kimlik doğrulaması'ndaki uygulama parolaları nedir](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Kullanıcıların güven cihazlar için çok faktörlü kimlik doğrulaması unutmayın
Cihazlar ve kullanıcılar güven tüm MFA kullanıcılar için boş bir özelliktir; tarayıcılar için çok faktörlü kimlik doğrulaması anımsama. Atlama MFA seçeneği başarılı bir gerçekleştirdikten sonra gün sayısı kümesini için kullanıcılara vermek tanır MFA kullanarak oturum açın. Bir kullanıcı iki aşamalı doğrulama aynı cihaza gerçekleştirebilir sayısını en aza indirerek bu kullanılabilirlik geliştirebilirsiniz.

Bir hesap veya aygıt aşılırsa, ancak, güvenilen cihazlar için MFA hatırlama güvenliği etkileyebilir. Bir kurumsal hesap güvenliği tehlikeye girdiğinde veya güvenilir bir cihaz kaybolur veya çalınırsa durumunda [tüm cihazlarda çok faktörlü kimlik doğrulama geri](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Bu işlem tüm cihazlar güvenilen durumundan iptal eder ve kullanıcı yeniden iki aşamalı doğrulamayı gerçekleştirmek için gereklidir. MFA'ndaki yönergeleri ile kendi cihazlarda geri yüklemek için kullanıcılar ayrıca söyleyebilirsiniz [iki aşamalı doğrulama için ayarlarınızı yönetme](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Nasıl çalışır?

Çok faktörlü kimlik doğrulaması çalışır bir kullanıcı ettiğinde tarayıcıda kalıcı bir tanımlama bilgisi ayarlayarak hatırlamak "için sorma **X** gün" oturum açma sırasında kutusu. Tanımlama bilgisinin süresinin kadar kullanıcı MFA için yeniden bu broswer istenmez. Kullanıcı aynı aygıtta farklı bir tarayıcı açar veya kendi tanımlama bilgilerini temizler, tekrar sorulur. 

"İçin sorma **X** gün" modern kimlik doğrulama desteği olup olmadığına bakılmaksızın, onay kutusu tarayıcı olmayan uygulamaları üzerinde gösterilen değil. Bu uygulamalar her saat yeni erişim belirteçleri sağlamak yenileme belirteçleri kullanın. Bir yenileme belirteci son zaman iki aşamalı doğrulamayı gerçekleştirildiğini doğrulanmış, Azure AD denetimleri olduğunda yapılandırılan gün sayısının içinde oluştu. 

Bu nedenle, güvenilen cihazlara MFA hatırlama (hangi normalde her zaman sor) web uygulamalarını kimlik doğrulama sayısını azaltır, ancak (hangi normalde her 90 günde istemi) modern kimlik doğrulaması istemcileri için kimlik doğrulama sayısını artırır.

> [!NOTE]
>Kullanıcıların AD FS ile Azure MFA sunucusu veya bir üçüncü taraf MFA çözümü için iki aşamalı doğrulamayı gerçekleştirdiğinizde bu özellik, AD FS'nin "Oturumumu açık bırak" özelliğiyle uyumlu değil. Kullanıcılarınızın "Oturumumu açık bırak" AD FS seçerseniz ve ayrıca MFA için cihazlarını güvenilir olarak işaretlemek, "MFA unutmayın" kaç gün süresi dolduktan sonra doğrulayamazsınız olmayacaktır. Azure AD yeni iki aşamalı doğrulama isteklerini, ancak AD FS yeniden iki aşamalı doğrulamayı gerçekleştirmek yerine tarih ve özgün MFA talep belirteciyle döndürür. Bu doğrulama döngü Azure arasında kapalı ayarlar AD ve AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Anımsa çok faktörlü kimlik doğrulamasını etkinleştir
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Yönetmek istediğiniz dizini seçin. 
4. Seçin **yapılandırın**
5. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarlarını Yönet**.
6. Hizmet Ayarları sayfasında, kullanıcı cihaz ayarları altında yönetmek denetleyin **kullanıcıların güvendikleri cihazlarda çok faktörlü kimlik doğrulaması unutmayın izin ver** kutusu.
   ![Aygıtları unutmayın](./media/multi-factor-authentication-whats-next/remember.png)
7. İki aşamalı doğrulamayı atlamak güvenilen cihazları izin vermek istediğiniz gün sayısını ayarlayın. Varsayılan değer 14 gündür.
8. **Kaydet** düğmesine tıklayın.
9. **Kapat**’a tıklayın.

### <a name="mark-a-device-as-trusted"></a>Bir aygıt güvenilen olarak işaretle

Bu özelliği etkinleştirdiğinizde, kullanıcılar bir aygıtın ne zaman güvenilir olarak işaretleyebilir denetleyerek oturum **bir daha sorma**.

![-Ekran görüntüsü sorma](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Seçilebilir doğrulama yöntemleri
Hangi doğrulama yöntemleri, kullanıcılarınız için kullanılabilir seçebilirsiniz. Aşağıdaki tabloda her bir yöntemin kısa bir genel bakış sağlar.

Kullanıcılarınız için MFA hesaplarına kaydettiğinizde, bunlar, etkin seçenekleri dışında tercih edilen doğrulama yöntemi seçin. İçinde kayıt işlemi için kılavuz ele [Hesabımı iki aşamalı doğrulama için ayarlama](multi-factor-authentication-end-user-first-time.md)

| Yöntem | Açıklama |
|:--- |:--- |
| Telefonu arama |Otomatik bir sesli aramayla yerleştirir. Kullanıcı telefonu yanıtladığında ve # kimliğini doğrulamak için telefon basar. Bu telefon numarası şirket içi Active Directory ile eşitlenmemiş. |
| Telefona kısa mesaj |Doğrulama kodunu içeren bir kısa mesaj gönderir. Kullanıcı ya da Yanıtla metin iletisi doğrulama kodunu veya oturum açma arabirimine doğrulama kodunu girmeniz istenir. |
| Mobil uygulama üzerinden bildirim |Telefonunuza veya kayıtlı cihazınıza anında iletme bildirimi gönderir. Kullanıcı bildirimi görünümleri ve seçer **doğrula** doğrulamayı tamamlamak için. <br>Microsoft Authenticator uygulaması [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), ve [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Mobil uygulamadan alınan doğrulama kodu |Microsoft Authenticator uygulamasını otuz her saniye yeni bir OATH doğrulama kodu oluşturur. Kullanıcı oturum açma arabirimine bu doğrulama kodu girer.<br>Microsoft Authenticator uygulaması [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), ve [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-to-enabledisable-authentication-methods"></a>Nasıl kimlik doğrulama yöntemleri etkinleştir/devre dışı bırak
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Sol taraftaki **Active Directory** öğesini seçin.
3. Yönetmek istediğiniz dizini seçin. 
4. Seçin **yapılandırın**
5. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarlarını Yönet**.
6. Hizmet Ayarları sayfasında, doğrulama seçeneklerini seçin/kullanmak istediğiniz seçenekleri seçimini kaldırın.
   ![Doğrulama seçenekleri](./media/multi-factor-authentication-whats-next/authmethods.png)
7. **Kaydet** düğmesine tıklayın.
8. **Kapat**’a tıklayın.

