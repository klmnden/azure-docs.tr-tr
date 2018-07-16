---
title: Nasıl çalışır? - Azure Active Directory Self Servis parola sıfırlama
description: Derin Dalış Azure AD Self Servis parola sıfırlama
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 04a446f43bd39ef7bfca590af67289813eab4032
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048888"
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Self Servis parola sıfırlama Azure AD derinlemesine bakış

Self Servis parola, (SSPR) iş nasıl sıfırlansın mı? Bu seçenek arabiriminin ne demektir? Azure Active Directory (Azure AD) SSPR hakkında daha fazla bilgi için okumaya devam edin.

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
2. Kullanıcı, kullanıcı Kimliğini girer ve captcha geçirir.
3. Azure AD, kullanıcının aşağıdaki denetimleri yaparak bu özelliği kullanabilmek için olduğunu doğrular:
   * Kullanıcı bu özelliği etkin olup olmadığını denetler ve Azure AD'yi sahip lisansı atandı.
     * Kullanıcı olmayan bu özelliğin etkinleştirilmesini veya atanmış bir lisansı olması, kullanıcı parolalarını sıfırlamak için yönetici ile iletişime geçin istenir.
   * Kullanıcı hakkına sahip denetimleri hesabını yönetici ilkesine uygun olarak tanımlanan veri meydan okuyun.
     * Ardından, ciddi bir şekilde ilkesi yalnızca bir sınama gerektiriyorsa, kullanıcı için en az bir yönetici İlkesi tarafından etkinleştirilen zorlukların tanımlanan uygun veri sahip olmasını sağlar.
       * Kullanıcı sınaması yapılandırılmamışsa, kullanıcı parolalarını sıfırlamak için kendi yöneticisine başvurmanız önerilir.
     * Ardından, ciddi bir şekilde İlkesi iki aşama gerektiriyorsa, kullanıcı tanımlı en az iki Yönetici İlkesi tarafından etkinleştirilen zorlukların uygun veri sahip olmasını sağlar.
       * Kullanıcı sınaması yapılandırılmamışsa, kullanıcı parolalarını sıfırlamak için kendi yöneticisine başvurmanız önerilir.
   * Kullanıcının parolası olup olmadığını görmek için denetimleri şirket içinde (Federal, geçişli kimlik doğrulaması veya parola karması eşitlenmiş) yönetilir.
     * Geri yazma dağıtılır ve yönetilen şirket kullanıcı parolasının ise, kullanıcının kimliğini doğrulamak ve kullanıcının parolasını sıfırlamak için devam izin verilmez.
     * Kullanıcının parolasını sıfırlamak için yönetici iletişim kurması istenir daha sonra yönetilen şirket içi geri yazma dağıtılmadığı ve kullanıcının parolasını ise.
4. Kullanıcı başarıyla parolalarını sıfırlayabilir olduğunu belirlenirse kullanıcı sıfırlama işlemini destekli.

## <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

SSPR etkinleştirilirse, kimlik doğrulama yöntemleri için aşağıdaki seçeneklerden en az birini seçmeniz gerekir. Bu seçenekler "kapılarını" anılan bazen işittiğiniz Böylece kullanıcılarınızın daha fazla esnekliğe sahip en az iki kimlik doğrulama yöntemi seçin önemle öneririz.

* Email
* Cep telefonu
* Ofis telefonu
* Güvenlik soruları

![Kimlik doğrulaması][Authentication]

### <a name="what-fields-are-used-in-the-directory-for-the-authentication-data"></a>Hangi alanların dizin için kimlik doğrulama verilerini kullanılır?

* **Ofis telefonu**: ofis telefonu için karşılık gelir.
    * Kullanıcılar bu alan kendilerini ayarlayamıyor. Bir yönetici tarafından tanımlanması gerekir.
* **Cep telefonu**: (herkese görünür) kimlik doğrulama telefonu veya cep telefonu (herkese görünür) karşılık gelir.
    * Hizmet için kimlik doğrulama telefonu önce arar ve ardından ise kimlik doğrulama telefonu, cep telefonu geri döner yok.
* **Alternatif e-posta adresi**: kimlik doğrulama e-posta (herkese görünür) ya da alternatif e-posta karşılık gelir.
    * Hizmet kimlik doğrulama e-posta için önce arar ve ardından geri alternatif e-posta için başarısız olur.

Varsayılan olarak, yalnızca bulut öznitelikleri ofis telefonu ve cep telefonu kimlik doğrulama verileri şirket içi dizininizden bulut dizininizle eşitlenir.

