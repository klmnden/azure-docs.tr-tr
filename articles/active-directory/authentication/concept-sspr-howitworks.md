---
title: Derin Dalış - Azure Active Directory Self Servis parola sıfırlama
description: Nasıl iş Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 47a6f475b5f1152850ec918b196883c6974f4d95
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60415642"
---
# <a name="how-it-works-azure-ad-self-service-password-reset"></a>Nasıl çalışır? Azure AD self servis parola sıfırlama

Self Servis parola, (SSPR) iş nasıl sıfırlansın mı? Bu seçenek arabiriminin ne demektir? Azure Active Directory (Azure AD) SSPR hakkında daha fazla bilgi için okumaya devam edin.

|     |
| --- |
| Mobil uygulama bildirimi ve mobil uygulama kodu olarak yöntemleri için Azure AD Self Servis parola sıfırlama, Azure Active Directory genel Önizleme özelliklerinden sunulmuştur. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="how-does-the-password-reset-portal-work"></a>Parola, portal iş nasıl sıfırlansın mı?

Bir kullanıcı görünümüne döndüğünde parola sıfırlama portalı, belirlemek için bir iş akışı başlatılır:

   * Sayfa yerelleştirilmiş ne olmalıdır?
   * Kullanıcı hesabının geçerli mi?
   * Hangi kuruluş için kullanıcıya ait?
   * Kullanıcının parolasını yönetildiği?
   * Kullanıcı bu özelliği kullanmak için mudur?

Sıfırlama sayfası parola ardındaki mantığı hakkında bilgi edinmek için aşağıdaki adımları okuyun:

