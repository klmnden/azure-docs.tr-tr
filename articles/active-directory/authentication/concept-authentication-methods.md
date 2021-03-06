---
title: Kimlik doğrulama yöntemleri - Azure Active Directory
description: MFA ve SSPR için Azure AD'de kullanılabilir kimlik doğrulama yöntemleri
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1322c919906dc2d0dd23de538fa2c1992fbe5da0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164838"
---
# <a name="what-are-authentication-methods"></a>Kimlik doğrulama yöntemleri nelerdir?

Azure multi-Factor Authentication ve Self Servis parola sıfırlama (SSPR), birden çok kimlik doğrulama yöntemlerini kaydedin açmasına istemeniz önerilir, yönetici olarak, kimlik doğrulama yöntemlerini seçme. Bir kimlik doğrulama yöntemi, bir kullanıcı için kullanılabilir olmadığı durumlarda, başka bir yöntem ile kimlik doğrulaması seçebilirsiniz.

Yöneticiler kullanıcıların SSPR MFA ve kimlik doğrulama yöntemlerini kullanılabilir ilkesi tanımlayabilirsiniz. Bazı kimlik doğrulama yöntemleri için tüm özellikler kullanılamayabilir. Yapılandırma hakkında daha fazla bilgi için ilkelerinizi makalelerine bakın [Self Servis parola sıfırlama başarıyla sunma](howto-sspr-deployment.md) ve [bulut tabanlı bir Azure multi-Factor Authentication'ı planlama](howto-mfa-getstarted.md)

Microsoft, birine erişiminizin olmadığı durumda birden çok kimlik doğrulama yöntemleri, gerekli en az sayıda seçmek için Yöneticiler etkin kullanıcılar önerir.

|Kimlik Doğrulama Yöntemi|Kullanım|
| --- | --- |
| Parola | MFA ve SSPR |
| Güvenlik soruları | SSPR yalnızca |
| E-posta adresi | SSPR yalnızca |
| Microsoft Authenticator uygulaması | MFA ve SSPR için genel önizlemeye sunuldu |
| OATH donanım belirteci | MFA ve SSPR için genel önizlemeye sunuldu |
| SMS | MFA ve SSPR |
| Sesli çağrı | MFA ve SSPR |
| Uygulama parolaları | Yalnızca belirli durumlarda MFA |

![Oturum açma ekranında kullanılan kimlik doğrulama yöntemleri](media/concept-authentication-methods/overview-login.png)

|     |
| --- |
| MFA ve SSPR ve mobil uygulama bildirimi ya da mobil uygulama kodu için OATH donanım belirteçleri olarak yöntemleri için Azure AD Self Servis parola sıfırlama olan Azure Active Directory genel Önizleme özellikleri. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="password"></a>Parola

Azure AD parolanızı bir kimlik doğrulama yöntemi olarak kabul edilir. Bir yöntem olduğundan, **devre dışı bırakılamaz**.

## <a name="security-questions"></a>Güvenlik soruları

Güvenlik sorularını kullanılabilir **yalnızca Azure AD Self Servis parola sıfırlama** yönetici olmayan hesaplar.

Güvenlik sorularını kullanıyorsanız, bunları başka bir yöntem ile birlikte kullanmanızı öneririz. Bazı kişiler, başka bir kullanıcının soruların yanıtlarını biliyor olabilirsiniz güvenlik sorularını diğer yöntemlerinden daha az güvenli olabilir.

> [!NOTE]
> Güvenlik sorularını dizindeki kullanıcı nesnesinin üzerinde özel ve güvenli bir şekilde depolanır ve yalnızca kullanıcıların kayıt sırasında yanıtlanması gereken. Bir yöneticinin okuyun veya bir kullanıcının soru veya yanıt değiştirmek hiçbir yolu yoktur.
>

### <a name="predefined-questions"></a>Önceden tanımlanmış soruları

* Hangi şehirde, uygulamanızın ilk eşiniz/partneriniz?
* Hangi şehirde çılgın amcanız karşılar?
* Hangi şehirde, en yakın eşdüzey Canlı?
* Hangi şehirde doğdu?
* Hangi şehirde ilk işiniz neydi?
* Anneniz hangi şehirde doğmuş?
* Hangi şehirde size yeni yılda üzerinde olan 2000?
* Lisede sevdiğiniz öğretmeninizin soyadı nedir?
* Başvurduğunuz ancak gitmediğiniz bir okul adı nedir?
* İçinde ilk Evlilik alma tutulan yerin adı nedir?
* Babanızın ikinci adı nedir?
* En sevdiğiniz yemek nedir?
* Annenizin annesinin nedir, adı ve Soyadı?
* Annenizin ikinci adı nedir?
* En büyük kardeşinizin doğduğu ay ve yıl nedir? (örn. Kasım 1985)
* En büyük kardeşinizin ikinci adı nedir?
* Babanızın babasının nedir, adı ve Soyadı?
* En küçük kardeşinizin ikinci adı nedir?
* Hangi okula altıncı sınıfta katıldığını?
* En iyi çocukluk arkadaşınız, adı ve Soyadı neydi?
* İlk partnerinizin adı ve Soyadı neydi?
* En sevdiğiniz ilkokul öğretmeninizin Soyadı neydi?
* Marka ve model ilk arabanızın veya motosikletinizin markası neydi?
* Katıldığınız ilk okulun adı neydi?
* İçinde Doğduğunuz Hastanenin adı neydi?
* Oturduğunuz ilk evin sokak adı neydi?
* Çocukluk kahramanınızın adı neydi?
* En sevdiğiniz doldurulmuş hayvanınızın adı neydi?
* İlk Evcil hayvanınızın adı neydi?
* Çocukluğunuzdaki takma adınız neydi?
* Lisede en sevdiğiniz Spor neydi?
* İlk işiniz neydi?
* Nelerin sizin Çocukluğunuzda kullandığınız telefon numarasının son dört hanesi kaldınız?
* Küçükken, size olmak istediğiniz?
* Tanıştığınız En ünlü kişi kim?

Tüm önceden tanımlı güvenlik soruları çevrilir ve kullanıcının tarayıcı yerel ayarları temel alarak Office 365 dilleri kümesinin içine yerelleştirilmiş.

### <a name="custom-security-questions"></a>Özel güvenlik soruları

Özel güvenlik soruları yerelleştirilmiş değil. Yönetici kullanıcı arabiriminde girerken kullanıcının tarayıcı yerel farklı olsa bile, tüm özel sorular aynı dilde görüntülenir. Yerelleştirilmiş sorular ihtiyacınız varsa, önceden tanımlanmış soruları kullanmanız gerekir.

Bir özel Güvenlik sorusu uzunluğu en fazla 200 karakter olabilir.

### <a name="security-question-requirements"></a>Güvenlik sorusu gereksinimleri

* En az yanıt karakter sınırı, üç karakter olabilir.
* En yüksek yanıt karakter sınırı, 40 karakter değil.
* Kullanıcılar, aynı soruyu bir kereden fazla yanıt veremiyor.
* Kullanıcılar, aynı birden fazla soru yanıtı sağlayamaz.
* Herhangi bir karakter kümesi, soruları ve yanıtları Unicode karakterleri dahil olmak üzere, tanımlamak için kullanılabilir.
* Tanımlanan sorusu kaydetmek için gereken soru sayısına eşit veya daha büyük olmalıdır.

## <a name="email-address"></a>E-posta adresi

E-posta adresi kullanılabilir **yalnızca Azure AD Self Servis parola sıfırlama**.

Microsoft, kullanıcının Azure AD parola erişmeye gerek duymaz bir e-posta hesabının kullanımını önerir.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması

Microsoft Authenticator uygulaması ek bir Azure AD iş veya Okul hesabı veya Microsoft hesabı için güvenlik düzeyi sağlar.

Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) ve [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071)'da kullanılabilir.

