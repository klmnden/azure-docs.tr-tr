---
title: Azure AD kimlik doğrulama yöntemleri
description: Azure AD MFA ve SSPR için hangi kimlik doğrulama yöntemleri kullanılabilir
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: 7776ca63dd5c02e470ead35e3dad73c051731fd1
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056264"
---
# <a name="what-are-authentication-methods"></a>Kimlik doğrulama yöntemleri nelerdir?

Azure AD Self Servis parola sıfırlama (SSPR) ve multi-Factor Authentication (MFA), kimin ilişkili özellikleri kullanırken olduğunuzu doğrulamak için kimlik doğrulama yöntemleri veya güvenlik bilgisi olarak bilinen ek bilgi için isteyebilir.

Yöneticiler kullanıcıların SSPR MFA ve kimlik doğrulama yöntemlerini kullanılabilir ilkesi tanımlayabilirsiniz. Bazı kimlik doğrulama yöntemleri için tüm özellikler kullanılamayabilir.

Microsoft, birine erişiminizin olmadığı durumda birden çok kimlik doğrulama yöntemleri, gerekli en az sayıda seçmek için Yöneticiler etkin kullanıcılar önerir.

|Kimlik Doğrulama Yöntemi|Kullanım|
| --- | --- |
| Parola | MFA ve SSPR |
| Güvenlik soruları | SSPR yalnızca |
| E-posta adresi | SSPR yalnızca |
| Microsoft Authenticator uygulaması | MFA ve SSPR için genel önizlemeye sunuldu |
| SMS | MFA ve SSPR |
| Sesli çağrı | MFA ve SSPR |
| Uygulama parolaları | Yalnızca belirli durumlarda MFA |

![Oturum açma ekranında kullanılan kimlik doğrulama yöntemleri](media/concept-authentication-methods/overview-login.png)

|     |
| --- |
| Mobil uygulama bildirimi ve mobil uygulama kodu olarak yöntemleri için Azure AD Self Servis parola sıfırlama, Azure Active Directory genel Önizleme özelliklerinden sunulmuştur. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
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

* Eşiniz/partneriniz ile hangi şehirde tanıştınız?
* Anneniz ile babanız hangi şehirde tanıştı?
* Size en yakın kardeşiniz hangi şehirde yaşıyor?
* Babanız hangi şehirde doğdu?
* İlk işiniz hangi şehirdeydi?
* Anneniz hangi şehirde doğmuş?
* 2000 yılına girdiğimiz yılbaşında hangi şehirdeydiniz?
* Lisede en sevdiğiniz öğretmeninizin soyadı neydi?
* Başvurduğunuz ancak gitmediğiniz üniversitenin adı nedir?
* İlk evlilik davetinizi verdiğiniz yerin adı nedir?
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
* En sevdiğiniz ilkokul öğretmeninizin soyadı neydi?
* İlk arabanızın veya motosikletinizin markası ve modeli neydi?
* Gittiğiniz ilk okulun adı neydi?
* Doğduğunuz hastanenin adı neydi?
* Çocukluğunuzda oturduğunuz ilk evin sokak adı neydi?
* Çocukluk kahramanınızın adı neydi?
* En sevdiğiniz peluş hayvanın adı neydi?
* İlk evcil hayvanınızın adı neydi?
* Çocukluğunuzdaki takma adınız neydi?
* Lisede en sevdiğiniz spor neydi?
* İlk işiniz neydi?
* Çocukluğunuzda kullandığınız telefon numarasının son dört rakamı neydi?
* Küçükken, büyüdüğünüzde ne olmak istiyordunuz?
* Tanıştığınız en ünlü kişi kim?

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

Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) ve [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)'da kullanılabilir.

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

### <a name="verification-code-from-mobile-app"></a>Mobil uygulamadan alınan doğrulama kodu

Microsoft Authenticator uygulamasını veya diğer üçüncü taraf uygulamaları bir OATH doğrulama kodu oluşturmak için yazılım belirteci olarak kullanılabilir. Kullanıcı kimliğiniz ve parolanızı girdikten sonra oturum açma ekranına uygulama tarafından sağlanan kodu girin. Doğrulama kodu, ikinci bir form kimlik doğrulaması sağlar.

> [!WARNING]
> Ne zaman sıfırlama doğrulama kodu kullanıcıları için kullanılabilecek tek seçenek için bir yöntem gereklidir yalnızca Self Servis parola sıfırlama için **yüksek düzeyde güvenlik sağlamak için**.
>

## <a name="mobile-phone"></a>Cep telefonu

İki seçenek, kullanıcılar cep telefonları ile kullanılabilir.

Kullanıcıların kendi cep telefonu numarası dizinde görünür olmasını istemediğiniz, ancak bunlar yine de parola sıfırlama için kullanmak istiyorsanız, Yöneticiler bu dizinde doldurmanız gerekir değil. Kullanıcılar doldurmak kendi **kimlik doğrulama telefonu** aracılığıyla özniteliği [parola sıfırlama kayıt portalı](https://aka.ms/ssprsetup). Yöneticiler, kullanıcının profilinde bu bilgiyi görebilir, ancak başka bir yayımlanmaz.

Düzgün çalışması için telefon numaraları biçiminde olmalıdır *+ CountryCode PhoneNumber*, örneğin, + 1 4255551234.

> [!NOTE]
> Orada ülke kodunu ve telefon numarası arasına bir boşluk olması gerekir.
>
> Parola sıfırlama telefon dahili numaralarına desteklemez. Kurulmadan önce bile X + 1 4255551234 12345 biçiminde uzantılar kaldırılır.

### <a name="text-message"></a>Kısa mesaj

SMS doğrulama kodu içeren bir cep telefonu numarası için gönderilir. Devam etmek için oturum açma arabiriminde sağlanan doğrulama kodunu girin.

### <a name="phone-call"></a>Telefon araması

Otomatik bir sesli çağrıyla belirttiğiniz telefon numarasına yapılır. Aramayı yanıtlamalı ve telefon tuş kimliğini doğrulamak için # tuşuna basın

## <a name="office-phone"></a>Ofis telefonu

Otomatik bir sesli çağrıyla belirttiğiniz telefon numarasına yapılır. Aramayı yanıtlamalı ve telefon tuş kimliğini doğrulamak için # tuşuna basar.

Düzgün çalışması için telefon numaraları biçiminde olmalıdır *+ CountryCode PhoneNumber*, örneğin, + 1 4255551234.

Office telefon özniteliğinin yöneticiniz tarafından yönetilir.

> [!NOTE]
> Orada ülke kodunu ve telefon numarası arasına bir boşluk olması gerekir.
>
> Parola sıfırlama telefon dahili numaralarına desteklemez. Kurulmadan önce bile X + 1 4255551234 12345 biçiminde uzantılar kaldırılır.

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

[Azure multi-Factor Authentication ve Azure AD Self Servis parola sıfırlama için yakınsanmış kaydını etkinleştirin](concept-registration-mfa-sspr-converged.md)

[Son kullanıcı kimlik doğrulama yöntemini yapılandırma belgeleri](https://aka.ms/securityinfoguide)
