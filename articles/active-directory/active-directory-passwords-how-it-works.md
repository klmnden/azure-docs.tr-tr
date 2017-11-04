---
title: "Nasıl çalışır? Azure AD SSPR'yi | Microsoft Docs"
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
ms.openlocfilehash: 71310534ec62b62bcd408d75060859c79bc470cf
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Self Servis parola sıfırlama Azure AD derin Dalış

SSPR nasıl çalışır? Bu seçenek arabiriminde anlamı nedir? Azure AD Self Servis parola hakkında daha fazla bilgi için okumaya devam sıfırlayın.

## <a name="how-does-the-password-reset-portal-work"></a>Parola portalı iş nasıl sıfırlama

Bir kullanıcı gittiğinde parola sıfırlama portalını, bir iş akışı belirlemek için başlayacağı zamana devre dışı:

   * Nasıl sayfa yerelleştirilmiş?
   * Kullanıcı hesabı geçerli mi?
   * Hangi kuruluş için kullanıcının ait?
   * Kullanıcının parolasını yönetildiği?
   * Kullanıcı bu özelliği kullanmak için lisanslanır?


Parola arkasındaki mantığı hakkında bilgi edinmek için aşağıdaki adımları okuma sayfa sıfırlayın.

1. Kullanıcı hesabı bağlantısı veya gelecek doğrudan erişemiyor [https://aka.ms/sspr](https://passwordreset.microsoftonline.com).
2. Deneyimi uygun dili işlenmeden tarayıcı yerel ayar temel. Office 365 desteklediği parola sıfırlama deneyimi aynı dillere yerelleştirilmiştir.
3. Kullanıcı bir kullanıcı kimliği girer ve bir güvenlik kodu geçirir.
4. Azure AD kullanıcı aşağıdaki denetimleri yaparak bu özelliği kullanabilmek için olup olmadığını doğrular:
   * Kullanıcının bu özellik etkinleştirildiğinde ve bir Azure AD lisansı atanmış olup olmadığını denetler.
     * Kullanıcının bu özellik etkinleştirildiğinde veya atanmış bir lisans yoksa, kullanıcı parolasını sıfırlamayı kendi yöneticisine başvurun istenir.
   * Kullanıcı hakkına sahip denetimleri hesaplarında Yönetici İlkesi uygun olarak tanımlanan verileri sınama.
     * İlkesi yalnızca bir sınama gerektiriyorsa, ardından bu kullanıcı için en az bir yönetici İlkesi tarafından etkinleştirilen zorluklar tanımlanan uygun veri olduğunu güvence altına.
       * Kullanıcı yapılandırılmamışsa, kullanıcı parolasını sıfırlamayı kendi yöneticisine başvurmanız önerilir.
     * İlkesi iki zorluk gerektiriyorsa, ardından bunu kullanıcının en az iki Yönetici İlkesi tarafından etkinleştirilen zorluklar için tanımlanan uygun veri olduğunu güvence altına.
       * Kullanıcı yapılandırılmamış sonra biz kullanıcıdır parolasını sıfırlamak için kendi yöneticisine başvurmanız önerilir.
   * Kullanıcının parolasını (Federe veya parola karması eşitlenen) şirket içinde yönetilen olup olmadığını denetler.
     * Daha sonra geri yazma dağıtılır ve kullanıcının parolasını şirket içinde yönetilir, kullanıcı kimliğini ve parolasını sıfırlamak için devam etmesine izin verilir.
     * Geri yazma dağıtılmadığı ve kullanıcının parolasını şirket içinde yönetilir, ardından kullanıcının parolasını sıfırlamak için kendi yöneticisine başvurun istenir.
5. Kullanıcının parolasını başarıyla sıfırlayamazsınız olduğunu belirlenirse kullanıcı sıfırlama işlemi boyunca yönlendirilir.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

Self Servis parola sıfırlama (SSPR) etkinleştirilirse, en az bir kimlik doğrulama yöntemleri için aşağıdaki seçeneklerden birini seçmeniz gerekir. Bazı durumlarda ağ geçitleri başvurulan Bu seçenekler duyabilirsiniz. Böylece, kullanıcılarınızın daha fazla esnekliğe sahip en az iki kimlik doğrulama yöntemi seçme öneririz.

* E-posta
* Cep telefonu
* Ofis telefonu
* Güvenlik Soruları

![Kimlik doğrulaması][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-authentication-data"></a>Hangi alanların dizinde kimlik doğrulama verileri için kullanılır

* Ofis telefonu iş telefonu karşılık gelen
    * Kullanıcılar bu ayarlanamıyor kendilerini onu tanımlanmalıdır bir yönetici tarafından alan
* Cep telefonu kimlik doğrulama telefon (herkese görünür) veya cep telefonu (herkese görünür) karşılık gelir.
    * Hizmet için kimlik doğrulama telefon ilk görünür, ardından cep telefonuna geri döner yoksa
* Kimlik doğrulama e-posta (herkese görünür) veya alternatif e-posta için alternatif e-posta adresi karşılık gelen
    * Hizmet kimlik doğrulama e-posta için önce arar ve ardından geri alternatif e-posta başarısız

Varsayılan olarak, yalnızca bulut öznitelikleri ofis telefonu ve cep telefonu, kimlik doğrulama verileri için şirket içi dizininizden bulut dizininiz için eşitlenir.

Bunlar, yönetici etkin olduğundan ve gerektiren kimlik doğrulama yöntemlerini mevcut verileri varsa kullanıcılar yalnızca parolalarını sıfırlayabilir.

Kullanıcıların cep telefonu numaralarına dizinde görünür olmasını istiyor musunuz ancak hala parola sıfırlama için kullanmak istediğiniz yöneticileri değil doldurmak bu dizinde ve kullanıcı ardından doldurur kendi **kimlik doğrulama telefon** aracılığıyla özniteliği [parola sıfırlama kayıt portalı](http://aka.ms/ssprsetup). Yöneticiler kullanıcı profili için bu bilgiyi görebilir, ancak başka bir yerde yayınlanmadı.

### <a name="number-of-authentication-methods-required"></a>Gereken kimlik doğrulama yöntemleri sayısı

Bu seçenek kullanılabilen kimlik doğrulama yöntemleri veya bir kullanıcı sıfırlama veya parolalarını kilidini açmak için geçtikleri gerekir ve 1 veya 2'ye ayarlanabilir geçitleri minimum sayısını belirler.

Kullanıcılar, yönetici tarafından etkinleştirilmişse daha fazla kimlik doğrulama yöntemleri sağlamak seçebilirsiniz.

Bir kullanıcıya kayıtlı gerekli en düşük yöntemleri yoksa, parolasını sıfırlamak için yönetici istemek için yönlendirmez bir hata sayfası görürler.

### <a name="how-secure-are-my-security-questions"></a>My güvenlik soruları ne kadar güvenli mi

Güvenlik soruları kullanırsanız, bazı kişiler başka bir kullanıcının soruların yanıtlarını bildiğiniz beri bunlar diğer yöntemlerinden daha az güvenli olabilir olarak bunları kullanımda başka bir yöntemle öneririz.

> [!NOTE] 
> Güvenlik soruları dizininde bir kullanıcı nesnesi üzerinde özel olarak ve güvenli bir şekilde depolanır ve kullanıcılar tarafından kayıt sırasında yalnızca yanıtlanması. Okuma veya kullanıcının soru veya yanıt değiştirmek bir yöneticinin bir yolu yoktur.
>

### <a name="security-question-localization"></a>Güvenlik sorusu yerelleştirme

İzleyen tüm önceden tanımlanmış sorular kullanıcının tarayıcı bölgesel ayarına göre Office 365 diller, tamamını içine yerelleştirilmiştir.

* İlk eşiniz/partneriniz ile hangi şehirde tanıştınız?
* Anneniz ile babanız hangi şehirde tanıştı?
* Size en yakın kardeşiniz hangi şehirde yaşıyor?
* Babanız hangi şehirde doğdu?
* İlk işiniz hangi şehirdeydi?
* Anneniz hangi şehirde doğdu?
* 2000 yılına girdiğimiz yılbaşında hangi şehirdeydiniz?
* Yüksek içinde en sevdiğiniz öğretmenin soyadı nedir * Okul?
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

Özel güvenlik soruları farklı yerel ayarlar için yerelleştirilmiş değil. Tüm özel sorular kullanıcının tarayıcı yerel farklı olsa bile yönetici kullanıcı arabiriminde girildiği dilde görüntülenir. Yerelleştirilmiş soruları gerekiyorsa, lütfen önceden tanımlanmış soruları kullanın.

Bir özel Güvenlik sorusu uzunluğu en fazla 200 karakter olabilir.

### <a name="security-question-requirements"></a>Güvenlik sorusu gereksinimleri

* En az yanıt karakter sınırı 3 karakterdir
* En büyük yanıt karakter sınırı 40 karakterdir
* Kullanıcılar, aynı soruyu birden fazla kez yanıt vermeyebilir
* Kullanıcıların birden fazla soru aynı yanıt sağlamayabilir
* Herhangi bir karakter kümesi sorular ve yanıtlar Unicode karakterler içeren tanımlamak için kullanılabilir
* Tanımlanmış soru sayısını kaydetmek için gereken soru sayısına eşit veya daha büyük olmalıdır

## <a name="registration"></a>Kayıt

### <a name="require-users-to-register-when-signing-in"></a>Kullanıcılardan oturum açarken kaydolmalarını iste

Bu seçeneğin etkinleştirilmesi izleyen olanlar gibi oturum açmak için Azure AD kullanan uygulamalar için oturum açma, parola tamamlamak amacıyla parola sıfırlama için etkin bir kullanıcı sıfırlama kaydı gerektirir:

* Office 365
* Azure portalına
* Erişim Paneli
* Federasyon uygulamalarına
* Azure AD kullanarak özel uygulamalar

Bu devre dışı bırakıldığında kullanıcılar yine de el ile kişi bilgileri ziyaret ederek kaydolabilir [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) veya tıklatarak **parola sıfırlama için kaydetme** profili sekmesi altındaki bağlantı erişim paneli.

> [!NOTE]
> Kullanıcıları parola sıfırlama kayıt Portalı'nı İptal'i tıklatarak veya pencereyi sayabilirsiniz ancak kayıt tamamlanana kadar kullanıcılar oturum açma her zaman istenir.
>

### <a name="number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısı

Bu seçenek ayarlama ve kimlik doğrulama bilgilerini reconfirming arasındaki süreyi belirler ve yalnızca etkinleştirirseniz, kullanılabilir **oturum açarken kullanıcıların** seçeneği.

Geçerli değerler: 0-730 hiçbir zaman kullanıcıların kendi kimlik bilgilerini yeniden onayladıktan ister yani 0 gün

## <a name="notifications"></a>Bildirimler

### <a name="notify-users-on-password-resets"></a>Parola sıfırlamayı kullanıcılara bildir

Bu seçenek Evet olarak ayarlarsanız, parola sıfırlama kullanıcı parolalarını SSPR portal üzerinden birincil ve alternatif e-posta adreslerini dosyada Azure AD içinde değiştirildi olduğunu bildiren bir e-posta alır. Hiç kimse bu sıfırlanmasını bildirilir olay.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Tüm Yöneticiler diğer yöneticilerin kullanıcıların parolalarını sıfırladığınızda bildir

Bu seçenek Evet olarak ayarlarsanız, ardından **tüm yöneticiler** başka bir yönetici SSPR kullanarak parolalarını değiştiğini bildiren Azure AD'de dosya, birincil e-posta adresine bir e-posta alırsınız.

Örnek: Bir ortamda dört Yöneticiler vardır. Yönetici "A" SSPR kullanarak kendi parolasını sıfırlar. Yöneticiler B, C ve D bunları bunun uyarı bir e-posta alır.

## <a name="on-premises-integration"></a>Şirket içi tümleştirme

Yükledikten, yapılandırılmış ve Azure AD Connect etkin değilse, şirket içi tümleştirmeler için aşağıdaki ek seçenekler gerekir.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Şirket içi dizininize parolaları geri Yaz

Parola geri yazma bu dizin için etkin denetler ve geri yazma açıksa, şirket içi geri yazma hizmetinin durumunu gösterir. Bu parola geri yazma'nın Azure AD Connect yapılandırmadan geçici olarak devre dışı bırakmak istiyorsanız kullanışlıdır.

* Anahtar Evet olarak ayarlarsanız, sonra geri yazma, Federasyon ve etkin ve parola karma eşitlenmiş kullanıcıların parolalarını sıfırlayabilmesi verebilir.
* Anahtarı ayarlanırsa Hayır'a, ardından geri yazma devre dışı, Federasyon ve ve parola karma eşitlenmiş kullanıcıları olan değil parolalarını sıfırlayamazsınız.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Kullanıcıların parolalarını sıfırlamadan hesaplarının kilidini izin ver

Parola sıfırlama portalı ziyaret eden kullanıcılar, şirket içi Active Directory hesaplarını, parola sıfırlama olmadan kilidini açmak için seçeneği verilmelidir olup olmadığını belirler. Varsayılan olarak, Azure AD parola sıfırlama gerçekleştirirken hesaplarının kilidini açar, bu ayar, bu iki işlem ayırmanıza olanak sağlar. 

* "Evet" olarak ayarlayın, sonra kullanıcılar seçeneği parolalarını sıfırlama ve hesabın kilidini ya da parola sıfırlama olmadan kilidini açmak için verilir durumunda.
* Kullanıcılar yalnızca sonra mümkün "Hayır" olarak ayarlayın birleşik bir parola gerçekleştirmek için sıfırlama ve hesabın kilidini açma işlemi varsa.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Parola iş B2B kullanıcıları için nasıl sıfırlama?
Parola sıfırlama ve değişiklik tüm B2B yapılandırmaları ile tam olarak desteklenir. Aşağıdaki üç durumda B2B kullanıcı parola sıfırlama için desteklenir.

1. **Kullanıcıların mevcut Azure AD kiracısı ile partner org'dan** - mevcut bir Azure AD kiracısı ile ortaklık kuruluş varsa, biz **Kiracı içinde ne olursa olsun parola sıfırlama ilkelerinin etkinleştirildiğinden dikkate**. Parola O365 müşteriler için ek ücret ödemeden olan Azure AD SSPR'yi etkinleştirildiğinden emin olmak için iş ortağı kuruluş yalnızca gereksinimlerini çalışmaya sıfırlamak ve içindeki adımları izleyerek etkinleştirilebilir bizim [parola yönetimiileçalışmayabaşlama](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords)Kılavuzu.
2. **Kaydolan kullanarak kullanıcıların [Self Servis kaydolma](active-directory-self-service-signup.md)**  - kuruluş, kullanılan ile ortaklık [Self Servis kaydolma](active-directory-self-service-signup.md) Kiracı alınacağı özellik, size bildirmek ile Sıfırla Kayıtlı e-posta.
3. **B2B kullanıcılar** -yeni kullanılarak oluşturulan tüm yeni B2B kullanıcılar [Azure AD B2B yetenekleri](active-directory-b2b-what-is-azure-ad-b2b.md) parolalarını davet işlemi sırasında kayıtlı e-posta ile mümkün olacaktır.

Bu senaryoyu test etmek için bu iş ortağı kullanıcılar biriyle http://passwordreset.microsoftonline.com gidin. Bir alternatif e-posta veya tanımlanan kimlik doğrulama e-posta sahip oldukları sürece, parola beklendiği gibi çalıştığını sıfırlayın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantılar, Azure AD kullanarak parola sıfırlama ile ilgili ek bilgiler sağlar

* [SSPR başarılı bir sunum nasıl tamamlamak?](active-directory-passwords-best-practices.md)
* [Sıfırlama veya parolanızı değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self Servis parola sıfırlama için kaydetme](active-directory-passwords-reset-register.md).
* [Bir lisans soru var mı?](active-directory-passwords-licensing.md)
* [Hangi verilerin SSPR tarafından kullanılır ve hangi verilerin, kullanıcılarınız için doldurmanız gerekir?](active-directory-passwords-data.md)
* [Hangi kimlik doğrulama yöntemlerinin kullanıcıların var mı?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile ilkesi seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden t hakkında önemli?](active-directory-passwords-writeback.md)
* [SSPR etkinliğinde üzerinde nasıl rapor edebilirim?](active-directory-passwords-reporting.md)
* [Tüm SSPR seçeneklerinde nedir ve ne anlama geldiklerini?](active-directory-passwords-how-it-works.md)
* [Bir şey bozuk düşünüyorum. SSPR nasıl sorun giderme?](active-directory-passwords-troubleshoot.md)
* [Herhangi bir yerde else kapsanmayan bir soru sahip](active-directory-passwords-faq.md)

[Authentication]: ./media/active-directory-passwords-how-it-works/sspr-authentication-methods.png "Azure AD kimlik doğrulama yöntemleri ve gerekli miktar"