> [!NOTE]
> Kullanıcıların Self Servis parola sıfırlama için kaydolurken mobil uygulamasını kaydetme seçeneği yoktur. Bunun yerine, kullanıcıların kendi mobil uygulamamız üzerinden kaydedebilirsiniz [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) veya güvenlik bilgileri kayıt Önizleme'de [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo).
>

### <a name="notification-through-mobile-app"></a>Mobil uygulama üzerinden bildirim

Microsoft Authenticator uygulaması hesaplara yetkisiz erişimi önlemek ve sahte işlemleri akıllı telefonundaki veya tabletindeki bildirim göndererek Durdur yardımcı olabilir. Kullanıcılar bildirimi görüntülemeniz ve işlem meşru, ise doğrulama seçin. Aksi takdirde, bunlar Reddet seçebilirsiniz.

> [!WARNING]
> Self Servis parola sıfırlama için bir yöntem yalnızca zaman sıfırlamak için kullanıcıları için kullanılabilecek tek seçenek doğrulama kodu olan **yüksek düzeyde güvenlik sağlamak için**.
>
> İki yöntem gerekli olduğunda kullanıcıların kullanarak sıfırlayabilir **EITHER** bildirim **veya** doğrulama kodu yanı sıra diğer yöntemleri etkinleştirildi.
>