1. Kullanıcının seçtiği **hesabınıza erişemiyor** doğrudan giden veya bağlantı [ https://aka.ms/sspr ](https://passwordreset.microsoftonline.com).
   * Tarayıcı yerel ayarına bağlı, deneyime uygun dilde işlenir. Parola sıfırlama deneyiminde, Office 365'in desteklediği aynı dilde yerelleştirilmiş olan.
   * Parola sıfırlama görüntülemek için farklı bir portal yerelleştirilmiş dil Ekle "? mkt =" yerelleştirme İspanyolca için aşağıdaki örnek ile URL sonuna parola sıfırlama [ https://passwordreset.microsoftonline.com/?mkt=es-us ](https://passwordreset.microsoftonline.com/?mkt=es-us).
2. Kullanıcı, kullanıcı Kimliğini girer ve captcha geçirir.
3. Azure AD, kullanıcının aşağıdaki denetimleri yaparak bu özelliği kullanabilmek için olduğunu doğrular:
   * Kullanıcı bu özelliği etkin olup olmadığını denetler ve Azure AD'yi sahip lisansı atandı.
     * Kullanıcı olmayan bu özelliğin etkinleştirilmesini veya atanmış bir lisansı olması, kullanıcı parolalarını sıfırlamak için yönetici ile iletişime geçin istenir.
   * Kullanıcı hesaplarına yönetici ilkesine uygun olarak tanımlanan uygun kimlik doğrulama yöntemleri olup olmadığını denetler.
     * Ardından, ciddi bir şekilde ilkesi yalnızca bir yöntem gerektiriyorsa, kullanıcı tanımlı en az bir yönetici İlkesi tarafından etkin kimlik doğrulama yöntemleri için uygun veri sahip olmasını sağlar.
       * Kimlik doğrulama yöntemlerini yapılandırılmamışsa, kullanıcı parolalarını sıfırlamak için kendi yöneticisine başvurmanız önerilir.
     * Ardından, ciddi bir şekilde İlkesi iki yöntem gerektiriyorsa, kullanıcı tanımlı en az iki Yönetici İlkesi tarafından etkin kimlik doğrulama yöntemleri için uygun veri sahip olmasını sağlar.
       * Kimlik doğrulama yöntemlerini yapılandırılmamışsa, kullanıcı parolalarını sıfırlamak için kendi yöneticisine başvurmanız önerilir.
     * Azure yöneticisi rolleri kullanıcıya atanmış ise, güçlü iki ağ geçidi parola ilkesini zorunlu tutulur. Bu ilke hakkında daha fazla bilgi bölümünde bulunabilir [yönetici sıfırlama İlkesi farklar](concept-sspr-policy.md#administrator-reset-policy-differences).
   * Kullanıcının parolası olup olmadığını görmek için denetimleri şirket içinde (Federal, geçişli kimlik doğrulaması veya parola karması eşitlenmiş) yönetilir.
     * Geri yazma dağıtılır ve yönetilen şirket kullanıcı parolasının ise, kullanıcının kimliğini doğrulamak ve kullanıcının parolasını sıfırlamak için devam izin verilmez.
     * Kullanıcının parolasını sıfırlamak için yönetici iletişim kurması istenir daha sonra yönetilen şirket içi geri yazma dağıtılmadığı ve kullanıcının parolasını ise.
4. Kullanıcı başarıyla parolalarını sıfırlayabilir olduğunu belirlenirse kullanıcı sıfırlama işlemini destekli.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

SSPR etkinleştirilirse, kimlik doğrulama yöntemleri için aşağıdaki seçeneklerden en az birini seçmeniz gerekir. Bu seçenekler "kapılarını" anılan bazen işittiğiniz Yüksek oranda olmasını öneririz, **iki veya daha fazla kimlik doğrulama yöntemlerini seçmesine** böylece kullanıcılarınız, gerektiğinde bunlar bir erişim oluşturulamıyor olması durumunda, daha fazla esnekliğine sahip olursunuz. Aşağıda listelenen yöntemleri hakkında ek bilgi makalesinde bulunabilir [kimlik doğrulama yöntemleri nelerdir?](concept-authentication-methods.md).

* Mobil uygulama bildirimi (önizleme)
* Mobil uygulama kodu (önizleme)
* Email
* Cep telefonu
* Ofis telefonu
* Güvenlik soruları

Bunlar yönetici etkinleştirilmiş kimlik doğrulama yöntemleri mevcut verileriniz varsa, kullanıcılar yalnızca parolasını sıfırlayabilirsiniz.

> [!IMPORTANT]
> Telefon araması seçenekleri 2019'ın Mart başlangıç MFA ve SSPR kullanıcıları Azure AD ücretsiz/deneme kiracıları için kullanılabilir olmayacak. SMS iletileri, bu değişiklikten etkilenmez. Telefon Araması'nda kullanıcılara kullanılabilir Ücretli Azure AD kiracılarıyla devam eder. Bu değişiklik, yalnızca Azure AD ücretsiz/deneme kiracıları etkiler.

> [!WARNING]
> Azure yönetici rolleri atanan hesaplar bölümünde tanımlanan yöntemleri kullanmak için gerekli olacak [yönetici sıfırlama İlkesi farklar](concept-sspr-policy.md#administrator-reset-policy-differences).

![Azure Portalı'nda kimlik doğrulama yöntemleri seçimi][Authentication]

### <a name="number-of-authentication-methods-required"></a>Gerekli kimlik doğrulama yöntemleri sayısı

Bu seçenek kullanılabilir kimlik doğrulama yöntemleri veya ağ geçitleri bir kullanıcı sıfırlama veya kilidini açma parolasını geçmesi gereken en düşük sayısını belirler. Bir veya iki ayarlanabilir.

Kullanıcılar yöneticinin, kimlik doğrulama yöntemi sağlar. daha fazla kimlik doğrulama yöntemleri sağlamanız seçebilirsiniz.

Bir kullanıcının kayıtlı gerekli en düşük yöntemleri yoksa bunları yönetici parolasını sıfırlama isteğinde yönlendiren bir hata sayfası görürler.

#### <a name="mobile-app-and-sspr-preview"></a>Mobil uygulama ve SSPR (Önizleme)

Microsoft Authenticator uygulaması gibi bir mobil uygulama, parola sıfırlama için bir yöntem olarak kullanırken aşağıdaki uyarılar bilmeniz gerekir:

* Yöneticiler bir yöntem gerektirdiğinde olması bir parola sıfırlama için kullanılan doğrulama kodu kullanılabilecek tek seçenek budur.
* Yöneticiler iki yöntem gerektirdiğinde olması bir parola sıfırlama için kullanılan, kullanıcıların kullanmaya **EITHER** bildirim **veya** doğrulama kodu yanı sıra diğer yöntemleri etkinleştirildi.

| Sıfırlama için gereken yöntemlerin sayısı | Bir | İki |
| :---: | :---: | :---: |
| Kullanılabilir mobil uygulama özellikleri | Kod | Kod veya bildirim |

Kullanıcılar, gelen Self Servis parola sıfırlama için kaydolurken mobil uygulamasını kaydetme seçeneği yoktur [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup). Kullanıcılar kendi mobil uygulamamız üzerinden kaydedebilir [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup), veya yeni güvenlik bilgileri kayıt Önizleme [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo).

> [!WARNING]
> Etkinleştirmelisiniz [Self Servis parola sıfırlama ve Azure multi-Factor Authentication (genel Önizleme) için kayıt yakınsanmış](concept-registration-mfa-sspr-converged.md) kullanıcılar, yeni deneyimi erişebilir önce [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo).

### <a name="change-authentication-methods"></a>Kimlik doğrulama yöntemleri değiştirme

Sıfırlama veya kilidini yalnızca bir gerekli kimlik doğrulama yöntemi için olan bir ilke ile başlatırsanız, kayıtlı ve iki yöntem için ne olacağını değiştirmek?

| Kayıtlı yöntemleri sayısı | Gereken yöntemlerin sayısı | Sonuç |
| :---: | :---: | :---: |
| 1 veya daha fazla | 1 | **Mümkün** sıfırlama veya kilidini açmak için |
| 1 | 2 | **%S** sıfırlama veya kilidini açmak için |
| 2 veya daha fazla | 2 | **Mümkün** sıfırlama veya kilidini açmak için |

Bir kullanıcı kullandığı kimlik doğrulama yöntemlerini türlerini değiştirirseniz, veri kullanılabilir en az miktarda yoksa SSPR kullanabilmek için kullanıcıların yanlışlıkla durabilir.

Örnek:
1. Özgün ilkenin gereken iki kimlik doğrulama yöntemleri ile yapılandırılır. Bu, yalnızca ofis telefon numarası ve güvenlik sorularını kullanır. 
2. Yönetici güvenlik sorularını artık kullanılacak ilkeyi değiştirir, ancak bir cep telefonu ve alternatif e-posta kullanımına izin verir.
3. Cep telefonu veya alternatif e-posta alanları doldurulmuş olmayan kullanıcıların parolalarını sıfırlayamaz.

## <a name="registration"></a>Kayıt

### <a name="require-users-to-register-when-they-sign-in"></a>Oturum açarken kaydolmalarını iste

Bu seçenek etkinleştirildiğinde, kullanıcılar Azure AD'yi kullanarak uygulamalarda oturum açarsanız parola sıfırlama kaydı tamamlamak bir kullanıcı gerektirir. Bu iş akışı aşağıdaki uygulamaları içerir:

* Office 365
* Azure portal
* Erişim Paneli
* Federasyon uygulamaları
* Azure AD kullanan özel uygulamalar

Kayıt gerektiren devre dışı bırakıldığında, kullanıcıların el ile kaydedebilirsiniz. Bunlar ya da ziyaret edebilirsiniz [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup) veya **parola sıfırlama için kaydolma** altında bağlantı **profili** erişim panelinde sekmesi.

> [!NOTE]
> Kullanıcılar, parola sıfırlama kayıt portalı seçerek kapatabilir **iptal** veya penceresini kapatarak. Ancak kullanıcılar, kayıt işlemini tamamlayana kadar oturum her zaman kaydı istenir.
>
> Bu kesme, bunlar zaten oturum açmış kullanıcının bağlantı sonu gelmez.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Kullanıcıların kendi kimlik doğrulama bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısını ayarla

Bu seçenek ayarlama ve kimlik doğrulama bilgilerini reconfirming arasındaki süreyi belirler ve yalnızca etkinleştirirseniz, kullanılabilir **oturum açarken kaydolmalarını iste** seçeneği.

Geçerli değerler 0 ile 730 gün "0" Kullanıcıların kendi kimlik doğrulama bilgilerini yeniden onaylamasını hiçbir zaman istenir anlamına gelir.

## <a name="notifications"></a>Bildirimler

### <a name="notify-users-on-password-resets"></a>Parola sıfırlamayı kullanıcılara bildir

Bu seçenek ayarlanırsa **Evet**, kullanıcıların kendi parolalarını sıfırlama parolasını değiştirdiğinden emin bildiren bir e-posta alabilir. E-posta, Azure AD'de dosya olan birincil veya alternatif e-postaları için SSPR portalı gönderilir. Başka hiç kimse sıfırlama olayı bildirilir.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildir

Bu seçenek ayarlanırsa **Evet**, ardından *tüm yöneticilerin* Azure AD'de dosya çubuğunda, birincil e-posta adresine bir e-posta alırsınız. E-posta bunları SSPR'ı kullanarak başka bir yönetici parolasını değiştirdiğinden bildirir.

Örnek: Bir ortamda dört yöneticileri vardır. Bir yönetici, SSPR'ı kullanarak kullanıcının parolasını sıfırlar. Yöneticiler, B ve C D bunları parola sıfırlama uyarı e-posta alırsınız.

## <a name="on-premises-integration"></a>Şirket içi tümleştirme

Yükleme, yapılandırma ve Azure AD Connect etkinleştirmek, şirket içi tümleştirmeler aşağıdaki ek seçenekleri sunulmaktadır. Bu seçenekler gri, daha sonra geri yazma düzgün şekilde yapılandırılmadı. Daha fazla bilgi için [parola geri yazmayı yapılandırmayla](howto-sspr-writeback.md).

![Parola geri yazma doğrulama etkinleştirildi ve çalışma][Writeback]

Bu sayfayı hızlı bir şirket içi geri yazma istemci durumunu sağlar, geçerli yapılandırmanız temelinde şu iletilerden biri görüntülenir:

* Şirket içi geri yazma istemciniz çalışır durumda.
* Azure AD, çevrimiçi ve şirket içi geri yazma istemcinize bağlı. Ancak, Azure AD Connect'in yüklü sürümü güncel değil gibi görünüyor. Göz önünde bulundurun [Azure AD Connect'i yükseltmeniz](../hybrid/how-to-upgrade-previous-version.md) son bağlantı özelliklerinden ve önemli hata düzeltmelerinden olmasını sağlamak için.
* Ne yazık ki, size Azure AD Connect'in yüklü sürümü güncel olmadığından, şirket içi geri yazma istemcinizin durumunu denetleyemiyor. [Azure AD Connect'e yükseltmenin](../hybrid/how-to-upgrade-previous-version.md) bağlantı durumunuzu denetleyebilmek için.
* Ne yazık ki şirket içi geri yazma istemcinize şu anda bağlanamıyoruz gibi görünüyor. [Azure AD Connect sorun giderme](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) bağlantı geri yüklemek için.
* Ne yazık ki, parola geri yazma düzgün şekilde yapılandırılmadı çünkü şirket içi geri yazma istemcinize bağlanamıyoruz. [Parola geri yazmayı yapılandırın](howto-sspr-writeback.md) bağlantı geri yüklemek için.
* Ne yazık ki şirket içi geri yazma istemcinize şu anda bağlanamıyoruz gibi görünüyor. Bu, bizim son geçici sorunlar nedeniyle olabilir. Sorun devam ederse [sorun giderme Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) bağlantı geri yüklemek için.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Parolalar şirket içi dizininize geri yazmak

Bu denetim, bu dizin için parola geri yazma özelliğinin etkin olup olmadığını belirler. Geri yazma açıksa, şirket içi geri yazma hizmetinin durumunu gösterir. Bu denetim, parola geri yazma'nın Azure AD Connect'i yeniden yapılandırmak zorunda kalmadan geçici olarak devre dışı bırakmak istiyorsanız kullanışlıdır.

* Anahtar ayarlanırsa **Evet**geri yazma sonra etkinleştirilir ve birleştirilmiş, geçişli kimlik doğrulaması veya parola karması eşitlenmiş kullanıcılar parolalarını sıfırlayabilir.
* Anahtar ayarlanırsa **Hayır**sonra geri yazmayı devre dışı bırakıldı ve birleştirilmiş, geçişli kimlik doğrulaması veya parola karması eşitlenmiş kullanıcılar parolalarını sıfırlayabilir değildir.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Kullanıcıların parolalarını sıfırlamadan hesapların kilidini açmak için

Bu denetim seçeneği kullanıcının parolasını sıfırlamak zorunda kalmadan, şirket içi Active Directory hesaplarını kilidini açmak için parola sıfırlama portalını ziyaret eden kullanıcılara verilmesi gerektiğini olup olmadığını belirler. Parola sıfırlama gerçekleştirdiğinde, varsayılan olarak, Azure AD hesaplarının kilidini açar. Bu iki işlemi birbirinden ayırmak için bu ayarı kullanın.

* Varsa kümesine **Evet**, sonra da kullanıcıların parolalarını sıfırlama ve hesabın kilidini açma veya parolayı sıfırlamak zorunda kalmadan hesaplarının kilidini seçeneği sunulur.
* Varsa kümesine **Hayır**, ardından kullanıcıları olan yalnızca bir birleşik bir parola sıfırlama gerçekleştirebilir ve hesap kilidini açma işlemi.

### <a name="on-premises-active-directory-password-filters"></a>Şirket içi Active Directory parola filtreleri

Azure AD Self Servis parola sıfırlama, bir yönetici tarafından başlatılan parola sıfırlama Active Directory'de denk gerçekleştirir. Özel parola kurallarını zorunlu tutmak için üçüncü taraf parola filtresi kullanıyorsanız ve bu parola filtresi sırasında Azure AD Self Servis denetlenir gerektiren varsa, parola sıfırlama, üçüncü taraf parola filtresi çözümü olarak uygulamak için yapılandırıldığından emin olun Senaryo yönetici parolasını sıfırlayın. [Windows Server Active Directory için Azure AD parola koruması](concept-password-ban-bad-on-premises.md) varsayılan olarak desteklenir.

## <a name="password-reset-for-b2b-users"></a>B2B kullanıcılar için parola sıfırlamayı

Parola sıfırlama ve değiştirme, tüm işletmeler arası (B2B) yapılandırmaları üzerinde tam olarak desteklenir. B2B kullanıcı parola sıfırlama, aşağıdaki üç durumda desteklenir:

* **Mevcut bir Azure AD kiracısı ile bir iş ortağı kuruluştan kullanıcılar**: Mevcut bir Azure AD kiracısı ile işbirliği kuruluş varsa, biz *her parola sıfırlama ilkeleri, Kiracı üzerinde etkin saygı*. Çalışmak için parola sıfırlama için iş ortağı kuruluşun yalnızca Azure AD SSPR etkin olduğundan emin olmak gerekir. Office 365 müşterileri için ek ücret yoktur ve adımları izleyerek etkinleştirilebilir bizim [parola yönetimini kullanmaya başlama](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) Kılavuzu.
* **Aracılığıyla kaydolun kullanıcılar** Self Servis kayıt: Kuruluş ile işbirliği yapıyoruz kullanılıyorsa [Self Servis kayıt](../users-groups-roles/directory-self-service-signup.md) biz kayıtlı e-posta ile parola sıfırlama bildirmek, bir kiracıda oturum alınacağı özellik.
* **B2B kullanıcıları**: Yeni kullanılarak oluşturulan tüm yeni B2B kullanıcıları [Azure AD B2B özellikleri](../active-directory-b2b-what-is-azure-ad-b2b.md) davet etme işlemi sırasında kayıtlı e-posta ile kullanıcıların parolalarını sıfırlamalarına mümkün olacaktır.

Bu senaryoyu test etmek için şuraya gidin: https://passwordreset.microsoftonline.com biri olan bu iş ortağı kullanıcılar. Bir alternatif e-posta veya tanımlanan kimlik doğrulama e-posta oluşturulduysa parola beklendiği gibi çalıştığını sıfırlayın.

> [!NOTE]
> Bu Hotmail.com, Outlook.com veya diğer kişisel e-posta adresleri gibi Azure AD kiracınıza Konuk erişim verilmiş Microsoft hesapları Azure AD SSPR kullanabilmek için değildir. Bulunan bilgileri kullanarak parolalarını sıfırlamak ihtiyaç duydukları [olamaz oturum açtığınızda Microsoft hesabınızı](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler, Azure AD aracılığıyla parola sıfırlama konusunda ek bilgiler sağlar:

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Authentication]: ./media/concept-sspr-howitworks/sspr-authentication-methods.png "Kullanılabilir Azure AD kimlik doğrulama yöntemleri ve gereken miktar"
[Writeback]: ./media/concept-sspr-howitworks/troubleshoot-writeback-running.png "Şirket içi tümleştirme parola geri yazmayı yapılandırma ve sorun giderme bilgileri"
