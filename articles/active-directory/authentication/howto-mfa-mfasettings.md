---
title: Azure çok faktörlü kimlik doğrulama - Azure Active Directory'yi yapılandırma
description: Bu makalede Azure portalında Azure multi-Factor Authentication ayarlarını yapılandırma
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f1dbd4b6635d615cc7bed4cf5cc38234ec0c3f1
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58886004"
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Azure multi-Factor Authentication ayarlarını yapılandırma

Bu makale, Azure portalında çok faktörlü kimlik doğrulaması ayarlarını yönetmek için yardımcı olur. En iyi Azure multi-Factor Authentication dışında almaya yardımcı olan çeşitli konuları kapsar. Tüm özellikler kullanılabilir her [Azure multi-Factor Authentication sürümü](concept-mfa-whichversion.md#what-features-do-i-need).

Multi-Factor Authentication göz atarak Azure portalından ilgili ayarlarına erişebilirsiniz **Azure Active Directory** > **MFA**.

![Azure portal - Azure AD çok faktörlü kimlik doğrulama ayarları](./media/howto-mfa-mfasettings/multi-factor-authentication-settings-portal.png)

## <a name="settings"></a>Ayarlar

Bu ayarlardan bazıları, MFA sunucusu, Azure mfa'yı veya her ikisi de uygulanır.

| Özellik | Açıklama |
| ------- | ----------- |
| Hesap kilitleme | Geçici olarak kilitleme hesaplar çok faktörlü kimlik doğrulaması hizmeti varsa çok fazla satırda kimlik doğrulama girişimlerini reddedildi. Bu özellik, yalnızca kimlik doğrulaması için PIN giren kullanıcılar için geçerlidir. (MFA sunucusu) |
| [Kullanıcı engelle/engelini kaldır](#block-and-unblock-users) | MFA sunucusu (şirket içi) belirli kullanıcılar multi-Factor Authentication istekleri almak mümkün olmasını engellemek için kullanılır. Engellenen kullanıcılar için kimlik doğrulama girişimleri otomatik olarak reddedilir. Kullanıcı engellendikten andan itibaren 90 gün boyunca engellenmiş kalır. |
| [Sahtekarlık uyarısı](#fraud-alert) | Doğrulama istekleri raporlamak için kullanıcıların özelliğiyle ilgili ayarları yapılandırın |
| Bildirimler | MFA sunucusu olay bildirimlerini etkinleştirin. |
| [OATH belirteçleri](concept-authentication-methods.md#oath-hardware-tokens-public-preview) | Bulut tabanlı Azure mfa'yı ortamlarında, kullanıcıların OATH belirteçlerini yönetmek için kullanılır. |
| [Telefon görüşmesi ayarları](#phone-call-settings) | Telefon aramaları ve Bulut ve şirket içi ortamlar için tebrikler ilgili ayarları yapılandırın. |
| Sağlayıcılar | Hesabınızla ilişkili bu var olan tüm kimlik doğrulama sağlayıcılarını gösterir. 1 Eylül 2018'den itibaren yeni kimlik doğrulama sağlayıcıları oluşturulmayabilir |

## <a name="manage-mfa-server"></a>MFA Sunucusunu yönet

Bu bölümde MFA sunucusu için yalnızca ayarlarıdır.

| Özellik | Açıklama |
| ------- | ----------- |
| Sunucu ayarları | MFA Sunucusu'nu indirme ve ortamınızı başlatmak için etkinleştirme kimlik bilgileri oluştur |
| [Bir kerelik geçiş](#one-time-bypass) | Sınırlı bir süre için iki aşamalı doğrulama gerçekleştirmeden kimlik doğrulama açmasına izin verin. |
| [Önbelleğe alma kuralları](#caching-rules) |  Önbelleğe alma, öncelikli olarak VPN gibi şirket içi sistemler, ilk isteği hala devam ederken birden fazla doğrulama isteği gönderdiğinizde kullanılır. Bu özellik, kullanıcı ilk doğrulama devam ediyor başarılı olduktan sonra otomatik olarak başarılı olması sonraki istekleri sağlar. |
| Sunucu durumu | Sürüm, durum, IP ve son iletişim saat ve tarih dahil olmak üzere şirket içi MFA sunucularınızın durumunu görürsünüz. |

## <a name="activity-report"></a>Etkinlik raporu

Burada kullanılabilen raporlama, MFA sunucusu (şirket içi) için özeldir. Azure MFA (bulut) için Azure AD'de oturum açma işlemleri raporu rapor bakın.

## <a name="block-and-unblock-users"></a>Engelleme ve kullanıcıların engelini kaldırma

Kullanım _engelleme ve kullanıcıların engelini kaldırma_ kullanıcılar kimlik doğrulama isteklerini almasını önlemek için özellik. Engellenen kullanıcılar için kimlik doğrulama girişimleri otomatik olarak reddedilir. Kullanıcı engellendikten andan itibaren 90 gün boyunca engellenmiş kalır.

### <a name="block-a-user"></a>Kullanıcıları engelle

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gözat **Azure Active Directory** > **MFA** > **kullanıcıları engelle/engelini kaldır**.
3. Seçin **Ekle** bir kullanıcıyı engellemek için.
4. Seçin **çoğaltma grubu**. Engellenen bir kullanıcı olarak için kullanıcı adını girin **kullanıcıadı\@etkialanı.com**. Bir yorum girin **neden** alan.
5. Seçin **Ekle** engelleyen kullanıcı tamamlanması.

### <a name="unblock-a-user"></a>Kullanıcıların engelini kaldır

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gözat **Azure Active Directory** > **MFA** > **kullanıcıları engelle/engelini kaldır**.
3. Seçin **Engellemeyi Kaldır** içinde **eylem** Engellemesi kaldırılacak kullanıcının yanındaki sütuna.
4. Bir yorum girin **neden engellemesini kaldırmak için** alan.
5. Seçin **Engellemeyi Kaldır** kullanıcının engellemesini kaldırma tamamlanması.

## <a name="fraud-alert"></a>Sahtekarlık uyarısı

Yapılandırma _sahtekarlık Uyarısı_ böylece kullanıcılarınız, kullanıcıların kaynaklara erişmeye sahte çalışır bildirebilirsiniz özelliği. Kullanıcıların sahtekarlık denemeleri mobil uygulamasını kullanarak ya da telefon numaraları bildirebilirsiniz.

### <a name="turn-on-fraud-alerts"></a>Dolandırıcılık uyarılarını Aç

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gözat **Azure Active Directory** > **MFA** > **sahtekarlık Uyarısı**.
3. Ayarlama **kullanıcıların sahtekarlık uyarısı göndermesine izin ver** ayarını **üzerinde**.
4. **Kaydet**’i seçin.

### <a name="configuration-options"></a>Yapılandırma seçenekleri

* **Sahtekarlık bildirildiğinde kullanıcıyı engelle**: Bir kullanıcı sahtekarlık bildirirse, kendi hesabı 90 gün veya yönetici hesabını engellemesinin kaldırıldığı kadar engellenir. Yönetici oturum açma raporunu kullanarak oturum açma işlemleri gözden geçirin ve gelecekteki sahtekarlığı önlemek, uygun eylemi gerçekleştirin. Yönetici böylece [engellemesini](#unblock-a-user) kullanıcının hesabı.
* **İlk Karşılama sırasında sahtekarlık bildirme kodu**: Kullanıcılar, iki aşamalı doğrulamayı gerçekleştirmek için bir telefon araması aldığında, bunlar normal basın **#** kullanıcıların oturum açma onaylamak için. Rapor sahtekarlık bir kod tuşlarına basarak önce kullanıcının girdiği **#**. Bu kodu **0** varsayılan olarak, ancak bunu özelleştirebilirsiniz.

   >[!NOTE]
   >Microsoft gelen varsayılan sesli karşılamalar basın seçmemelerini **0#** sahtekarlık uyarısı göndermek için. Bir kod dışında kullanmak istiyorsanız **0**, kaydedin ve kullanıcılarınız için uygun yönergelere ile kendi özel sesli karşılamalar karşıya yükleyin.
   >

### <a name="view-fraud-reports"></a>Sahtekarlık raporlarını görüntüle

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **Azure Active Directory** > **oturum açma**. Sahtekarlık rapor artık standart Azure AD oturum açma işlemleri raporu bir parçasıdır.

## <a name="phone-call-settings"></a>Telefon görüşmesi ayarları

### <a name="caller-id"></a>Arayan kimliği

**MFA arayan kimlik numarası** -Bu, sayı, kullanıcılarınızın telefonlarında da görürsünüz. Yalnızca ABD tabanlı sayı izin verilir.

>[!NOTE]
>Bazen genel telefon ağ üzerinden multi-Factor Authentication aramalarını yerleştirildiğinde, bunlar arayan kimliği desteği olmayan bir taşıyıcı yönlendirilir Çok faktörlü kimlik doğrulama sistemi her zaman gönderdiği olsa bile bu nedenle, arayan kimliği, garanti edilmez.

### <a name="custom-voice-messages"></a>Özel sesli mesajları

İki aşamalı doğrulamayla için kendi kayıt veya greetings kullanabilirsiniz _özel sesli mesajları_ özelliği. Bu iletiler, ek olarak veya Microsoft kayıtları değiştirmek için kullanılabilir.

Başlamadan önce aşağıdaki kısıtlamalara dikkat edin:

* Desteklenen dosya biçimleri .wav ve .mp3 ' dir.
* Dosya boyutu sınırı 5 MB şeklindedir.
* Kimlik doğrulama iletilerinde 20 saniyeden daha kısa olmalıdır. 20 saniyeden daha uzun bir iletiyi doğrulama başarısız olmasına neden olabilir. Kullanıcının ileti tamamlanır ve doğrulama zaman aşımına uğramadan önce yanıt vermeyebilir.

### <a name="custom-message-language-behavior"></a>Özel ileti dil davranışı

Kullanıcıya özel sesli mesajı yürütüldüğünde, ileti dilini şu etkenlere bağlıdır:

* Geçerli kullanıcının dili.
  * Kullanıcının tarayıcı tarafından algılanan dili.
  * Diğer kimlik doğrulama senaryoları farklı şekilde davranabilir.
* Kullanılabilir hiçbir özel iletiler dili.
  * Özel bir ileti eklendiğinde, bu dil Yöneticisi tarafından seçilir.

Örneğin, biri Almanca dil içeren tek bir özel ileti ise:

* Almanca dilinde kimliğini doğrulayan bir kullanıcı özel Almanca ileti duyacaktır.
* İngilizce dilinde kimliğini doğrulayan bir kullanıcıyı standart İngilizce ileti duyacaktır.

### <a name="set-up-a-custom-message"></a>Özel bir iletiyi ayarlayın

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
1. Gözat **Azure Active Directory** > **MFA** > **telefon araması ayarları**.
1. Seçin **Ekle Karşılama**.
1. Karşılama türü seçin.
1. Dili seçin.
1. Karşıya yüklenecek bir .mp3 veya .wav ses dosyasını seçin.
1. **Add (Ekle)** seçeneğini belirleyin.

## <a name="one-time-bypass"></a>Bir kerelik geçiş

_Bir kerelik atlama_ iki aşamalı doğrulama gerçekleştirmeden bir kereliğine kimlik doğrulaması bir kullanıcı özelliği sağlar. Geçiş geçicidir ve belirtilen sayıda saniye geçtikten sonra süresi dolar. Burada mobil uygulaması ya da telefon bir bildirim ya da telefon aramasına almıyor durumlarda, kullanıcının istenen kaynağa erişebilmek için bir kerelik geçiş izin verebilirsiniz.

### <a name="create-a-one-time-bypass"></a>Bir kerelik geçiş oluşturmak

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gözat **Azure Active Directory** > **MFA** > **bir kerelik atlama**.
3. **Add (Ekle)** seçeneğini belirleyin.
4. Gerekirse, çoğaltma grubu için geçiş'i seçin.
5. Kullanıcı adı olarak girin **kullanıcıadı\@etkialanı.com**. Atlama sürmelidir saniye sayısını girin. Atlama nedeni girin.
6. **Add (Ekle)** seçeneğini belirleyin. Zaman sınırı hemen uygulanmaya başlanır. Kullanıcının bir kerelik atlama süresi dolmadan önce oturum açmanız gerekir.

### <a name="view-the-one-time-bypass-report"></a>Bir kerelik atlama raporunu görüntüle

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Gözat **Azure Active Directory** > **MFA** > **bir kerelik atlama**.

## <a name="caching-rules"></a>Önbelleğe alma kuralları

Bir kullanıcı kullanarak kimlik doğrulaması yapıldıktan sonra kimlik doğrulama girişimlerini izin vermek için bir zaman dönemi ayarlayabilirsiniz _önbelleğe alma_ özelliği. Belirtilen kullanıcı için sonraki kimlik doğrulama girişimleri otomatik olarak döneme başarılı. Önbelleğe alma, öncelikli olarak VPN gibi şirket içi sistemler, ilk isteği hala devam ederken birden fazla doğrulama isteği gönderdiğinizde kullanılır. Bu özellik, kullanıcı ilk doğrulama devam ediyor başarılı olduktan sonra otomatik olarak başarılı olması sonraki istekleri sağlar.

>[!NOTE]
>Önbelleğe alma özelliği, Azure Active Directory (Azure AD) için oturum açma için kullanılmak üzere tasarlanmamıştır.

### <a name="set-up-caching"></a>Önbelleğe alma ayarlayın

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. Gözat **Azure Active Directory** > **MFA** > **önbelleğe alma kuralları**.
3. **Add (Ekle)** seçeneğini belirleyin.
4. Seçin **önbellek türü** aşağı açılan listeden. En fazla sayısını girin **önbelleğe saniye**.
5. Gerekirse, kimlik doğrulama türünü seçin ve bir uygulama belirtin.
6. **Add (Ekle)** seçeneğini belirleyin.

## <a name="mfa-service-settings"></a>MFA hizmet ayarları

Uygulama parolaları için ayarları, güvenilen IP'ler, doğrulama seçenekleri ve Azure multi-Factor Authentication için multi-Factor authentication hizmeti ayarlarında bulunabilir unutmayın. Hizmet ayarları erişilebilir Azure portalından göz atarak **Azure Active Directory** > **MFA** > **Başlarken**  >  **Yapılandırma** > **ek bulut tabanlı MFA ayarları**.

![Azure multi-Factor Authentication hizmeti ayarları](./media/howto-mfa-mfasettings/multi-factor-authentication-settings-service-settings.png)

## <a name="app-passwords"></a>Uygulama parolaları

Bazı uygulamalar, Office 2010 veya önceki ve iOS 11, önce Apple Mail iki aşamalı doğrulamayı desteklemez. Uygulamalar, ikinci bir doğrulama kabul edecek şekilde yapılandırılmamışlardır. Bu uygulamaları kullanmak için yararlanmak _uygulama parolaları_ özelliği. İki aşamalı doğrulamayı atlayabilir ve çalışmaya devam etmek için uygulama izin vermek için geleneksel parolanız yerine uygulama parolasını kullanın.

Modern kimlik doğrulaması için Microsoft Office 2013 istemcilerindeki ve sonraki sürümlerde desteklenir. Outlook gibi office 2013 modern kimlik doğrulama protokolleri destekleyin ve iki aşamalı doğrulamayla çalışması için etkinleştirilebilir. Uygulama parolaları, istemci etkinleştirildikten sonra istemci için gerekli değildir.

>[!NOTE]
>Uygulama parolaları dayalı koşullu erişimi çok faktörlü kimlik doğrulama ilkeleri ve modern kimlik doğrulaması ile çalışmaz.

### <a name="considerations-about-app-passwords"></a>Uygulama parolaları hakkında dikkat edilecek noktalar

Uygulama parolaları kullanılırken aşağıdaki önemli noktaları göz önünde bulundurun:

* Uygulama parolaları, yalnızca uygulama başına bir kez girilir. Kullanıcıların parolalarını izlemek veya bunları her zaman girmeniz gerekmez.
* Gerçek parola otomatik olarak oluşturulur ve kullanıcı tarafından sağlanmaz. Otomatik olarak oluşturulan parolanın bir saldırgan tarafından tahmin edilmesi ve daha güvenlidir.
* Kullanıcı başına 40 parolalık bir sınır yoktur.
* Parolaları önbelleğe almak ve bunları şirket içi senaryolarda kullanan uygulamalar, uygulama parolasını dışında iş veya Okul hesabı bilinen değilse başarısız başlayabilirsiniz. Bu senaryoya örnek olarak şirket içi Exchange e-postalar, ancak arşivlenen postanın bulutta. Bu senaryoda, aynı parola çalışmaz.
* Multi-Factor Authentication kullanıcının hesabında etkinleştirildikten sonra uygulama parolaları en tarayıcı olmayan istemciler gibi Outlook ve Microsoft Skype ile iş için kullanılabilir. Windows PowerShell gibi tarayıcı olmayan uygulamaları ile uygulama parolaları kullanarak yönetim eylemlerini gerçekleştirilemiyor. Kullanıcı bir yönetici hesabı olsa bile eylem gerçekleştirilemiyor. PowerShell betikleri çalıştırmak için güçlü bir parolası olan bir hizmet hesabı oluşturun ve hesabı için iki aşamalı doğrulamayı etkinleştirme.

>[!WARNING]
>Uygulama parolaları burada istemciler hem şirket içi ile iletişim kurar ve uç noktaları bulut otomatik olarak bulmak Karma ortamlarda çalışmaz. Etki alanı parolalarını şirket içi kimlik doğrulaması için gereklidir. Uygulama parolaları, bulut kimlik doğrulaması için gereklidir.

### <a name="guidance-for-app-password-names"></a>Uygulama parolası adlarının Kılavuzu

Uygulama parolası adlarının üzerinde kullanıldıklarından cihaz yansıtmalıdır. Outlook, Word ve Excel gibi tarayıcı olmayan uygulamalar içeren dizüstü bilgisayarınız varsa, adlı bir uygulama parolası oluşturma **dizüstü** bu uygulamalar için. Adlı başka bir uygulama parolası oluşturmanız **Masaüstü** , masaüstü bilgisayarınızda aynı uygulamaları için.

>[!NOTE]
>Uygulama başına bir uygulama parolası yerine, cihaz başına bir uygulama parolası oluşturmanız önerilir.

### <a name="federated-or-single-sign-on-app-passwords"></a>Federe veya çoklu oturum açma uygulama parolaları

Azure AD, şirket içi Windows Server Active Directory etki alanı Hizmetleri ile (AD DS) veya Federasyon çoklu oturum açma (SSO) destekler. Kuruluşunuz Azure AD ile birleştirildiyse ve Azure multi-Factor Authentication kullanıyorsanız, uygulama parolaları hakkında aşağıdaki noktaları göz önünde bulundurun.

>[!NOTE]
>Aşağıdaki noktaları yalnızca federasyon (SSO) müşteriler için geçerlidir.

* Uygulama parolaları Azure AD tarafından doğrulanır ve bu nedenle Federasyonu atlar. Federasyon, etkin bir şekilde uygulama parolaları yalnızca ayarlanırken kullanılır.
* Kimlik sağlayıcısı (IDP) pasif akışı aksine Federasyon (SSO) kullanıcıları kurulmadı. İş veya Okul hesabı uygulama parolaları depolanır. Bir kullanıcı şirketten ayrıldığında, kullanıcının bilgileri için iş veya Okul hesabı kullanarak akışları **DirSync** gerçek zamanlı olarak. Hesabı devre dışı bırakma/silme, Azure AD'de uygulama parolasını devre dışı bırakma/silme geciktirip eşitlemek için üç saat sürebilir.
* Şirket içi istemci erişim denetimi ayarları uygulama parolaları özelliği tarafından kabul değildir.
* Günlüğe kaydetme/denetim özelliği şirket içi kimlik doğrulaması ile uygulama parolaları özelliğini kullanmak için kullanılabilir.
* Bazı gelişmiş mimarileri, istemcilerle iki aşamalı doğrulama için kimlik bilgilerini bir birleşimini gerektirir. Bu kimlik bilgileri, bir iş veya Okul hesabı kullanıcı adı ve parolaları ve uygulama parolaları içerebilir. Kimlik doğrulamasının nasıl yapıldığı gereksinimleri bağlıdır. Bir şirket içi altyapı karşı kimlik doğrulaması istemcileri için bir iş veya Okul hesabı kullanıcı adı ve parola gerekli. Azure AD karşı kimlik doğrulaması istemcileri için bir uygulama parolası gereklidir.

  Örneğin, aşağıdaki mimari olduğunu varsayalım:

  * Bir şirket içi Active Directory örneğini Azure AD ile birleştirildiyse.
  * Exchange online kullanıyorsanız.
  * Skype Kurumsal şirket içi için kullanıyorsunuz.
  * Azure çok faktörlü kimlik doğrulaması kullanıyorsunuz.

  Bu senaryoda, aşağıdaki kimlik bilgilerini kullanın:

  * Skype Kurumsal'a oturum açmak için iş veya Okul hesabı kullanıcı adı ve parola kullanın.
  * Adres Defteri'ni çevrimiçi Exchange'e bağlayan bir Outlook istemcisinden erişmek için bir uygulama parolasını kullanın.

### <a name="allow-users-to-create-app-passwords"></a>Kullanıcıların uygulama parolaları oluşturmasına izin ver

Varsayılan olarak, kullanıcılar uygulama parolaları oluşturamaz. Uygulama parolaları özelliği etkinleştirilmelidir. Kullanıcıların uygulama parolaları oluşturma olanağı vermek için aşağıdaki yordamı kullanın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**.
4. Multi-Factor Authentication'ın altında seçin **hizmet ayarları**.
5. Üzerinde **hizmet ayarları** sayfasında **kullanıcıların tarayıcı olmayan uygulamalara oturum açmak için uygulama parolaları oluşturmasına izin** seçeneği.

### <a name="create-app-passwords"></a>Uygulama parolaları oluşturma

Kullanıcıların uygulama parolaları, ilk kayıt sırasında oluşturabilirsiniz. Kullanıcının kayıt işleminin sonunda uygulama parolaları oluşturma seçeneği vardır.

Kullanıcılar, uygulama parolaları kayıt sonrasında de oluşturabilirsiniz. Daha fazla bilgi ve kullanıcılarınız için ayrıntılı adımlar için bkz. [Azure multi-Factor Authentication, uygulama parolaları nedir?](../user-help/multi-factor-authentication-end-user-app-passwords.md)

## <a name="trusted-ips"></a>Güvenilen IP'ler

_Güvenilen IP'ler_ özelliği, Azure multi-Factor Authentication, yönetilen veya Federasyon Kiracı yöneticileri tarafından kullanılır. Bu özellik şirket intranetten oturum kullanıcılar için iki aşamalı doğrulamayı atlar. Bu özellik, Azure multi-Factor Authentication'ın tam sürümünü ve Yöneticiler için değil ücretsiz sürümü ile kullanılabilir. Azure multi-Factor Authentication'ın tam sürümünü alma hakkında daha fazla ayrıntı için bkz. [Azure multi-Factor Authentication](multi-factor-authentication.md).

> [!NOTE]
> Yalnızca IPv4 adresleriyle iş IP'ler ve koşullu erişim adlandırılmış konumlar MFA güvenilen.

Kuruluşunuz için şirket içi uygulamaları Not MFA NPS uzantısı dağıtır, kaynak IP adresini her zaman kimlik doğrulama denemesi akar NPS sunucusu olarak görünür.

| Azure AD Kiracı türü | Güvenilen IP'ler özellik seçenekleri |
|:--- |:--- |
| Yönetilen |**Belirli IP adresleri**: Yöneticiler, kullanıcılar şirket intranetten oturum açmak için iki aşamalı doğrulamayı atlayabilir bir IP adresi aralığı belirtin.|
| Federasyon |**Tüm Federasyon kullanıcıları**: Kuruluşunuzun içinde oturum tüm Federasyon kullanıcıları, iki aşamalı doğrulamayı atlayabilir. Kullanıcıların, Active Directory Federasyon Hizmetleri (AD FS) tarafından verilen bir talep kullanarak doğrulamayı atlama.<br/>**Belirli IP adresleri**: Yöneticiler, kullanıcılar şirket intranetten oturum açmak için iki aşamalı doğrulamayı atlayabilir bir IP adresi aralığı belirtin. |

Güvenilen IP'ler, şirket intraneti içinde yalnızca çalıştığını atlama. Seçerseniz **tüm Federasyon kullanıcıları** seçeneği ve bir kullanıcı oturum açtığında gelen şirket intranet dışından, kullanıcının iki aşamalı doğrulama kullanarak kimlik doğrulaması gerekir. Kullanıcının AD FS talep sunsa bile işlem aynıdır. 

### <a name="end-user-experience-inside-of-corpnet"></a>Corpnet içinde son kullanıcı deneyimi

Güvenilen IP'ler özelliği devre dışı bırakıldığında, iki aşamalı doğrulama tarayıcı akışlar için gereklidir. Uygulama parolaları, eski zengin istemci uygulamaları için gereklidir.

Güvenilen IP'ler özelliği etkinleştirilmişse, bu iki aşamalı doğrulama, *değil* tarayıcı akışlar için gereklidir. Uygulama parolaları tarafından *değil* koşuluyla kullanıcının bir uygulama parolası oluşturmadı eski zengin istemci uygulamaları için gerekli. Bir uygulama parolası kullanımda sonra parolayı gerekli olarak kalır. 

### <a name="end-user-experience-outside-corpnet"></a>Son kullanıcı deneyimi corpnet dışında

Güvenilen IP'ler özellik etkinleştirilip etkinleştirilmediği bağımsız olarak, iki aşamalı doğrulama tarayıcı akışlar için gerekli değildir. Uygulama parolaları, eski zengin istemci uygulamaları için gereklidir.

### <a name="enable-named-locations-by-using-conditional-access"></a>Koşullu erişim kullanarak adlandırılmış konumlar etkinleştir

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **koşullu erişim** > **adlandırılmış Konumlar**.
3. Seçin **yeni konuma**.
4. Konum için bir ad girin.
5. Seçin **güvenilen konum olarak işaretle**.
6. CIDR gösteriminde gibi IP aralığını girin **192.168.1.1/24**.
7. **Oluştur**’u seçin.

### <a name="enable-the-trusted-ips-feature-by-using-conditional-access"></a>Koşullu erişim kullanarak güvenilen IP'ler özelliğini etkinleştir

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **koşullu erişim** > **adlandırılmış Konumlar**.
3. Seçin **MFA güvenilen Ip'lerini Yapılandır**.
4. Üzerinde **hizmet ayarları** sayfasındaki **güvenilen IP'ler**, aşağıdaki iki seçenekten birini seçin:

   * **Benim intranet bağlantısı intranetimden gelen Federasyon kullanıcılarının istekleri için**: Bu seçeneği için onay kutusunu işaretleyin. Tüm kullanıcılar, Kurumsal ağda oturum açma AD FS tarafından verilen bir talep kullanarak iki aşamalı doğrulamayı atlama sürümüyle federasyona eklenenler. AD FS uygun trafiği intranet talep eklemek için bir kuralı olduğundan emin olun. Kural yoksa, aşağıdaki kural AD FS'de oluşturun:

      `c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);`

   * **İstekleri belirli bir aralıktan genel IP'lerin**: Bu seçeneği belirlemek için metin kutusuna CIDR gösterimini kullanarak IP adreslerini girin.
      * Aralık xxx.xxx.xxx.1 xxx.xxx.xxx.254 aracılığıyla bulunan IP adresleri için gibi gösterimi kullanın **xxx.xxx.xxx.0/24**.
      * Tek bir IP adresi gibi gösterimi kullanın **xxx.xxx.xxx.xxx/32**.
      * 50 adede kadar IP adresi aralıklarını girin. Bu IP adreslerinden oturum açma kullanıcıların iki aşamalı doğrulamayı atlama.

5. **Kaydet**’i seçin.

### <a name="enable-the-trusted-ips-feature-by-using-service-settings"></a>Güvenilen IP'ler özelliği hizmet ayarlarını kullanarak etkinleştirin.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**.
4. Multi-Factor Authentication'ın altında seçin **hizmet ayarları**.
5. Üzerinde **hizmet ayarları** sayfasındaki **güvenilen IP'ler**, aşağıdaki iki seçenekten birini (veya her ikisi de) seçin:

   * **İntranetimde bulunan Federasyon kullanıcılardan gelen istekleri için**: Bu seçeneği için onay kutusunu işaretleyin. Tüm kullanıcılar, Kurumsal ağda oturum açma AD FS tarafından verilen bir talep kullanarak iki aşamalı doğrulamayı atlama sürümüyle federasyona eklenenler. AD FS uygun trafiği intranet talep eklemek için bir kuralı olduğundan emin olun. Kural yoksa, aşağıdaki kural AD FS'de oluşturun:

      `c:[Type== "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"] => issue(claim = c);`

   * **Belirtilen IP adresi alt ağları gelen istekleri için**: Bu seçeneği belirlemek için metin kutusuna CIDR gösterimini kullanarak IP adreslerini girin.
      * Aralık xxx.xxx.xxx.1 xxx.xxx.xxx.254 aracılığıyla bulunan IP adresleri için gibi gösterimi kullanın **xxx.xxx.xxx.0/24**.
      * Tek bir IP adresi gibi gösterimi kullanın **xxx.xxx.xxx.xxx/32**.
      * 50 adede kadar IP adresi aralıklarını girin. Bu IP adreslerinden oturum açma kullanıcıların iki aşamalı doğrulamayı atlama.

6. **Kaydet**’i seçin.

## <a name="verification-methods"></a>Doğrulama yöntemleri

Kullanıcılarınız için kullanılabilir doğrulama yöntemleri seçebilirsiniz. Aşağıdaki tabloda yöntemleri kısa bir genel bakış sağlar.

Kullanıcılarınız için Azure multi-Factor Authentication, hesaplarını kaydettiğinizde, etkinleştirdiğiniz seçenekler arasından tercih edilen doğrulama yöntemi seçin. Kullanıcı kayıt sürecinde rehberlik sağlanır [hesabım için iki aşamalı doğrulamayı ayarlama](../user-help/multi-factor-authentication-end-user-first-time.md).

| Yöntem | Açıklama |
|:--- |:--- |
| Telefonu arama |Otomatik bir sesli çağrıyla yerleştirir. Kullanıcı telefonu yanıtladığında ve telefon tuş kimliğini doğrulamak için # tuşuna basar. Telefon numarası şirket içi Active Directory ile eşitlenmez. |
| Telefona kısa mesaj |Bir doğrulama kodu içeren bir kısa mesaj gönderir. Kullanıcı doğrulama kodunu oturum açma arabirimine girmesi istenir. Bu işlem, tek yönlü SMS çağrılır. İki yönlü SMS, belirli bir kodu metnin geri gerektiğini anlamına gelir. İki yönlü SMS kullanım dışı ve 14 Kasım 2018'den sonra desteklenmiyor. İki yönlü SMS otomatik olarak ayarlı için yapılandırılmış kullanıcılar _telefon_ doğrulama o zaman.|
| Mobil uygulama üzerinden bildirim |Telefonunuza veya kayıtlı cihazınıza anında iletme bildirimi gönderir. Kullanıcı bildirimi görünümleri ve seçer **doğrulama** doğrulamayı tamamlamak için. Microsoft Authenticator uygulamasını kullanılabilir [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071), [Android](https://go.microsoft.com/fwlink/?Linkid=825072), ve [iOS](https://go.microsoft.com/fwlink/?Linkid=825073). |
| Mobil uygulamadaki veya donanım belirtecindeki doğrulama kodu |Microsoft Authenticator uygulamasını her 30 saniyede yeni bir OATH doğrulama kodu oluşturur. Kullanıcı oturum açma arabirimine doğrulama kodunu girer. Microsoft Authenticator uygulamasını kullanılabilir [Windows Phone](https://go.microsoft.com/fwlink/?Linkid=825071), [Android](https://go.microsoft.com/fwlink/?Linkid=825072), ve [iOS](https://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="enable-and-disable-verification-methods"></a>Etkinleştirme ve doğrulama yöntemlerini devre dışı

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**.
4. Multi-Factor Authentication'ın altında seçin **hizmet ayarları**.
5. Üzerinde **hizmet ayarları** sayfasındaki **doğrulama seçenekleri**, kullanıcılarınıza sağlamak için yöntemleri seçin/seçimini kaldırın.
6. **Kaydet**’e tıklayın.

Kimlik doğrulama yöntemlerinin kullanımı hakkında ek bilgi makalesinde bulunabilir [kimlik doğrulama yöntemleri nelerdir](concept-authentication-methods.md).

## <a name="remember-multi-factor-authentication"></a>Çok Faktörlü Kimlik Doğrulamasını Anımsa

_Çok faktörlü kimlik doğrulamasını anımsa_ özelliğidir cihazlar ve kullanıcı tarafından güvenilen tarayıcılar için multi-Factor Authentication tüm kullanıcılar için ücretsiz bir özellik. Kullanıcılar, belirtilen sayıda gün, bunlar başarıyla bir cihaz için çok faktörlü kimlik doğrulaması kullanarak oturum açmış sonra sonraki doğrulamaları atlayabilirsiniz. Özellik, bir kullanıcının aynı cihazda iki adımlı doğrulama için sayısını en aza indirerek kullanılabilirliği geliştirir.

>[!IMPORTANT]
>Bir hesabı ya da cihaz tehlikedeyse, çok faktörlü kimlik doğrulaması için güvenilen cihazları hatırlamak güvenlik etkileyebilir. Bir kurumsal hesap güvenliği tehlikeye girdiğinde ya da güvenilir bir cihaz kaybolur veya çalınırsa, aşağıdakileri yapmalısınız [tüm cihazlarda multi-Factor Authentication geri](howto-mfa-userdevicesettings.md#restore-mfa-on-all-remembered-devices-for-a-user).
>
>Tüm cihazlardan bir güvenilen durum geri yükleme eylemini iptal eder ve kullanıcıyı yeniden iki adımlı doğrulama için gereklidir. Kullanıcılarınızın çok faktörlü kimlik doğrulaması'ndaki yönergeleri ile kendi cihazlarını geri yüklemek için de bildirebilirsiniz [iki adımlı doğrulama ayarlarınızı yönetme](../user-help/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted).
>

### <a name="how-the-feature-works"></a>Özelliği nasıl çalışır

Bir kullanıcı seçtiğinde anımsa multi-Factor Authentication özelliğini tarayıcıda kalıcı bir tanımlama bilgisi ayarlar **X için bir daha sorma gün** oturum açma seçeneği. Tanımlama bilgisi süresi doluncaya kadar tekrar için multi-Factor Authentication, aynı tarayıcıdan kullanıcıdan değil. Kullanıcının aynı cihazda farklı bir tarayıcı açar veya kendi tanımlama bilgilerini temizler, bunlar yeniden doğrulayın istenir.

**X için bir daha sorma gün** seçeneği, tarayıcı olmayan uygulamaları uygulaması modern kimlik doğrulamasını destekleyip desteklemediğini üzerinde göremiyorsanız. Bu uygulamaları kullanabilmek _yenileme belirteçleri_ her saat yeni erişim belirteçleri sağlar. Bir yenileme belirteci doğrulandığında, Azure AD'ye son iki aşamalı doğrulama belirtilen gün sayısı içinde oluştu denetler.

Bu özellik, normal olarak her zaman sor web apps'te, kimlik doğrulama sayısını azaltır. Bu özellik, normalde her 90 günde ister modern kimlik doğrulaması istemcileri kimlik doğrulamalarını sayısını artırır. Ayrıca, koşullu erişim ilkeleriyle birlikte kullanıldığında doğrulama sayısını artırabilir.

>[!IMPORTANT]
>**Çok faktörlü kimlik doğrulamasını anımsa** özelliği ile uyumlu değil **Oturumumu açık bırak** kullanıcılar gerçekleştirirken iki aşamalı doğrulama için AD FS ile Azure multi-Factor Authentication AD FS özelliği Kimlik doğrulama sunucusu veya bir üçüncü taraf multi-Factor authentication çözümünü.
>
>Kullanıcılarınızın seçerseniz **Oturumumu açık bırak** AD FS işareti olarak cihazlarını çok faktörlü kimlik doğrulaması için güvenilen Ayrıca, kullanıcının otomatik olarak sonra doğrulanmış değil **çok faktörlü kimlik doğrulamasıunutmayın**gün sayısı sona. Azure AD yeni iki aşamalı doğrulama isteklerini, ancak AD FS özgün çok faktörlü kimlik doğrulaması talep ve tarih yerine yeniden gerçekleştiriliyor iki aşamalı doğrulama ile bir belirteç döndürür. **Azure arasında bir doğrulama döngüsü kapalı bu tepki ayarlar AD ve AD FS.**
>

### <a name="enable-remember-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamasını etkinleştir unutmayın

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol tarafta, seçin **Azure Active Directory** > **kullanıcılar ve gruplar** > **tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**.
4. Multi-Factor Authentication'ın altında seçin **hizmet ayarları**.
5. Üzerinde **hizmet ayarları** sayfasında **yönetmek çok faktörlü kimlik doğrulamasını anımsa**seçin **çok faktörlü kimlik doğrulamasını anımsa kullanıcılarıngüvendiklericihazlarda**seçeneği.
6. İki aşamalı doğrulamayı atlama güvenilen cihazlara izin vermek için gün sayısını ayarlayın. Varsayılan değer 14 gündür.
7. **Kaydet**’i seçin.

### <a name="mark-a-device-as-trusted"></a>Bir aygıt güvenilen olarak işaretleyin

Parolayı anımsa çok faktörlü kimlik doğrulaması özelliği etkinleştirdikten sonra kullanıcıların ne zaman güvenilir olarak bir cihaz işaretleyebilirsiniz seçerek oturum **bir daha sorma**.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD oturum açma sayfasında bulunan marka değiştirme](../fundamentals/customize-branding.md)