Kullanımını etkinleştirirseniz, hem bildirim aracılığıyla mobil uygulama ve doğrulama kodu mobil uygulamasından bir bildirim kullanarak Microsoft Authenticator uygulamasını kaydetme kullanıcılar kimliklerini doğrulamak için hem bildirim hem de kodu kullanabilirsiniz.

> [!NOTE]
> Kuruluşunuzda çalışan veya Çin'e seyahat personeli varsa **mobil uygulama üzerinden bildirim** metodunda **Android cihazları** bu ülkede çalışmaz. Alternatif yöntemler söz konusu kullanıcılar için kullanılabilir yapılması gerekir.

### <a name="verification-code-from-mobile-app"></a>Mobil uygulamadan alınan doğrulama kodu

Microsoft Authenticator uygulamasını veya diğer üçüncü taraf uygulamaları bir OATH doğrulama kodu oluşturmak için yazılım belirteci olarak kullanılabilir. Kullanıcı kimliğiniz ve parolanızı girdikten sonra oturum açma ekranına uygulama tarafından sağlanan kodu girin. Doğrulama kodu, ikinci bir form kimlik doğrulaması sağlar.

> [!WARNING]
> Ne zaman sıfırlama doğrulama kodu kullanıcıları için kullanılabilecek tek seçenek için bir yöntem gereklidir yalnızca Self Servis parola sıfırlama için **yüksek düzeyde güvenlik sağlamak için**.
>

Kullanıcılar, en fazla beş OATH donanım belirteçleri veya kimlik doğrulayıcı uygulamalar herhangi bir zamanda kullanılmak üzere yapılandırılmış Microsoft Authenticator uygulaması gibi bir birleşimi olabilir.

## <a name="oath-hardware-tokens-public-preview"></a>OATH donanım belirteçleri (genel Önizleme)

OATH nasıl tek kullanımlık parola (OTP) kodları belirten açık bir standart üretilir. Azure AD 30 saniyelik veya 60 saniye çeşitli OATH-TOTP SHA-1 belirteçleri kullanımını destekler. Müşteriler bu belirteçleri, kendi seçtikleri satıcıdan tedarik. Gizli anahtarları birlikte tüm belirteçlerin uyumlu olmayabilir 128 karakterle sınırlıdır.

![MFA sunucusu OATH belirteçleri dikey penceresine OATH belirteçlerini karşıya yükleme](media/concept-authentication-methods/oath-tokens-azure-ad.png)

Genel Önizleme kapsamında OATH donanım belirteçleri desteklenmektedir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