Bunlar yönetici etkin olduğundan ve gerektiren kimlik doğrulama yöntemlerini mevcut verileriniz varsa, kullanıcılar yalnızca parolasını sıfırlayabilirsiniz.

Kullanıcıların kendi cep telefonu numarası dizinde görünür olmasını istemediğiniz, ancak bunlar yine de parola sıfırlama için kullanmak istiyorsanız, Yöneticiler bu dizinde doldurmanız gerekir değil. Kullanıcılar sonra doldurmanız, **kimlik doğrulama telefonu** aracılığıyla özniteliği [parola sıfırlama kayıt portalı](https://aka.ms/ssprsetup). Yöneticiler, kullanıcının profilinde bu bilgiyi görebilir, ancak başka bir yayımlanmaz.

### <a name="the-number-of-authentication-methods-required"></a>Gerekli kimlik doğrulama yöntemleri sayısı

Bu seçenek kullanılabilir kimlik doğrulama yöntemleri veya ağ geçitleri bir kullanıcı sıfırlama veya kilidini açma parolasını geçmesi gereken en düşük sayısını belirler. Bir veya iki ayarlanabilir.

Kullanıcılar yöneticinin, kimlik doğrulama yöntemi sağlar. daha fazla kimlik doğrulama yöntemleri sağlamanız seçebilirsiniz.

Bir kullanıcının kayıtlı gerekli en düşük yöntemleri yoksa bunları yönetici parolasını sıfırlama isteğinde yönlendiren bir hata sayfası görürler.

#### <a name="change-authentication-methods"></a>Kimlik doğrulama yöntemleri değiştirme

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
3. Cep telefonu ve alternatif e-posta alanları doldurulmuş olmayan kullanıcıların parolalarını sıfırlayamaz.

### <a name="how-secure-are-my-security-questions"></a>Güvenlik sorularımı ne kadar güvenli?

Güvenlik sorularını kullanıyorsanız, bunları başka bir yöntem ile birlikte kullanmanızı öneririz. Bazı kişiler, başka bir kullanıcının soruların yanıtlarını biliyor olabilirsiniz güvenlik sorularını diğer yöntemlerinden daha az güvenli olabilir.

> [!NOTE] 
> Güvenlik sorularını dizindeki kullanıcı nesnesinin üzerinde özel ve güvenli bir şekilde depolanır ve yalnızca kullanıcıların kayıt sırasında yanıtlanması gereken. Bir yöneticinin okuyun veya bir kullanıcının soru veya yanıt değiştirmek hiçbir yolu yoktur.
>

### <a name="security-question-localization"></a>Güvenlik sorusu yerelleştirme

İzleyin ve önceden tanımlanmış sorulara Office 365 dilleri tam kümesine yerelleştirilmiş olduğunu ve kullanıcının tarayıcı yerel ayarını temel alır:

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
* En büyük kardeşinizin doğduğu ay ve yıl nedir? (örn. Kasım 1985)
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

Özel güvenlik soruları farklı yerel ayarlar için yerelleştirilmiş değil. Yönetici kullanıcı arabiriminde girerken kullanıcının tarayıcı yerel farklı olsa bile, tüm özel sorular aynı dilde görüntülenir. Yerelleştirilmiş sorular ihtiyacınız varsa, önceden tanımlanmış soruları kullanmanız gerekir.

Bir özel Güvenlik sorusu uzunluğu en fazla 200 karakter olabilir.

Parola sıfırlama portalı ve sorular farklı bir yerelleştirilmiş görüntülemek için dil Ekle "? mkt =<Locale>" yerelleştirme İspanyolca için aşağıdaki örnek ile URL sonuna parola sıfırlama [ https://passwordreset.microsoftonline.com/?mkt=es-us ](https://passwordreset.microsoftonline.com/?mkt=es-us).

### <a name="security-question-requirements"></a>Güvenlik sorusu gereksinimleri

* En az yanıt karakter sınırı, üç karakter olabilir.
* En yüksek yanıt karakter sınırı, 40 karakter değil.
* Kullanıcılar, aynı soruyu bir kereden fazla yanıt veremiyor.
* Kullanıcılar, aynı birden fazla soru yanıtı sağlayamaz.
* Herhangi bir karakter kümesi, soruları ve yanıtları Unicode karakterleri dahil olmak üzere, tanımlamak için kullanılabilir.
* Tanımlanan sorusu kaydetmek için gereken soru sayısına eşit veya daha büyük olmalıdır.

## <a name="registration"></a>Kayıt

### <a name="require-users-to-register-when-they-sign-in"></a>Oturum açarken kaydolmalarını iste

Bu seçeneği etkinleştirmek için uygulamaları Azure AD kullanarak oturum, parola sıfırlama kaydı tamamlamak parola sıfırlama için etkinleştirilmiş kullanıcıyı sahiptir. Buna aşağıdakiler dahildir:

* Office 365
* Azure portalına
* Erişim Paneli
* Federasyon uygulamaları
* Azure AD kullanarak özel uygulamalar

Kayıt gerektiren devre dışı bırakıldığında, kullanıcılara kişi bilgileri yine de el ile kaydedebilirsiniz. Bunlar ya da ziyaret edebilirsiniz [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup) veya **parola sıfırlama için kaydolma** altında bağlantı **profili** erişim panelinde sekmesi.

> [!NOTE]
> Kullanıcılar, parola sıfırlama kayıt portalı seçerek kapatabilir **iptal** veya penceresini kapatarak. Ancak kullanıcılar, kayıt işlemini tamamlayana kadar oturum her zaman kaydı istenir.
>
> Bu, zaten oturum açmış kullanıcının bağlantı sonu gelmez.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Kullanıcıların kendi kimlik doğrulama bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısını ayarla

Bu seçenek ayarlama ve kimlik doğrulama bilgilerini reconfirming arasındaki süreyi belirler ve yalnızca etkinleştirirseniz, kullanılabilir **oturum açarken kaydolmalarını iste** seçeneği.

Geçerli değerler 0 ile 730 gün "0" Kullanıcıların kendi kimlik doğrulama bilgilerini yeniden onaylamasını hiçbir zaman istenir anlamına gelir.

## <a name="notifications"></a>Bildirimler

### <a name="notify-users-on-password-resets"></a>Parola sıfırlamayı kullanıcılara bildir

Bu seçenek ayarlanırsa **Evet**, sonra da, parola sıfırlama kullanıcının parolasını değiştirdiğinden emin bildiren bir e-posta alır. E-posta, Azure AD'de dosya olan birincil veya alternatif e-postaları için SSPR portalı gönderilir. Başka hiç kimse bu sıfırlama bildirim olayı.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Diğer yöneticiler parolalarını sıfırladığında tüm yöneticilere bildir

Bu seçenek ayarlanırsa **Evet**, ardından *tüm yöneticilerin* Azure AD'de dosya çubuğunda, birincil e-posta adresine bir e-posta alırsınız. E-posta bunları SSPR'ı kullanarak başka bir yönetici parolasını değiştirdiğinden bildirir.

Örnek: Bir ortamda dört yöneticileri vardır. Bir yönetici, SSPR'ı kullanarak kullanıcının parolasını sıfırlar. B ve C D yöneticilerin, parola sıfırlama uyaran bir e-posta alırsınız.

## <a name="on-premises-integration"></a>Şirket içi tümleştirme

Yükleme, yapılandırma ve Azure AD Connect etkinleştirmek, şirket içi tümleştirmeler aşağıdaki ek seçenekleri sunulmaktadır. Bu seçenekler gri, daha sonra geri yazma düzgün şekilde yapılandırılmadı. Daha fazla bilgi için [parola geri yazmayı yapılandırmayla](howto-sspr-writeback.md#configure-password-writeback).

![Geri yazma][Writeback]

Bu sayfa bir hızlı geçerli yapılandırmanız temelinde şu iletilerden biri görüntülenir şirket içi geri yazma istemcinin durumu sağlar:

* Şirket içi geri yazma istemciniz çalışır durumda.
* Azure AD, çevrimiçi ve şirket içi geri yazma istemcinize bağlı. Ancak, Azure AD Connect'in yüklü sürümü güncel değil gibi görünüyor. Göz önünde bulundurun [Azure AD Connect'i yükseltmeniz](./../connect/active-directory-aadconnect-upgrade-previous-version.md) son bağlantı özelliklerinden ve önemli hata düzeltmelerinden olmasını sağlamak için.
* Ne yazık ki, size Azure AD Connect'in yüklü sürümü güncel olmadığından, şirket içi geri yazma istemcinizin durumunu denetleyemiyor. [Azure AD Connect'e yükseltmenin](./../connect/active-directory-aadconnect-upgrade-previous-version.md) bağlantı durumunuzu denetleyebilmek için.
* Ne yazık ki şirket içi geri yazma istemcinize şu anda bağlanamıyoruz gibi görünüyor. [Azure AD Connect sorun giderme](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) bağlantı geri yüklemek için.
* Ne yazık ki, parola geri yazma düzgün şekilde yapılandırılmadı çünkü şirket içi geri yazma istemcinize bağlanamıyoruz. [Parola geri yazmayı yapılandırın](howto-sspr-writeback.md#configure-password-writeback) bağlantı geri yüklemek için.
* Ne yazık ki şirket içi geri yazma istemcinize şu anda bağlanamıyoruz gibi görünüyor. Bu, bizim son geçici sorunlar nedeniyle olabilir. Sorun devam ederse [sorun giderme Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) bağlantı geri yüklemek için.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Parolalar şirket içi dizininize geri yazmak

Bu denetim, bu dizin için parola geri yazma özelliğinin etkin olup olmadığını belirler. Geri yazma açıksa, şirket içi geri yazma hizmetinin durumunu gösterir. Bu, parola geri yazma'nın Azure AD Connect'i yeniden yapılandırmak zorunda kalmadan geçici olarak devre dışı bırakmak istiyorsanız kullanışlıdır.

* Anahtar ayarlanırsa **Evet**geri yazma sonra etkinleştirilir ve birleştirilmiş, geçişli kimlik doğrulaması veya parola karması eşitlenmiş kullanıcılar parolalarını sıfırlayabilir.
* Anahtar ayarlanırsa **Hayır**sonra geri yazmayı devre dışı bırakıldı ve birleştirilmiş, geçişli kimlik doğrulaması veya parola karması eşitlenmiş kullanıcılar parolalarını sıfırlayabilir değildir.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Kullanıcıların parolalarını sıfırlamadan hesapların kilidini açmak için

Bu denetim seçeneği kullanıcının parolasını sıfırlamak zorunda kalmadan, şirket içi Active Directory hesaplarını kilidini açmak için parola sıfırlama portalını ziyaret eden kullanıcılara verilmesi gerektiğini olup olmadığını belirler. Parola sıfırlama gerçekleştirdiğinde, varsayılan olarak, Azure AD hesaplarının kilidini açar. Bu iki işlemi birbirinden ayırmak için bu ayarı kullanın. 

* Varsa kümesine **Evet**, sonra da kullanıcıların parolalarını sıfırlama ve hesabın kilidini açma veya parolayı sıfırlamak zorunda kalmadan hesaplarının kilidini seçeneği sunulur.
* Varsa kümesine **Hayır**, ardından kullanıcıları olan yalnızca bir birleşik bir parola sıfırlama gerçekleştirebilir ve hesap kilidini açma işlemi.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Parola, iş B2B kullanıcıları için nasıl sıfırlansın mı?
Parola sıfırlama ve değiştirme, tüm işletmeler arası (B2B) yapılandırmaları üzerinde tam olarak desteklenir. B2B kullanıcı parola sıfırlama, aşağıdaki üç durumda desteklenir:

   * **Mevcut bir Azure AD kiracısı ile bir iş ortağı kuruluştan kullanıcılar**: mevcut bir Azure AD kiracısı ile ortaklık kuruluşta varsa biz *her parola sıfırlama ilkeleri, Kiracı üzerinde etkin dikkate*. Çalışmak için parola sıfırlama için iş ortağı kuruluşun yalnızca Azure AD SSPR etkin olduğundan emin olmak gerekir. Office 365 müşterileri için ek ücret yoktur ve adımları izleyerek etkinleştirilebilir bizim [parola yönetimini kullanmaya başlama](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) Kılavuzu.
   * **Aracılığıyla kaydolun kullanıcılar** Self Servis kayıt: kuruluş, kullanılan ile işbirliği yapıyoruz, [Self Servis kayıt](../users-groups-roles/directory-self-service-signup.md) biz kayıtlı e-posta ile parola sıfırlama bildirmek, bir kiracıda oturum alınacağı özellik.
   * **B2B kullanıcıları**: yeni kullanılarak oluşturulan tüm yeni B2B kullanıcıları [Azure AD B2B özellikleri](../active-directory-b2b-what-is-azure-ad-b2b.md) davet etme işlemi sırasında kayıtlı e-posta ile kullanıcıların parolalarını sıfırlamalarına mümkün olacaktır.

Bu senaryoyu test etmek için şuraya gidin: http://passwordreset.microsoftonline.com biri olan bu iş ortağı kullanıcılar. Bir alternatif e-posta veya tanımlanan kimlik doğrulama e-posta oluşturulduysa parola beklendiği gibi çalıştığını sıfırlayın.

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
