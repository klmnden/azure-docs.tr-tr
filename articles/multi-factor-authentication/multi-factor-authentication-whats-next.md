---
title: "Azure çok faktörlü kimlik doğrulamasını yapılandırma | Microsoft Docs"
description: "Bu makalede raporları, sahtekarlık Uyarısı, tek seferlik atlamaların, özel sesli mesajları için önbelleğe alma, güvenilen IP'leri ve uygulama parolaları Azure çok faktörlü kimlik doğrulama ayarlarının nasıl yapılandırılacağı açıklanmaktadır."
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
ms.date: 11/29/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 4dce84becbf7d9758bd507e258b781b903fc64d9
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure çok faktörlü kimlik doğrulama ayarlarını yapılandırın

Bu makalede Azure çok faktörlü kimlik doğrulaması ve çalışıyor olduğunuz göre yönetmenize yardımcı olur. En Azure multi-Factor Authentication dışında elde size yardımcı çeşitli konuları kapsar. Tüm özellikler kullanılabilir olan her [Azure multi-Factor Authentication sürümü](/multi-factor-authentication-get-started.md#what-features-do-i-need).

| Özellik | Açıklama | 
|:--- |:--- |
| [Engelleme ve kullanıcıların Engellemeyi Kaldır](#block-and-unblock-users) |Kullanıcılar kimlik doğrulama isteklerini almasını önlemek için engelle/Engellemeyi Kaldır kullanıcılar özelliğini kullanın. |
| [Sahtekarlık Uyarısı](#fraud-alert) |Böylece kullanıcılarınızın kaynaklarına erişmek için sahte denemeleri raporlayabilirsiniz sahtekarlık uyarısı özelliğini yapılandırın. |
| [Bir kerelik atlama](#one-time-bypass) |Bir kereliğine kimlik doğrulaması yapmalarına izin vermek için tek seferlik atlama özelliğini kullanma _atlayarak_ çok faktörlü kimlik doğrulaması. |
| [Özel sesli mesajları](#custom-voice-messages) |Kendi kayıtları veya Tebrikler çok faktörlü kimlik doğrulamasıyla kullanılacak özel sesli iletileri özelliğini kullanın. |
| [Önbelleğe alma](#caching-in-azure-multi-factor-authentication) |Önbelleğe alma özelliği, belirli bir süre sonraki kimlik doğrulama girişimleri otomatik olarak başarılı şekilde ayarlamak için kullanın. |
| [Güvenilen IP'leri](#trusted-ips) |Federe veya yönetilen bir Kiracı Yöneticiler, kullanıcılar şirket intranetten oturum açmak için iki aşamalı doğrulamayı atlamak için güvenilen IP'ler özelliğini kullanabilirsiniz. |
| [Uygulama parolaları](#app-passwords) |Çok faktörlü kimlik doğrulamasını atlamak ve çalışmaya devam etmek için bir uygulama etkinleştirmek için uygulama parolası özelliğini kullanın. |
| [Güvenilen cihazlar ve tarayıcılar için çok faktörlü kimlik doğrulaması unutmayın](#remember-multi-factor-authentication-for-trusted-devices) |Çok faktörlü kimlik doğrulaması kullanarak başarıyla açan bir kullanıcıya sahip olduktan sonra gün sayısı kümesini için güvenilen cihazlar ve tarayıcılar unutmayın için bu özelliği kullanın. |
| [Seçilebilir doğrulama yöntemleri](#selectable-verification-methods) |Kullanıcıların kullanabilmek için kimlik doğrulama yöntemleri listesini seçmek için bu özelliği kullanın. |

## <a name="block-and-unblock-users"></a>Engelleme ve kullanıcıların Engellemeyi Kaldır
Kullanım _ya da kullanıcılar engelini kaldırmanız_ kullanıcılar kimlik doğrulama isteklerini almasını önlemek için özellik. Engellenen kullanıcılar için kimlik doğrulama girişimleri otomatik olarak reddedilir. Kullanıcılar engellenmiş durumda olan zamandan 90 gün boyunca engellenmiş olarak kalır.

### <a name="block-a-user"></a>Bir kullanıcı engelleme
1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.

2. Gözat **Azure Active Directory** > **MFA sunucusu** > **engelle/Engellemeyi Kaldır kullanıcılar**.

3. Seçin **Ekle** bir kullanıcıyı engellemek için.

4. Seçin **çoğaltma grubu**. Engellenen bir kullanıcı için kullanıcı adı girin **kullanıcıadı<span></span>@domain.com**. Bir yorum girin **neden** alan.

5. Seçin **Ekle** engelleyen kullanıcı tamamlamak için.

### <a name="unblock-a-user"></a>Bir kullanıcının engelini kaldırma
1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.

2. Gözat **Azure Active Directory** > **MFA sunucusu** > **engelle/Engellemeyi Kaldır kullanıcılar**.

3. Seçin **Engellemeyi Kaldır** içinde **eylem** Engellemesi kaldırılacak kullanıcının yanındaki sütuna.

4. Bir yorum girin **neden engellemelerini kaldırma için** alan.

5. Seçin **Engellemeyi Kaldır** kullanıcının engellemesini kaldırma tamamlamak için.

## <a name="fraud-alert"></a>Sahtekarlık uyarısı
Yapılandırma _sahtekarlık Uyarısı_ böylece kullanıcılarınızın kaynaklarına erişmek için sahte denemeleri raporlayabilirsiniz özelliği. Kullanıcıların mobil uygulamayı kullanarak veya telefon aracılığıyla sahtekarlık denemeleri bildirebilirsiniz.

### <a name="turn-on-fraud-alerts"></a>Sahtekarlık uyarılarını Aç
1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.

2. Gözat **Azure Active Directory** > **MFA sunucusu** > **sahtekarlık Uyarısı**.

   ![Sahtekarlık uyarılarını Aç](./media/multi-factor-authentication-whats-next/fraudalert.png)

3. Ayarlama **kullanıcıların sahtekarlık uyarısı göndermesine izin ver** ayarını **üzerinde**.

4. **Kaydet**’i seçin.

### <a name="configuration-options"></a>Yapılandırma seçenekleri

- **Sahtekarlık bildirildiğinde kullanıcıyı engelle**: kullanıcı sahtekarlık bildirirse, kendi hesabı 90 gün veya yönetici hesaplarına engelini kaldırır kadar engellendi. Bir yönetici oturum açma raporunu kullanarak oturum açma işlemleri gözden geçirin ve gelecekteki sahtekarlığı önlemek için uygun eylemi gerçekleştirin. Yönetici böylece [engellemesini](#unblock-a-user) kullanıcının hesabı.
- **İlk Karşılama sırasında sahtekarlık bildirme kodu**: kullanıcıların iki aşamalı doğrulamayı gerçekleştirmek için bir telefon araması aldığınızda, bunlar normalde tuşlarına basarak  **#**  kullanıcıların oturum açma onaylamak için. Sahtekarlık bildirme kodu tuşlarına basarak önce kullanıcının girdiği  **#** . Bu kodu **0** varsayılan olarak, ancak özelleştirebilirsiniz.

  >[!NOTE]
  >Microsoft'tan varsayılan sesli karşılamalar basın kullanıcılara söylemek **0#** bir sahtekarlık uyarısı göndermek için. Dışında bir kod kullanmak istiyorsanız, **0**, kaydetme ve kullanıcılarınız için uygun yönergelere kendi özel sesli karşılamalar yükleyin.
  >

### <a name="view-fraud-reports"></a>Sahtekarlık raporlarını görüntüle
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.

2. Sol taraftaki **Active Directory** öğesini seçin.

3. Yönetmek istediğiniz dizini seçin. 

4. Seçin **yapılandırma**.

5. Altında **çok faktörlü kimlik doğrulaması**seçin **hizmet ayarlarını Yönet**.

6. Ekranın alt kısmındaki **hizmet ayarlarını** sayfasında, **Portal'a Git**.

7. Azure multi-Factor Authentication Yönetim Portalı'nda altında **görünümü bir raporu**seçin **sahtekarlık Uyarısı**.

8. Raporda görüntülemek istediğiniz tarih aralığını girin. Ayrıca, kullanıcı adları, telefon numaraları ve kullanıcı durumunu da belirtebilirsiniz.

9. Seçin **çalıştırmak** sahtekarlık uyarısı raporunu görüntülemek için. Rapor vermek için seçin **CSV'ye aktar**.

## <a name="one-time-bypass"></a>Bir kerelik atlama
_Bir kerelik atlama_ iki aşamalı doğrulamayı gerçekleştirme olmadan bir seferliğine kimlik doğrulaması bir kullanıcı özelliği sağlar. Atlama geçicidir ve belirtilen sayıda saniye geçtikten sonra süresi dolar. Burada mobil uygulaması ya da telefon bildirim veya telefon görüşmesi almıyor durumlarda, kullanıcının istenen kaynak erişebilmesi için bir kerelik geçiş izin verebilirsiniz.

### <a name="create-a-one-time-bypass"></a>Bir kerelik geçiş oluşturmak

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.

2. Gözat **Azure Active Directory** > **MFA sunucusu** > **bir kerelik atlama**.

   ![Bir kerelik atlama oluşturma](./media/multi-factor-authentication-whats-next/onetimebypass.png)

3. **Add (Ekle)** seçeneğini belirleyin.

4. Gerekirse, geçiş için çoğaltma grubunu seçin.

5. Kullanıcı adı olarak girin **kullanıcıadı<span></span>@domain.com**. Atlamanın saniye sayısını girin. Atlama nedenini girin. 

6. **Add (Ekle)** seçeneğini belirleyin. Zaman sınırı hemen yürürlüğe girer. Kullanıcı, bir kerelik atlama süresi dolmadan önce oturum açması gerekiyor. 

### <a name="view-the-one-time-bypass-report"></a>Bir kerelik atlama raporunu görüntüle
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.

2. Sol taraftaki **Active Directory** öğesini seçin.

3. Yönetmek istediğiniz dizini seçin. 

4. Seçin **yapılandırma**.

5. Altında **çok faktörlü kimlik doğrulaması**seçin **hizmet ayarlarını Yönet**.

6. Ekranın alt kısmındaki **hizmet ayarlarını** sayfasında, **Portal'a Git**.

7. Azure multi-Factor Authentication Yönetim Portalı'nda altında **görünümü bir raporu**seçin **bir kerelik geçiş**.

8. Raporda görüntülemek istediğiniz tarih aralığını girin. Ayrıca, kullanıcı adları, telefon numaraları ve kullanıcı durumunu da belirtebilirsiniz.

9. Seçin **çalıştırmak** atlamaların raporunu görüntülemek için. Rapor vermek için seçin **CSV'ye aktar**.

## <a name="custom-voice-messages"></a>Özel sesli mesajları
İki aşamalı doğrulamayla birlikte için kendi kayıtları veya Tebrikler kullanabilirsiniz _özel sesli mesajları_ özelliği. Bu iletiler, ek olarak veya Microsoft kayıtlarını değiştirmek için kullanılabilir.

Başlamadan önce aşağıdaki kısıtlamalara dikkat edin:

* Desteklenen dosya biçimleri .wav ve .mp3 olacaktır.
* Dosya boyutu sınırını 5 MB'tır.
* Kimlik doğrulama iletileri 20 saniyeden daha kısa olmalıdır. 20 saniyeden daha uzun iletileri doğrulama başarısız olmasına neden olabilir. Kullanıcının, iletinin tamamlandıktan ve doğrulama zaman aşımına uğramadan önce yanıt vermeyebilir.

### <a name="set-up-a-custom-message"></a>Özel ileti ayarlayın

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.

2. Gözat **Azure Active Directory** > **MFA sunucusu** > **telefon araması ayarları**.

   ![Kayıt özel telefon iletileri](./media/multi-factor-authentication-whats-next/phonecallsettings.png)

3. Seçin **Ekle Tebrik**.

4. Karşılama türünü seçin. Dili seçin.

5. Karşıya yüklemek için bir .mp3 veya .wav ses dosyası seçin.

6. **Add (Ekle)** seçeneğini belirleyin.

## <a name="caching-in-azure-multi-factor-authentication"></a>Azure çok faktörlü kimlik doğrulaması önbelleğe alma
 Bir kullanıcı kullanarak doğrulandıktan sonra kimlik doğrulama girişimlerini izin vermek için bir süre ayarlayabilirsiniz _önbelleğe alma_ özelliği. Belirtilen kullanıcı için sonraki kimlik doğrulama girişimleri otomatik olarak süre başarılı. Önbelleğe alma, öncelikle ilk istek hala devam ederken şirket içi sistemlere, örneğin VPN, birden fazla doğrulama isteği gönderdiğinizde kullanılır. Bu özellik, kullanıcı ilk doğrulama devam ediyor başarılı olduktan sonra otomatik olarak başarılı olması sonraki isteklere izin verir. 

>[!NOTE]
>Önbelleğe alma özelliği, Azure Active Directory'ye (Azure AD) oturum açma işlemleri için kullanılmak üzere tasarlanmamıştır.

### <a name="set-up-caching"></a>Önbelleğe almayı kurmak 
1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.

2. Gözat **Azure Active Directory** > **MFA sunucusu** > **kuralları önbelleğe alma**.

   ![Kuralları önbelleğe almayı kurmak](./media/multi-factor-authentication-whats-next/cachingrules.png)

3. **Add (Ekle)** seçeneğini belirleyin.

4. Seçin **önbellek türü** aşağı açılan listeden. En fazla sayısını girin **önbelleğe saniye**. 

5. Gerekirse, bir kimlik doğrulama türünü seçin ve bir uygulama belirtin. 

6. **Add (Ekle)** seçeneğini belirleyin.


## <a name="trusted-ips"></a>Güvenilen IP'ler
_Güvenilen IP'leri_ Azure çok faktörlü kimlik doğrulama özelliği, yönetilen ya da Federasyon Kiracı Yöneticiler tarafından kullanılır. Bu özellik şirket intranetten oturum açan kullanıcılar için iki aşamalı doğrulamayı atlar. Özellik, Azure çok faktörlü kimlik doğrulaması'nın tam sürümünü ve Yöneticiler için değil ücretsiz sürümü ile kullanılabilir. Azure çok faktörlü kimlik doğrulaması'nın tam sürümünü alma hakkında daha fazla bilgi için bkz: [Azure çok faktörlü kimlik doğrulaması](multi-factor-authentication.md).

| Azure AD Kiracı türü | Güvenilen IP'leri özellik seçenekleri |
|:--- |:--- |
| Yönetilen |**Belirli IP adresi aralığı**: Yöneticiler şirket intranetten oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayan bir IP adresi aralığı belirtin.|
| Federasyon |**Tüm Federasyon kullanıcıları**: kuruluş içinde oturumunu tüm Federasyon kullanıcıları iki aşamalı doğrulamayı devre dışı bırakabilir. Kullanıcıların, Active Directory Federasyon Hizmetleri (AD FS) tarafından verilen bir talep kullanarak doğrulamayı atlama.<br/>**Belirli IP adresi aralığı**: Yöneticiler şirket intranetten oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayan bir IP adresi aralığı belirtin. |

Güvenilen IP'ler atlama yalnızca şirket intraneti içinde çalışır. Seçerseniz **tüm Federasyon kullanıcıları** seçeneği ve bir kullanıcı oturum açtığında gelen şirket intranet dışından kullanıcının iki aşamalı doğrulama kullanarak kimlik doğrulaması gerekir. Kullanıcı bir AD FS talep sunsa bile aynı işlemidir. 

**Corpnet içinde son kullanıcı deneyimi**

Güvenilen IP'ler özelliği devre dışı bırakıldığında, iki aşamalı doğrulamayı tarayıcı akışları için gereklidir. Uygulama parolaları eski zengin istemci uygulamaları için gereklidir. 

Güvenilen IP'ler özelliği etkinleştirilmişse, iki aşamalı doğrulamayı olan *değil* tarayıcı akışları için gereklidir. Uygulama parolaları *değil* kullanıcı bir uygulama parolası oluşturulmuş kurmadı koşuluyla eski zengin istemci uygulamaları için gereklidir. Bir uygulama parolası kullanıma alındıktan sonra parolayı gerekli kalır. 

**Son kullanıcı deneyimi corpnet dışında**

Güvenilen IP'ler özelliği etkinleştirilip etkinleştirilmediği bağımsız olarak, iki aşamalı doğrulamayı tarayıcı akışları için gereklidir. Uygulama parolaları eski zengin istemci uygulamaları için gereklidir. 

### <a name="enable-named-locations-by-using-conditional-access"></a>Adlandırılmış konumlara koşullu erişim kullanarak etkinleştirin

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol tarafta seçin **Azure Active Directory** > **koşullu erişim** > **konumları adlı**.

3. Seçin **yeni konum**.

4. Konum için bir ad girin.

5. Seçin **güvenilir konum olarak işaretle**.

6. CIDR gösteriminde gibi IP aralığını girin **192.168.1.1/24**.

7. **Oluştur**’u seçin.

### <a name="enable-the-trusted-ips-feature-by-using-conditional-access"></a>Koşullu erişim kullanarak güvenilen IP'leri özelliğini etkinleştir

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol tarafta seçin **Azure Active Directory** > **koşullu erişim** > **konumları adlı**.

3. Seçin **yapılandırma MFA güvenilen IP'leri**.

4. Üzerinde **hizmet ayarlarını** sayfasında **güvenilen IP'ler**, aşağıdaki iki seçeneklerden birini seçin:
   
   * **My intranetten gelen Federasyon kullanıcılarının istekleri için**: Bu seçeneği seçmek için onay kutusunu seçin. Tüm Federasyon AD FS tarafından verilen bir talep kullanarak oturum açma Kurumsal ağdan iki aşamalı Doğrulamayı atla kullanıcıları. AD FS uygun trafiği intranet talep eklemek için bir kuralı bulunduğundan emin olun. Kural yoksa, aşağıdaki kural AD FS'de oluşturun:<br/>

     ```
     c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);
     ```
     
   * **Belirli bir ortak IP aralığını gelen istekleri için**: Bu seçeneği seçmek için IP adreslerini metin kutusunda CIDR gösterimini kullanarak girin.
   
     * Aralık xxx.xxx.xxx.1 xxx.xxx.xxx.254 aracılığıyla bulunan IP adresleri için gibi gösterimini kullanın **xxx.xxx.xxx.0/24**.
     * Tek bir IP adresi gibi gösterimini kullanın **xxx<span></span>.xxx.xxx.xxx/32**.
     
     En fazla 50 IP adres aralıklarını girin. Bu IP adreslerinden oturum açan kullanıcıların iki aşamalı Doğrulamayı atla.

5. **Kaydet**’i seçin.

### <a name="enable-the-trusted-ips-feature-by-using-service-settings"></a>Hizmet ayarları kullanarak güvenilen IP'leri özelliğini etkinleştir

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol tarafta seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

3. Seçin **çok faktörlü kimlik doğrulaması**.

4. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarları**.

5. Üzerinde **hizmet ayarlarını** sayfasında **güvenilen IP'ler**, aşağıdaki iki seçeneklerden birini seçin:
   
   * **Federasyon kullanıcıları my intranet üzerinde yapılan istekler için**: Bu seçeneği seçmek için onay kutusunu seçin. Tüm Federasyon AD FS tarafından verilen bir talep kullanarak oturum açma Kurumsal ağdan iki aşamalı Doğrulamayı atla kullanıcıları. AD FS uygun trafiği intranet talep eklemek için bir kuralı bulunduğundan emin olun. Kural yoksa, aşağıdaki kural AD FS'de oluşturun:<br/>

     ```
     c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);
     ```

   * **Belirtilen IP adresi alt ağları gelen istekleri için**: Bu seçeneği seçmek için IP adreslerini metin kutusunda CIDR gösterimini kullanarak girin. 
     
     * Aralık xxx.xxx.xxx.1 xxx.xxx.xxx.254 aracılığıyla bulunan IP adresleri için gibi gösterimini kullanın **xxx.xxx.xxx.0/24**.
     * Tek bir IP adresi gibi gösterimini kullanın **xxx<span></span>.xxx.xxx.xxx/32**.
     
     En fazla 50 IP adres aralıklarını girin. Bu IP adreslerinden oturum açan kullanıcıların iki aşamalı Doğrulamayı atla.

6. **Kaydet**’i seçin.

![Güvenilen IP'leri ile hizmet ayarlarını etkinleştir](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Uygulama parolaları

Bazı uygulamalar, Office 2010 gibi veya önceki sürümleri ve Apple Mail iki aşamalı doğrulamayı desteklemez. Uygulamalar, ikinci doğrulama kabul edecek şekilde yapılandırılmamışlardır. Bu uygulamaları kullanmak için yararlanmak _uygulama parolaları_ özelliği. İki aşamalı Doğrulamayı atla ve çalışmaya devam etmek uygulama izin vermek için bir uygulama parolası yerine geleneksel parolanızı kullanabilirsiniz.

>[!NOTE]
>Daha sonra ve Microsoft Office 2013 istemcilerin için modern kimlik doğrulaması
> 
>Office 2013 istemcileri ve daha sonra (Outlook gibi), modern kimlik doğrulama protokollerini destekler ve iki aşamalı doğrulamayla birlikte çalışmak üzere etkinleştirilebilir. İstemciye etkinleştirildikten sonra uygulama parolaları istemci için gerekli değildir. Daha fazla bilgi için bkz: [Office 2013 modern kimlik doğrulaması genel önizlemesi duyuru](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).
>

### <a name="considerations-about-app-passwords"></a>Uygulama parolaları hakkında dikkat edilecek noktalar
Uygulama parolaları kullanırken aşağıdaki önemli noktaları göz önünde bulundurun:

* Uygulama parolaları uygulama başına yalnızca bir kez girilir. Kullanıcıların parolaları izlemek veya bunları her zaman girmeniz gerekmez.
* Gerçek parola otomatik olarak oluşturulur ve kullanıcı tarafından sağlanmaz. Otomatik olarak oluşturulan parolanın bir saldırgan tahmin edilmesi daha zor ve daha güvenlidir.
* Kullanıcı başına 40 parolalık bir sınırı yoktur. 
* Parolaları önbelleğe ve şirket içi senaryolarda kullanan uygulamalar, uygulama parolası dışında iş veya Okul hesabı bilinen değil çünkü başarısız olmasına başlatabilirsiniz. Bu senaryo, şirket içi Exchange e-postaları örneğidir ancak arşivlenen postanın bulutta olduğu. Bu senaryoda, aynı parola çalışmaz.
* Kullanıcı hesabında çok faktörlü kimlik doğrulamasını etkinleştirildikten sonra uygulama parolaları en tarayıcı olmayan istemcilerin Outlook ve Microsoft Skype gibi iş için kullanılabilir. Windows PowerShell gibi tarayıcı olmayan uygulamaları ile uygulama parolaları kullanarak yönetim eylemlerini gerçekleştirilemiyor. Kullanıcı bir yönetici hesabı olsa bile eylemler gerçekleştirilemez. PowerShell betikleri çalıştırmak için güçlü bir parolası olan bir hizmet hesabı oluşturun ve iki aşamalı doğrulama için hesap etkinleştirmeyin.

>[!WARNING]
>Uygulama parolaları burada istemcileri hem şirket içi ile iletişim kurmak ve Otomatik bulma uç noktaları bulut Karma ortamlarda çalışmıyor. Etki alanı parolaları, şirket içi kimlik doğrulaması için gereklidir. Uygulama parolaları ile bulut kimlik doğrulaması için gereklidir.
>

### <a name="guidance-for-app-password-names"></a>Uygulama parolası adlarının için yönergeler
Uygulama parolası adlarının üzerinde kullanıldıklarından aygıt yansıtmalıdır. Outlook, Word ve Excel gibi tarayıcı olmayan uygulamaları olan dizüstü bilgisayarınız varsa, adlı bir uygulama parolası oluşturmanız **dizüstü** bu uygulamalar için. Adlı başka bir uygulama parolası oluşturmanız **Masaüstü** masaüstü bilgisayarınızda çalışan aynı uygulamalar için. 

>[!NOTE]
>Uygulama başına bir uygulama parolası yerine cihaz başına bir uygulama parolası oluşturmanızı öneririz.

### <a name="federated-or-single-sign-on-app-passwords"></a>Federasyon ya da tek oturum açma uygulama parolaları
Azure AD, şirket içi Windows Server Active Directory etki alanı Hizmetleri ile (AD DS) Federasyon veya çoklu oturum açma (SSO) destekler. Kuruluşunuz Azure AD ile birleştirildiyse ve Azure çok faktörlü kimlik doğrulaması kullanıyorsanız, uygulama parolaları hakkında aşağıdaki noktaları göz önünde bulundurun.

>[!NOTE]
>Aşağıdaki noktaları yalnızca federasyon (SSO) müşteriler için geçerlidir.

* Uygulama parolaları Azure AD tarafından doğrulanır ve bu nedenle, Federasyon atlama. Federasyon yalnızca uygulama parolaları ayarlanırken etkin şekilde kullanılır.
* Pasif akış aksine Federasyon (SSO) kullanıcıları için kimlik sağlayıcıyı (IDP) temas değil. Uygulama parolaları iş veya Okul hesabı depolanır. Bir kullanıcının şirketten ayrılması durumunda kullanıcının bilgileri için iş veya Okul hesabı kullanarak akar **DirSync** gerçek zamanlı. Hesabı devre dışı bırakma/silme, Azure AD'de uygulama parolasını devre dışı bırak/silinmesini geciktirebilir eşitlemek için üç saat sürebilir.
* Şirket içi istemci erişim denetimi ayarları uygulama parolaları özelliğiyle dikkate alınır değil.
* Günlüğe kaydetme/denetleme yeteneği şirket içi kimlik doğrulaması uygulama parolaları özelliği ile kullanmak için kullanılabilir değildir.
* Bazı gelişmiş mimarileri istemcilerle iki aşamalı doğrulama için kimlik bilgilerini bileşimini gerektirir. Bu kimlik bilgileri, bir iş veya Okul hesabı kullanıcı adı ve parolaları ve uygulama parolaları içerebilir. Kimlik doğrulaması nasıl gerçekleştirildiğini gereksinimleri bağlıdır. Bir şirket içi altyapı karşı kimlik doğrulaması istemcileri için bir iş veya Okul hesabı kullanıcı adı ve parola gerekli. Azure AD karşı kimlik doğrulaması istemcileri için bir uygulama parolası gereklidir.

  Örneğin, aşağıdaki mimari olduğunu varsayalım:

  * Şirket içi Active Directory örneğini Azure AD ile birleştirildiyse.
  * Exchange online kullanıyorsanız.
  * Skype Kurumsal şirket içi için kullanıyorsunuz.
  * Azure çok faktörlü kimlik doğrulaması kullanıyorsanız.

  ![Federe bir kuruluşta uygulama parolaları kullanma](./media/multi-factor-authentication-whats-next/federated.png)

  Bu senaryoda, aşağıdaki kimlik bilgilerini kullanın:

  * Skype Kurumsal oturum açmak için iş veya Okul hesabı kullanıcı adı ve parola kullanın.
  * Exchange online bağlayan bir Outlook istemcisinden adres defteri erişmek için bir uygulama parolasını kullanın.

### <a name="allow-users-to-create-app-passwords"></a>Kullanıcıların uygulama parolaları oluşturmasına izin ver
Varsayılan olarak, kullanıcıların uygulama parolaları oluşturulamıyor. Uygulama parolaları özelliği etkinleştirilmelidir. Kullanıcıların uygulama parolaları oluşturma olanağı vermek için aşağıdaki yordamı kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol tarafta seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

3. Seçin **çok faktörlü kimlik doğrulaması**.

4. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarları**.

5. Üzerinde **hizmet ayarlarını** sayfasında, **kullanıcıların tarayıcı olmayan uygulamalara oturum açmak için uygulama parolaları oluşturmasına izin** seçeneği.

   ![Kullanıcıların uygulama parolaları oluşturmasına izin ver](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Uygulama parolaları oluşturma
Kullanıcıların uygulama parolaları, ilk kaydı sırasında oluşturabilirsiniz. Kullanıcı kayıt işleminin sonunda uygulama parolaları oluşturma seçeneğiniz vardır.

Kullanıcıların uygulama parolaları kayıttan sonra oluşturabilirsiniz. Uygulama parolaları, Azure portalında veya Office 365 portalında ayarlarıyla değiştirilebilir. Daha fazla bilgi ve kullanıcılarınız için ayrıntılı adımlar için bkz: [Azure çok faktörlü kimlik doğrulaması'ndaki uygulama parolaları nedir?](./end-user/multi-factor-authentication-end-user-app-passwords.md)

<a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>
## <a name="remember-multi-factor-authentication-for-trusted-devices"></a>Çok faktörlü kimlik doğrulaması için güvenilen cihazlar unutmayın
_Çok faktörlü kimlik doğrulaması unutmayın_ özelliği cihazları ve kullanıcı tarafından güvenilen tarayıcılar tüm çok faktörlü kimlik doğrulaması kullanıcılar için ücretsiz özelliğidir. Kullanıcılar, belirtilen sayıda gün, bunlar başarıyla bir aygıta çok faktörlü kimlik doğrulaması kullanarak açan sonra sonraki Doğrulamalar atlayabilirsiniz. Özellik, bir kullanıcı, aynı cihazda iki aşamalı doğrulamayı gerçekleştirmek için sahip sayısını en aza indirerek kullanılabilirlik iyileştirir.

>[!IMPORTANT]
>Bir hesap veya aygıt aşılırsa, çok faktörlü kimlik doğrulaması için güvenilen cihazlar hatırlamak güvenlik etkileyebilir. Bir kurumsal hesap güvenliği tehlikeye girdiğinde veya güvenilir bir cihaz kaybolur veya çalınırsa durumunda [tüm cihazlarda çok faktörlü kimlik doğrulama geri](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user).
>
>Tüm cihazlar güvenilen durumundan geri yükleme eylemini iptal eder ve kullanıcı yeniden iki aşamalı doğrulamayı gerçekleştirmek için gerekli. Ayrıca çok faktörlü kimlik doğrulaması'ndaki yönergeleri ile kendi cihazlarda geri yüklemek için kullanıcılarınızın söyleyebilirsiniz [iki aşamalı doğrulama için ayarlarınızı yönetme](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted).
>

### <a name="how-the-feature-works"></a>Özelliği nasıl çalışır

Kullanıcı seçtiğinde anımsa çok faktörlü kimlik doğrulama özelliği tarayıcıda kalıcı bir tanımlama bilgisi ayarlar **X için bir daha sorma gün** oturum açma seçeneği. Tanımlama bilgisinin süresinin kadar kullanıcıdan yeniden çok faktörlü kimlik doğrulaması için o aynı tarayıcıdan istenir değildir. Kullanıcı aynı aygıtta farklı bir tarayıcı açar veya kendi tanımlama bilgilerini temizler, bunlar yeniden doğrulayın istenir. 

**X için bir daha sorma gün** seçeneği tarayıcı olmayan uygulamaları, uygulama modern kimlik doğrulamasını destekleyip desteklemediğini bağımsız olarak gösterilen değil. Bu uygulamaları kullanmak _yenileme belirteçleri_ her saat yeni erişim belirteçleri sağlar. Bir yenileme belirteci doğrulandığında, Azure AD son iki aşamalı doğrulamayı belirtilen gün sayısı içinde oluştu denetler. 

Özellik normalde her zaman sor web uygulamalarını kimlik doğrulama sayısını azaltır. Özelliğin normalde her 90 günde sor modern kimlik doğrulaması istemcileri için kimlik doğrulama sayısını artırır.

>[!IMPORTANT]
>**Çok faktörlü kimlik doğrulaması unutmayın** özelliği ile uyumlu değil **Oturumumu açık bırak** kullanıcılar yapılırken iki aşamalı doğrulama AD FS ile Azure multi-Factor için AD FS özelliği Kimlik doğrulama sunucusu veya bir üçüncü taraf çok faktörlü kimlik doğrulama çözümüdür.
>
>Kullanıcılarınızın seçerseniz **Oturumumu açık bırak** AD FS ve cihazlarını olarak çok faktörlü kimlik doğrulaması için güvenilen işareti Ayrıca, kullanıcının otomatik olarak sonra doğrulandı değil **çok faktörlü kimlik doğrulamasıunutmayın**gün sayısı sona. Azure AD yeni iki aşamalı doğrulama isteklerini, ancak AD FS özgün çok faktörlü kimlik doğrulaması talep ve tarih yerine gerçekleştirme iki aşamalı doğrulamayı yeniden belirteciyle döndürür. Azure arasında doğrulama döngüsü kapalı bu tepki ayarlar AD ve AD FS.
>

### <a name="enable-remember-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını etkinleştir unutmayın
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol tarafta seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

3. Seçin **çok faktörlü kimlik doğrulaması**.

4. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarları**.

5. Üzerinde **hizmet ayarlarını** sayfasında **yönetmek çok faktörlü kimlik doğrulaması unutmayın**seçin **çok faktörlü kimlik doğrulaması anımsaması kullanıcılarıngüvendiklericihazlarda**seçeneği.

   ![Çok faktörlü kimlik doğrulaması için güvenilen cihazlar unutmayın](./media/multi-factor-authentication-whats-next/remember.png)

6. İki aşamalı doğrulamayı atlamak güvenilen cihazlar izin vermek için gün sayısını ayarlayın. Varsayılan değer 14 gündür.

7. **Kaydet**’i seçin.

### <a name="mark-a-device-as-trusted"></a>Bir aygıt güvenilen olarak işaretle

Anımsa çok faktörlü kimlik doğrulama özelliği etkinleştirdikten sonra kullanıcılar bir aygıtın ne zaman güvenilir olarak işaretleyebilir seçerek oturum **bir daha sorma**.

!["Güvenilen cihazlar için sorma" seçin](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Seçilebilir doğrulama yöntemleri
Kullanarak, kullanıcılarınız için kullanılabilir olan doğrulama yöntemlerini seçebilir _seçilebilir doğrulama yöntemlerini_ özelliği. Aşağıdaki tabloda yöntemleri kısa bir genel bakış sağlar.

Kullanıcılarınız için Azure multi-Factor Authentication hesaplarını kaydettiğinizde, etkinleştirdiğiniz seçenekleri arasından, tercih edilen doğrulama yöntemi seçin. Kullanıcı kayıt işlemine yönelik yönergeler sağlanır [Hesabımı iki aşamalı doğrulama için ayarlama](multi-factor-authentication-end-user-first-time.md).

| Yöntem | Açıklama |
|:--- |:--- |
| Telefonu arama |Otomatik bir sesli aramayla yerleştirir. Kullanıcı telefonu yanıtladığında ve # kimliğini doğrulamak için telefon basar. Telefon numarası şirket içi Active Directory ile eşitlenmemiş. |
| Telefona kısa mesaj |Bir doğrulama kodu içeren bir kısa mesaj gönderir. Kullanıcı oturum açma arabirimine doğrulama kodunu girmeniz istenir. Bu işlem tek yönlü SMS adı verilir. İki yönlü SMS kullanıcının belirli bir kodu metnin geri gerekir anlamına gelir. İki yönlü SMS kullanım ve 14 Kasım 2018 sonra desteklenmiyor. İki yönlü SMS için otomatik olarak geçiş için yapılandırılmış olan kullanıcılar _telefon çağrısı_ o zaman doğrulama.|
| Mobil uygulama üzerinden bildirim |Telefonunuza veya kayıtlı cihazınıza anında iletme bildirimi gönderir. Kullanıcı bildirimi görünümleri ve seçer **doğrula** doğrulamayı tamamlamak için. Microsoft Authenticator uygulaması [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), ve [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Mobil uygulamadan alınan doğrulama kodu |Microsoft Authenticator uygulamasını her 30 saniyede yeni bir OATH doğrulama kodu oluşturur. Kullanıcı oturum açma arabirimine doğrulama kodunu girer. Microsoft Authenticator uygulaması [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), ve [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="enable-and-disable-verification-methods"></a>Etkinleştirme ve doğrulama yöntemlerini devre dışı
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol tarafta seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.

3. Seçin **çok faktörlü kimlik doğrulaması**.

4. Çok faktörlü kimlik doğrulaması altında seçin **hizmet ayarları**.

5. Üzerinde **hizmet ayarlarını** sayfasında **doğrulama seçeneklerini**, kullanıcılarınıza sağlamak için yöntemleri seçebilirsiniz/seçimini.

   ![Doğrulama yöntemlerini seç](./media/multi-factor-authentication-whats-next/authmethods.png)

6. **Kaydet** düğmesine tıklayın.