Belirteçleri elde edilen sonra aşağıda gösterildiği örnek olarak UPN, seri numarası, gizli anahtar, zaman aralığı, üretici ve model dahil olmak üzere bir virgülle ayrılmış değerler (CSV) dosya biçiminde yüklenmelidir.

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,1234567890abcdef1234567890abcdef,60,Contoso,HardwareKey
```

> [!NOTE]
> Yukarıda da gösterildiği gibi CSV dosyasında üst bilgi satırı eklediğinizden emin olun.

Bir CSV dosyası olarak bir yönetici ardından Azure portalında oturum açın ve gidin kez düzgün biçimlendirilmiş **Azure Active Directory**, **MFA sunucusu**, **OATH belirteçleri**, ve Sonuçta elde edilen CSV dosyasını karşıya yükleyin.

CSV dosyasının boyutuna bağlı olarak, bu işlem birkaç dakika sürebilir. Tıklayın **Yenile** geçerli durumunu almak için düğme. Dosyayı herhangi bir hata varsa, hataları çözmek size listeleyen bir CSV dosyasını indirmek için seçeneğine sahip olursunuz.

Hataları giderdikten sonra yönetici ardından her anahtar tıklayarak etkinleştirebilirsiniz **etkinleştirme** etkinleştirilmesi için belirteç ve girmek için OTP belirtecinde güncel olarak görüntülenen.

Kullanıcılar, en fazla beş OATH donanım belirteçleri veya kimlik doğrulayıcı uygulamalar herhangi bir zamanda kullanılmak üzere yapılandırılmış Microsoft Authenticator uygulaması gibi bir birleşimi olabilir.

## <a name="phone-options"></a>Telefon Seçenekleri

### <a name="mobile-phone"></a>Cep telefonu

İki seçenek, kullanıcılar cep telefonları ile kullanılabilir.

Kullanıcıların kendi cep telefonu numarası dizinde görünür olmasını istemediğiniz, ancak bunlar yine de parola sıfırlama için kullanmak istiyorsanız, Yöneticiler bu dizinde doldurmanız gerekir değil. Kullanıcılar doldurmak kendi **kimlik doğrulama telefonu** aracılığıyla özniteliği [parola sıfırlama kayıt portalı](https://aka.ms/ssprsetup). Yöneticiler, kullanıcının profilinde bu bilgiyi görebilir, ancak başka bir yayımlanmaz.

Düzgün çalışması için telefon numaraları biçiminde olmalıdır *+ CountryCode PhoneNumber*, örneğin, + 1 4255551234.

> [!NOTE]
> Orada ülke kodunu ve telefon numarası arasına bir boşluk olması gerekir.
>
> Parola sıfırlama telefon dahili numaralarına desteklemez. Kurulmadan önce bile X + 1 4255551234 12345 biçiminde uzantılar kaldırılır.

#### <a name="text-message"></a>Kısa mesaj

SMS doğrulama kodu içeren bir cep telefonu numarası için gönderilir. Devam etmek için oturum açma arabiriminde sağlanan doğrulama kodunu girin.

#### <a name="phone-call"></a>Telefon araması

Otomatik bir sesli çağrıyla belirttiğiniz telefon numarasına yapılır. Aramayı yanıtlamalı ve telefon tuş kimliğini doğrulamak için # tuşuna basın

> [!IMPORTANT]
> Telefon araması seçenekleri 2019'ın Mart başlangıç MFA ve SSPR kullanıcıları Azure AD ücretsiz/deneme kiracıları için kullanılabilir olmayacak. SMS iletileri, bu değişiklikten etkilenmez. Telefon Araması'nda kullanıcılara kullanılabilir Ücretli Azure AD kiracılarıyla devam eder. Bu değişiklik, yalnızca Azure AD ücretsiz/deneme kiracıları etkiler.

### <a name="office-phone"></a>Ofis telefonu

Otomatik bir sesli çağrıyla belirttiğiniz telefon numarasına yapılır. Aramayı yanıtlamalı ve telefon tuş kimliğini doğrulamak için # tuşuna basar.

Düzgün çalışması için telefon numaraları biçiminde olmalıdır *+ CountryCode PhoneNumber*, örneğin, + 1 4255551234.

Office telefon özniteliğinin yöneticiniz tarafından yönetilir.

> [!IMPORTANT]
> Telefon araması seçenekleri 2019'ın Mart başlangıç MFA ve SSPR kullanıcıları Azure AD ücretsiz/deneme kiracıları için kullanılabilir olmayacak. SMS iletileri, bu değişiklikten etkilenmez. Telefon Araması'nda kullanıcılara kullanılabilir Ücretli Azure AD kiracılarıyla devam eder. Bu değişiklik, yalnızca Azure AD ücretsiz/deneme kiracıları etkiler.

> [!NOTE]
> Orada ülke kodunu ve telefon numarası arasına bir boşluk olması gerekir.
>
> Parola sıfırlama telefon dahili numaralarına desteklemez. Kurulmadan önce bile X + 1 4255551234 12345 biçiminde uzantılar kaldırılır.

### <a name="troubleshooting-phone-options"></a>Sorun giderme telefon seçenekleri

Telefon numarası kullanarak kimlik doğrulama yöntemleri için ilgili sık karşılaşılan sorunlar:

* Tek bir cihazda engellenen arayan kimliği
   * Cihaz sorunlarını giderme
* Yanlış telefon numarası, yanlış ülke kodu, ev telefonu numarasını iş telefon numarası ile karşılaştırması
   * Kullanıcı nesneyle ilgili sorunları giderme ve kimlik doğrulama yöntemleri yapılandırılır. Doğru telefon numaralarını kayıtlı emin olun.
* Yanlış PIN girildi
   * Kullanıcı Azure MFA Sunucusu'nda kayıtlı doğru PIN kullandı onaylayın.
* Sesli mesaja iletilen çağrısı
   * Kullanıcı telefon açık olduğundan ve kendi alanında hizmetin kullanılabilir olduğundan emin olun veya alternatif yöntemini kullanın.
* Kullanıcı engellendi
   * Azure portalında kullanıcının engelini kaldırmak için yönetici vardır.
* Cihazda SMS abone değil
   * Kullanıcı yöntemlerini değiştirmek veya cihazda SMS etkinleştirme sahip.
* Hatalı telekomünikasyon sağlayıcıları (telefon girişi yok algılandı, DTMF tonlarını sorunları, birden fazla cihazda engellenen arayan kimliği eksik veya birden çok cihazda SMS engellendi)
   * Microsoft, telefon görüşmeleri ve kimlik doğrulaması için SMS iletileri yönlendirmek için birden çok telekomünikasyon sağlayıcıları kullanır. Yukarıdaki sorunları görüyorsanız, bir kullanıcı 5 dakika içinde en az 5 kez yöntemini kullanın ve Microsoft Destek ile irtibat kurduğunuzda, kullanıcının bilgilere sahip girişimi sahip.

## <a name="app-passwords"></a>Uygulama parolaları

Belirli tarayıcı olmayan uygulamaları olmayan bir kullanıcı için multi-Factor authentication etkinleştirildiğinde multi-Factor authentication desteği ve tarayıcı olmayan uygulamaları kullanmaya çalışır, bunlar kimliğini doğrulayamıyor. Bir uygulama parolası kullanıcıların kimliğini doğrulamak devam etmesini sağlar.

Koşullu erişim ilkeleri ve değil kullanıcı başına MFA aracılığıyla çok faktörlü kimlik doğrulaması zorunlu kılarsanız, uygulama parolaları oluşturamazsınız. Erişimi denetlemek için koşullu erişim ilkeleri kullanan uygulamalar, uygulama parolaları gerekmez.

Kuruluşunuz SSO Azure AD ile birleştirildiyse ve Azure mfa'yı kullanıyor olacak, sonra aşağıdaki ayrıntılara dikkat edin:

* Uygulama parolası, Azure AD tarafından doğrulanır ve bu nedenle, Federasyon atlar. Federasyon yalnızca uygulama parolaları ayarlanırken kullanılır. Federasyon (SSO) kullanıcılar için parolalar Kurumsal kimlikte depolanır. Kullanıcının şirketten ayrılması durumunda, bu bilgileri DirSync kullanılarak kurumsal Kimliğe için akış gerekir. Hesabı devre dışı bırakma/silme işlemi devre dışı bırakma/silme, uygulama parolaları Azure AD'ye geciktirir eşitleme, üç saate kadar sürebilir.
* Şirket için İstemci Erişimi Denetimi ayarları Uygulama Parolası tarafından onaylanmaz.
* Günlüğe kaydetme ve denetim şirket içi kimlik doğrulama yeteneği, uygulama parolaları için kullanılabilir.
* Burada kimlik doğrulaması ile bağlı istemciler, iki aşamalı doğrulamayı kullanırken Kurumsal kullanıcı adı ve parolaları ve uygulama parolaları oluşan birleşimlerin kullanıldığı bazı gelişmiş Mimari Tasarım gerektirebilir. Bir şirket içi altyapı karşı kimlik doğrulaması istemcileri için bir kuruluş kullanıcı adı ve parola kullanırsınız. Azure AD karşı kimlik doğrulaması istemcileri için uygulama parolasını kullanmanız gerekir.
* Varsayılan olarak, kullanıcılar uygulama parolaları oluşturamaz. Kullanıcıların uygulama parolaları, select oluşturmasına izin vermeniz gerekiyorsa **kullanıcıların tarayıcı olmayan uygulamaları seçeneği ile oturum açmak için uygulama parolaları oluşturmasına izin** hizmet ayarları altında.

## <a name="next-steps"></a>Sonraki adımlar

[Self servis parola sıfırlama kuruluşunuz için etkinleştirme](quickstart-sspr.md)

[Kuruluşunuz için Azure multi-Factor Authentication'ı etkinleştir](howto-mfa-getstarted.md)

[Kiracınızda bulunan toplam kayıt etkinleştirme](howto-registration-mfa-sspr-combined.md)

[Son kullanıcı kimlik doğrulama yöntemini yapılandırma belgeleri](https://aka.ms/securityinfoguide)
