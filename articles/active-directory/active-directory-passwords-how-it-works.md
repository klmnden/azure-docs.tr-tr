---
title: "Azure AD SSPR'yi nasıl çalışır | Microsoft Docs"
description: "Derin Dalış Azure AD Self Servis parola sıfırlama"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 19518ad8dc2d697f1716750adc3f0ad7d7f8a875
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Self Servis parola sıfırlama Azure AD derin Dalış

Self Servis parola (SSPR) iş nasıl sıfırlama? Bu seçenek arabiriminde anlamı nedir? Azure Active Directory (Azure AD) SSPR hakkında daha fazla bilgi için okumaya devam edin.

## <a name="how-does-the-password-reset-portal-work"></a>Parola portalı iş nasıl sıfırlama?

Bir kullanıcı için çıktığında parola sıfırlama portalını, bir iş akışı belirlemek için başlayacağı zamana devre dışı:

   * Nasıl sayfa yerelleştirilmiş?
   * Kullanıcı hesabı geçerli mi?
   * Hangi kuruluş için kullanıcının ait?
   * Kullanıcının parolasını yönetildiği?
   * Kullanıcı bu özelliği kullanmak için lisanslanır?

Parola arkasındaki mantığı hakkında bilgi edinmek için aşağıdaki adımları okuma sıfırlama sayfasına:

1. Kullanıcının seçtiği **hesabınıza erişemiyor** doğrudan giden veya bağlantı [https://aka.ms/sspr](https://passwordreset.microsoftonline.com).
   * Tarayıcı bölgesel ayarına göre deneyimi uygun dili işlenir. Parola sıfırlama deneyimi Office 365 destekleyen aynı dile yerelleştirilmiştir.
2. Kullanıcı bir kullanıcı kimliği girer ve bir güvenlik kodu geçirir.
3. Azure AD, kullanıcının aşağıdaki denetimleri yaparak bu özelliği kullanabilmek için olduğunu doğrular:
   * Kullanıcı bu özelliği etkin olup olmadığını denetler ve Azure AD sahip lisansı atanmış.
     * Kullanıcı Bu özellik etkinleştirildiğinde ya da içermiyor atanmış bir lisansa sahip, kullanıcının parolasını sıfırlamak için kendi yöneticisine başvurun istenir.
   * Kullanıcı hakkına sahip denetimleri hesaplarında Yönetici İlkesi uygun olarak tanımlanan verileri sınama.
     * Ardından bu ciddi bir şekilde ilkesi yalnızca bir sınama gerektiriyorsa, kullanıcı için en az bir yönetici İlkesi tarafından etkinleştirilen zorluklar tanımlanan uygun veri sahip olmasını sağlar.
       * Kullanıcı sınaması yapılandırılmamışsa, kullanıcı parolasını sıfırlamayı kendi yöneticisine başvurmanız önerilir.
     * Ardından bu ciddi bir şekilde İlkesi iki zorluk gerektiriyorsa, kullanıcı için en az iki Yönetici İlkesi tarafından etkinleştirilen zorluklar tanımlanan uygun veri sahip olmasını sağlar.
       * Kullanıcı sınaması yapılandırılmamışsa, kullanıcı parolasını sıfırlamayı kendi yöneticisine başvurmanız önerilir.
   * Kullanıcının parolası olup olmadığını görmek için denetimleri (Federe veya parola karması eşitlenen) şirket içi yönetilen.
     * Daha sonra geri yazma dağıtılır ve kullanıcının parolasını yönetilen şirket içi ise, kullanıcı kimliğini ve parolasını sıfırlamak için devam etmesine izin verilir.
     * Geri yazma dağıtılmadığı ve kullanıcının parolasını yönetilen şirket içi ise, ardından kullanıcının parolasını sıfırlamak için kendi yöneticisine başvurun istenir.
4. Kullanıcının parolasını başarıyla sıfırlayamazsınız olduğunu belirlenirse kullanıcı sıfırlama işlemi boyunca yönlendirilir.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

SSPR etkinleştirilirse, en az bir kimlik doğrulama yöntemleri için aşağıdaki seçeneklerden birini seçmeniz gerekir. Bu seçenekler için "geçitleri" adlandırılan bazen duyduğunuz Kullanıcılarınızın daha fazla esneklik en az iki kimlik doğrulama yöntemi seçmeniz önerilir.

* E-posta
* Cep telefonu
* Ofis telefonu
* Güvenlik soruları

![Kimlik doğrulaması][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-the-authentication-data"></a>Hangi alanların dizinde için kimlik doğrulama verileri kullanılır?

* **Ofis telefonu**: ofis telefonu karşılık gelir.
    * Kullanıcılar bu alan kendilerini ayarlanamıyor. Bir yönetici tarafından tanımlanması gerekir.
* **Cep telefonu**: (herkese görünür) kimlik doğrulama telefon veya cep telefonu (herkese görünür) karşılık gelen.
    * Hizmeti için kimlik doğrulama telefon ilk arar ve ardından kimlik doğrulama telefon ise cep telefonu geri döner yok.
* **Alternatif e-posta adresi**: kimlik doğrulama e-posta (herkese görünür) veya alternatif e-posta karşılık gelir.
    * Hizmet kimlik doğrulama e-posta için önce arar ve geri alternatif e-posta için başarısız olur.

Varsayılan olarak, yalnızca bulut öznitelikleri ofis telefonu ve cep telefonu kimlik doğrulama verileri için şirket içi dizininizden bulut dizininiz için eşitlenir.

Bunlar, yönetici etkin olduğundan ve gerektiren kimlik doğrulama yöntemlerini mevcut verileri varsa kullanıcılar yalnızca parolalarını sıfırlayabilir.

Kullanıcıların cep telefonu numaralarına dizinde görünür olmasını istemiyorsanız, ancak bunlar hala parola sıfırlama için kullanmak istediğiniz, Yöneticiler bu dizinde doldurmanız gerekir değil. Kullanıcıların sonra doldurmak kendi **kimlik doğrulama telefon** aracılığıyla özniteliği [parola sıfırlama kayıt portalı](http://aka.ms/ssprsetup). Yöneticiler kullanıcı profili için bu bilgiyi görebilir, ancak başka bir yerde yayınlanmadı.

### <a name="the-number-of-authentication-methods-required"></a>Gereken kimlik doğrulama yöntemleri sayısı

Bu seçenek kullanılabilen kimlik doğrulama yöntemleri veya bir kullanıcı sıfırlama veya kilidini açma parolasını gidip aracılığıyla ağ geçitleri minimum sayısını belirler. Bir veya iki için ayarlanabilir.

Kullanıcılar, yönetici bu kimlik doğrulama yöntemini etkinleştirirse daha fazla kimlik doğrulama yöntemleri sağlamak seçebilirsiniz.

Bir kullanıcıya kayıtlı gerekli en düşük yöntemleri yoksa yönlendirmez yönetici parolasını sıfırlama isteği için bir hata sayfası görürler.

#### <a name="change-authentication-methods"></a>Kimlik doğrulama yöntemlerini değiştirme

Sıfırlama veya kilidini yalnızca bir gerekli kimlik doğrulama yöntemini sahip bir ilke ile başlatırsanız, kayıtlı ve iki yöntem için ne olacağını değiştirme?

| Kayıtlı yöntemleri sayısı | Gerekli yöntemleri sayısı | Sonuç |
| :---: | :---: | :---: |
| 1 veya daha fazla | 1 | **Mümkün** sıfırlamak veya kilidini açmak için |
| 1 | 2 | **%S** sıfırlamak veya kilidini açmak için |
| 2 veya daha fazla | 2 | **Mümkün** sıfırlamak veya kilidini açmak için |

Bir kullanıcının kullandığı kimlik doğrulama yöntemlerini türlerini değiştirirseniz, kullanılabilir veri miktarına yoksa SSPR kullanabilmek için kullanıcıların yanlışlıkla durabilir.

Örnek: 
1. Özgün ilke gereken iki kimlik doğrulama yöntemleri ile yapılandırılır. Yalnızca ofis telefon numarası ve güvenlik sorularını kullanır. 
2. Yönetici güvenlik sorularını artık kullanılacak ilkesini değiştirir, ancak bir cep telefonu ve alternatif bir e-posta kullanılmasına izin verir.
3. Alternatif e-posta alanları doldurulur ve cep telefonu olmayan kullanıcıların parolalarını sıfırlayamazsınız.

### <a name="how-secure-are-my-security-questions"></a>My güvenlik soruları ne kadar güvenli mi?

Güvenlik soruları kullanırsanız, bunları başka bir yöntem ile birlikte kullanmanızı öneririz. Bazı kişiler başka bir kullanıcının soruların yanıtlarını biliyor olabilirsiniz için güvenlik soruları diğer yöntemlerinden daha az güvenli olabilir.

> [!NOTE] 
> Güvenlik soruları dizininde bir kullanıcı nesnesi üzerinde özel olarak ve güvenli bir şekilde depolanır ve kullanıcılar tarafından kayıt sırasında yalnızca yanıtlanması. Okuma veya kullanıcının soru veya yanıt değiştirmek bir yöneticinin bir yolu yoktur.
>

### <a name="security-question-localization"></a>Güvenlik sorusu yerelleştirme

İzleyen tüm önceden tanımlanmış sorular Office 365 dilleri tam kümesine yerelleştirilmiş ve kullanıcının tarayıcı bölgesel ayarına göre:

* İlk eşiniz/partneriniz ile hangi şehirde tanıştınız?
* Anneniz ile babanız hangi şehirde tanıştı?
* Size en yakın kardeşiniz hangi şehirde yaşıyor?
* Babanız hangi şehirde doğdu?
* İlk işiniz hangi şehirdeydi?
* Anneniz hangi şehirde doğdu?
* 2000 yılına girdiğimiz yılbaşında hangi şehirdeydiniz?
* Lisede en sevdiğiniz öğretmenin soyadı nedir?
* Başvurduğunuz ancak gitmediğiniz üniversitenin adı nedir?
* İlk düğününüzü gerçekleştirdiğiniz yerin adı nedir?
* Babanızın ikinci adı nedir?
* En sevdiğiniz yemek nedir?
* Anneannenizin adı ve soyadı nedir?
* Annenizin ikinci adı nedir?
* En büyük kardeşinizin Doğum günü ay ve yıl nedir? (örn. Kasım 1985)
* En büyük kardeşinizin ikinci adı nedir?
* Dedenizin adı ve soyadı nedir?
* En küçük kardeşinizin ikinci adı nedir?
* Altıncı sınıfta hangi okula gidiyordunuz?
* Çocukken en iyi arkadaşınızın adı ve soyadı neydi?
* İlk partnerinizin adı ve soyadı neydi?
* İlkokulda en sevdiğiniz öğretmenin soyadı neydi?
* İlk arabanızın veya motosikletinizin markası ve modeli neydi?
* Gittiğiniz ilk okulun adı neydi?
* Doğduğunuz hastanenin adı neydi?
* Çocukluğunuzun geçtiği ilk evin bulunduğu sokağın adı neydi?
* Çocukluk kahramanınızın adı neydi?
* En sevdiğiniz peluş hayvanın adı neydi?
* İlk evcil hayvanınızın adı neydi?
* Çocukken lakabınız neydi?
* Lisedeyken en sevdiğiniz spor hangisiydi?
* İlk işiniz neydi?
* Çocukken kullandığınız telefon numaranızın son dört rakamı neydi?
* Küçükken büyüdüğünüzde ne olmak istiyordunuz?
* Tanıştığınız en ünlü kişi kim?

### <a name="custom-security-questions"></a>Özel güvenlik soruları

Özel güvenlik soruları farklı yerel ayarlar için yerelleştirilmiş değil. Yönetici kullanıcı arabiriminde girerken kullanıcının tarayıcı yerel farklı olsa bile, tüm özel soruları aynı dilde görüntülenir. Yerelleştirilmiş soruları gerekiyorsa, önceden tanımlanmış soruları kullanmanız gerekir.

Bir özel Güvenlik sorusu uzunluğu en fazla 200 karakter olabilir.

### <a name="security-question-requirements"></a>Güvenlik sorusu gereksinimleri

* En az yanıt karakter sınırı üç karakterdir.
* En büyük yanıt karakter sınırı 40 karakter olabilir.
* Kullanıcılar, aynı soruyu birden fazla kez yanıt veremiyor.
* Kullanıcılar, birden fazla soru aynı yanıt sağlayamaz.
* Herhangi bir karakter kümesi sorular ve yanıtlar Unicode karakterleri dahil olmak üzere, tanımlamak için kullanılabilir.
* Tanımlanmış soru sayısını kaydetmek için gereken soru sayısına eşit veya daha büyük olmalıdır.

## <a name="registration"></a>Kayıt

### <a name="require-users-to-register-when-they-sign-in"></a>Kullanıcılar oturum açtığında kullanıcıların

Bu seçeneği etkinleştirmek için bunlar uygulamaları için Azure AD kullanarak oturum açarsanız parola sıfırlama kayıt işlemini tamamlamak parola sıfırlama için etkin bir kullanıcı vardır. Buna aşağıdakiler dahildir:

* Office 365
* Azure portalına
* Erişim Paneli
* Federasyon uygulamalarına
* Azure AD kullanarak özel uygulamalar

Kayıt gerektiren devre dışı bırakıldığında, kullanıcılar yine de el ile kişi bilgileri kaydedebilirsiniz. Ya da ziyaret edebilirsiniz [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) veya seçin **parola sıfırlama için kaydetme** altında bağlantı **profil** erişim paneli sekmesindedir.

> [!NOTE]
> Kullanıcılar kapatmak parola sıfırlama kayıt Portalı'nı seçerek **iptal** veya penceresini kapatarak. Ancak, kayıt işlemi tamamlanana kadar oturum her zaman kaydı istenir.
>
> Bunlar zaten açtıysanız bu kullanıcının bağlantıyı kesin değildir.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Kullanıcıların kendi kimlik bilgilerini yeniden onayladıktan istenir önce geçmesi gereken gün sayısını ayarlayın

Bu seçenek ayarlama ve kimlik doğrulama bilgilerini reconfirming arasındaki süreyi belirler ve yalnızca etkinleştirirseniz, kullanılabilir **oturum açarken kullanıcıların** seçeneği.

Geçerli değerler 0 ile 730 gün "0" Kullanıcıların hiçbir zaman kendi kimlik bilgilerini yeniden onayladıktan istenir anlamına gelir.

## <a name="notifications"></a>Bildirimler

### <a name="notify-users-on-password-resets"></a>Parola sıfırlamayı kullanıcılara bildir

Bu seçenek belirlenirse, **Evet**, sonra da, parola sıfırlama kullanıcı parolalarını değiştirildi bildiren bir e-posta alır. E-posta SSPR portalı Azure AD içinde dosyada olan birincil ve alternatif e-postaları gönderilir. Hiç kimse bu sıfırlanmasını bildirilir olay.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Tüm Yöneticiler diğer yöneticilerin kullanıcıların parolalarını sıfırladığınızda bildir

Bu seçenek belirlenirse, **Evet**, ardından *tüm yöneticiler* Azure AD içinde dosya kendi birincil e-posta adresine bir e-posta alırsınız. E-posta bunları SSPR kullanarak başka bir yönetici parolasını değişmiş olduğunu bildirir.

Örnek: Bir ortamda dört Yöneticiler vardır. Yönetici bir SSPR kullanarak kendi parolasını sıfırlar. Yöneticiler B, C ve D bunları parola sıfırlama uyarıları bir e-posta alır.

## <a name="on-premises-integration"></a>Şirket içi tümleştirme

Yükleme, yapılandırma ve Azure AD Connect etkinleştirirseniz, şirket içi tümleştirmeler için aşağıdaki ek seçeneğiniz vardır. Bu seçenekler griyse sonra geri yazma düzgün yapılandırılmamış. Daha fazla bilgi için bkz: [parola geri yazma yapılandırma](active-directory-passwords-writeback.md#configuring-password-writeback).

![Geri yazma][Writeback]

Bu sayfa bir hızlı geçerli yapılandırmasını temel alarak aşağıdaki iletilerinden biri görüntülenir şirket içi geri yazma istemcinin durumu sağlar:

* Şirket içi geri yazma istemciniz çalışır durumda.
* Azure AD çevrimiçi olduğundan ve şirket içi geri yazma istemciniz bağlanır. Ancak, Azure AD Connect yüklü olan sürümü güncel değil gibi görünüyor. Göz önünde bulundurun [yükseltme Azure AD Connect](./connect/active-directory-aadconnect-upgrade-previous-version.md) önemli hata düzeltmeleri ve en son bağlantı özellikleri olmasını sağlamak için.
* Ne yazık ki, Azure AD Connect yüklü olan sürümü güncel olduğundan biz şirket içi geri yazma istemci durumunuzu denetlenemiyor. [Azure AD Connect yükseltme](./connect/active-directory-aadconnect-upgrade-previous-version.md) bağlantı durumunuzu denetlemek için.
* Ne yazık ki, şirket içi geri yazma istemciniz şu an bağlanamıyoruz gibi görünüyor. [Azure AD Connect sorun giderme](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) bağlantı geri yüklemek için.
* Ne yazık ki, parola geri yazma düzgün yapılandırılmamış olduğundan, şirket içi geri yazma istemciye bağlanamıyoruz. [Parola geri yazma yapılandırma](active-directory-passwords-writeback.md#configuring-password-writeback) bağlantı geri yüklemek için.
* Ne yazık ki, şirket içi geri yazma istemciniz şu an bağlanamıyoruz gibi görünüyor. Bu bizim uçtaki geçici sorunlardan dolayı olabilir. Sorun devam ederse [sorun giderme Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) bağlantı geri yüklemek için.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Şirket içi dizininize parolaları geri Yaz

Bu denetim, parola geri yazma bu dizin için etkin olup olmadığını belirler. Geri yazma açıksa, şirket içi geri yazma hizmetinin durumunu gösterir. Bu parola geri yazma'nın Azure AD Connect'i yeniden yapılandırmak zorunda kalmadan geçici olarak devre dışı bırakmak istiyorsanız kullanışlıdır.

* Anahtar ayarlanmışsa **Evet**, daha sonra geri yazma etkinleştirilmiş ve Federasyon ve parola karma eşitlenen kullanıcılar parolalarını sıfırlayamazsınız.
* Anahtar ayarlanmışsa **Hayır**, daha sonra geri yazma özelliğini devre dışı ve Federasyon ve parola karma eşitlenen kullanıcılar parolalarını sıfırlama mümkün değildir.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Kullanıcıların parolalarını sıfırlamadan hesaplarının kilidini izin ver

Bu denetim, parola sıfırlama portalı ziyaret eden kullanıcılar parolalarını sıfırlamasına gerek kalmadan şirket içi Active Directory hesaplarının kilidini açmak için seçeneği verilmelidir olup olmadığını belirler. Parola sıfırlama gerçekleştirdiğinde, varsayılan olarak, Azure AD hesaplarının kilidini açar. Bu iki işlem ayırmak için bu ayarı kullanın. 

* Varsa kümesine **Evet**, sonra da kullanıcıların parolalarını sıfırlama ve hesabın kilidini açma veya parola sıfırlama gerek kalmadan hesaplarının kilidini seçeneği sunulur.
* Varsa kümesine **Hayır**, ardından kullanıcıları olan yalnızca bir birleşik parola sıfırlama gerçekleştirebilir ve hesap kilidini açma işlemi.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Parola iş B2B kullanıcıları için nasıl sıfırlama?
Parola sıfırlama ve değişiklik tüm işletmeden işletmeye (B2B) yapılandırmaları üzerinde tam olarak desteklenir. B2B kullanıcı parola sıfırlama aşağıdaki üç durumda desteklenir:

   * **Mevcut bir Azure AD Kiracı İş ortağı kuruluşla kullanıcılardan**: var olan bir Azure AD kiracısı ile ortaklık kuruluş varsa, biz *bu Kiracı'ne olursa olsun parola sıfırlama ilkelerinin etkinleştirildiğinden dikkate*. İş amacıyla parola sıfırlama için iş ortağı kuruluşun yalnızca Azure AD SSPR'yi etkin olduğundan emin olun gerekir. Office 365 müşterileri için ek ücret yoktur ve içindeki adımları izleyerek etkinleştirilebilir bizim [parola yönetimini kullanmaya başlama](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) Kılavuzu.
   * **Üzerinden oturum açan kullanıcıların** Self Servis kaydolma: kuruluş, kullanılan ile ortaklık varsa [Self Servis kaydolma](active-directory-self-service-signup.md) Kiracı alınacağı özellik, biz kayıtlı e-posta ile parola sıfırlama olanak sağlayacak.
   * **B2B kullanıcılar**: yeni kullanılarak oluşturulan tüm yeni B2B kullanıcılar [Azure AD B2B yetenekleri](active-directory-b2b-what-is-azure-ad-b2b.md) parolalarını davet işlemi sırasında kayıtlı e-posta ile mümkün olacaktır.

Bu senaryoyu test etmek için bu iş ortağı kullanıcılar biriyle http://passwordreset.microsoftonline.com gidin. Bunlar bir alternatif e-posta veya tanımlanan kimlik doğrulama e-posta varsa, parola beklendiği gibi çalışır sıfırlayın.

> [!NOTE]
> Konuk erişimi olanlar Hotmail.com, Outlook.com ya da diğer kişisel e-posta adresleri gibi Azure AD kiracınız için verilmiş Microsoft hesapları Azure AD SSPR'yi kullanmanız mümkün değildir. İçinde bulunan bilgileri kullanarak parolalarını sıfırlamak ihtiyaç duydukları [olamaz oturum açtığınızda Microsoft hesabınızı](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler parola sıfırlama Azure AD ile ilgili ek bilgiler sağlar:

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md)
* [Lisans soru var mı?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](active-directory-passwords-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Authentication]: ./media/active-directory-passwords-how-it-works/sspr-authentication-methods.png "Kullanılabilir Azure AD kimlik doğrulama yöntemleri ve gereken miktar"
[Writeback]: ./media/active-directory-passwords-how-it-works/troubleshoot-writeback-running.png "Tümleştirme parola geri yazma yapılandırmasını ve sorun giderme bilgileri şirket içi"
