---
title: Azure AD SSPR - Azure Active Directory ile şirket içi parola geri yazma tümleştirme
description: Bulut parolalarını şirket içine geri yazılan alma AD infratstructure
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 491545aabd3415850eb1b1d712a46401b73ad845
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190723"
---
# <a name="what-is-password-writeback"></a>Parola geri yazma nedir?

Bulut tabanlı parola sıfırlama yardımcı olması idealdir ancak çoğu şirket kullanıcılarının bulunduğu bir şirket içi dizin çözümlenmedi. Microsoft destek tutma geleneksel nasıl yaptığını Active Directory (AD) parola değişikliği bulutta eşitlenmiş şirket içinde mi? Parola geri yazma, etkinleştirilmiş olan bir özelliktir [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) parola değişiklikleri gerçek zamanlı olarak mevcut bir şirket içi dizine geri yazılması için bulutta sağlar.

Parola geri yazma özelliğini kullanan ortamlarda desteklenir:

* [Active Directory Federasyon Hizmetleri](../hybrid/how-to-connect-fed-management.md)
* [Parola karması eşitleme](../hybrid/how-to-connect-password-hash-synchronization.md)
* [Doğrudan kimlik doğrulama](../hybrid/how-to-connect-pta.md)

> [!WARNING]
> Parola geri yazma, Azure AD Connect sürüm 1.0.8641.0 ve eski olduğunda kullanan müşteriler için çalışma durdurur [Azure erişim denetimi hizmeti (ACS) 7 Kasım 2018'de kullanımdan](../develop/active-directory-acs-migration.md). Azure AD Connect sürüm 1.0.8641.0 eski ve bunlar üzerinde ACS işlevselliği için bağımlı olduğundan parola geri yazma o anda artık izin verir.
>
> Hizmette, yeni bir sürüme bir Azure AD Connect'in önceki sürümünden yükseltme kesinti yaşanmasını önlemek için bu makaleye bakın [Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme](../hybrid/how-to-upgrade-previous-version.md)
>

Parola geri yazma sağlar:

* **Şirket içi Active Directory parola ilkelerinin uygulanması**: Bir kullanıcı parolasını sıfırlandıktan sonra bu dizine yapmadan önce şirket içi Active Directory ilkesini karşıladığından emin olmak için denetlenir. Bu gözden geçirme geçmişini, karmaşıklık, yaş, parola filtreleri ve yerel Active Directory içinde tanımladığınız herhangi bir parola kısıtlama denetimi içerir.
* **Sıfır Gecikmeli geri bildirim**: Parola geri yazma eşzamanlı bir işlemdir. Parola ilkesini karşılamadığı veya değil sıfırlama veya herhangi bir nedenle değiştirdiyseniz, kullanıcılarınızın hemen bildirilir.
* **Destekler parola değişikliklerini erişim panelinde ve Office 365**: Federe olduğunda veya parola karması eşitlenmiş kullanıcılar gelen süresi dolmuş ya da süresi doldu parolalarını değiştirmek için bu parolaları geri yerel Active Directory ortamınızı yazılır.
* **Bunları Azure portalından bir yönetici sıfırlar parola geri yazmayı destekler**: Her bir yönetici bir kullanıcının parolasını sıfırlar [Azure portalında](https://portal.azure.com), kullanıcının Federasyon ya da parola karması eşitlenmiş, parolayı yeniden şirket içine yazılır. Bu işlevsellik şu anda Office Yönetim Portalı'nda desteklenmiyor.
* **Herhangi bir gelen güvenlik duvarı kuralları gerektirmeyen**: Parola geri yazma, temel alınan bir iletişim kanalı bir Azure Service Bus geçişini kullanır. Tüm iletişim bağlantı noktası 443 üzerinden giden.

> [!Note]
> Parola geri yazma ile şirket içi Active Directory'de korumalı gruplardaki mevcut kullanıcı hesapları kullanılamaz. İçinde mevcut yönetici hesapları, grupları korumalı şirket içi AD parola geri yazma ile kullanılabilir. Korunan grupları hakkında daha fazla bilgi için bkz. [korumalı hesaplar ve gruplar Active Directory'de](https://technet.microsoft.com/library/dn535499.aspx).
>

## <a name="licensing-requirements-for-password-writeback"></a>Parola geri yazma için lisans gereksinimleri

**Self Servis parola sıfırlama/değiştirme/kilidini açma şirket içi geri yazma ile Azure AD premium özelliğidir**. Lisanslama hakkında daha fazla bilgi için bkz. [Azure Active Directory site fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

Parola geri yazma özelliğini kullanmak için kiracınızda atanan aşağıdaki lisanslardan birine sahip olmalıdır:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 veya A3
* Enterprise Mobility + Security E5'e veya A5
* Microsoft 365 E3 veya A3
* Microsoft 365 E5 veya A5
* Microsoft 365 F1
* Microsoft 365 İş

> [!WARNING]
> Tek başına Office 365 planları lisanslama *"Self Servis parola sıfırlama/değiştirme/kilidini açma şirket içi geri yazma ile" desteklemeyen* ve çalışmak bu işlev için önceki planlardan birine sahip olması gerekir.
>

## <a name="how-password-writeback-works"></a>Parola geri yazma nasıl çalışır?

Federe veya parola karma sıfırlama veya buluttaki parolalarını değiştirme denemelerini eşitlendiğinde, aşağıdaki eylemler gerçekleşir:

1. Kullanıcının parola türüne sahip görmek için bir onay yapılır. Yönetilen şirket içi parola ise:
   * Geri yazma hizmetinde çalışır duruma olup olmadığını görmek için bir onay yapılır. Bu durumda, kullanıcı geçebilirsiniz.
   * Geri yazma hizmetinin kapalı ise, kullanıcı parolalarını hemen sıfırlanamaz bilgisi verilir.
1. Ardından, kullanıcı uygun kimlik doğrulama geçitleri geçirir ve ulaştığında **parolayı Sıfırla** sayfası.
1. Kullanıcı yeni bir parola seçer ve bunu doğrular.
1. Kullanıcı seçtiğinde **Gönder**, düz metin parola geri yazma Kurulum işlemi sırasında oluşturulan bir simetrik anahtarla şifrelenir.
1. Şifreli parola, bir HTTPS kanalı üzerinden (yani sizin için geri yazma Kurulum işlemi sırasında kurulmuştur), bir kiracıya özgü service bus geçişi için gönderilen bir yükte dahildir. Bu geçiş, yalnızca şirket içi yüklemenizi bildiği rastgele oluşturulmuş bir parolayla korunuyor.
1. Service bus iletiyi ulaştıktan sonra parola sıfırlama uç noktası otomatik olarak uyanır ve sıfırlama isteği bekliyor olduğunu görür.
1. Hizmet bulut çapa özniteliği kullanılarak kullanıcı için ardından arar. Bu arama başarılı olması:

   * Kullanıcı nesnesi Active Directory Bağlayıcı alanında bulunmalıdır.
   * Kullanıcı nesnesi karşılık gelen meta veri deposu (MV) nesnesine bağlanmalıdır.
   * Kullanıcı nesnesi ilgili Azure Active Directory Bağlayıcısı nesneye bağlı olması gerekir.
   * Active Directory Bağlayıcısı nesnesinden MV bağlantısını eşitleme kuralı olmalıdır `Microsoft.InfromADUserAccountEnabled.xxx` bağlantısına.
   
   Çağrı buluttan geldiğinde, eşitleme altyapısı kullanan **cloudAnchor** Azure Active Directory Bağlayıcı alanı nesne aramak için özniteliği. Ardından bağlantıyı MV nesnesine geri izler ve sonra bağlantı geri Active Directory nesnesi için izler. Eşitleme altyapısı aynı kullanıcı birden çok Active Directory nesnelerini (çok ormanlı) olabileceğinden kullanır `Microsoft.InfromADUserAccountEnabled.xxx` doğru olanı seçmek için bağlantı.

1. Kullanıcı hesabı bulunduğunda, doğrudan uygun Active Directory ormanında parolayı sıfırlamak için bir deneme yapılır.
1. Parola ayarlama işlemi başarılı olursa, kullanıcının parolasını değiştirdiğinden bildirilir.
   > [!NOTE]
   > Kullanıcının parola karmasını Azure AD'ye parola karması eşitleme kullanılarak eşitlenmişse şirket içi parola ilkesini bulut parola ilkesini zayıf bir fırsat yoktur. Bu durumda, şirket içi ilke uygulanmaz. Bu ilke, çoklu oturum açma sağlamak için parola karması eşitleme veya Federasyon kullanıyorsanız şirket içi ilkenizi bulutta olursa olsun uygulanmasını sağlar.
   >

1. Parola ayarlama işleminin başarısız olması, bir hata yeniden denemek için kullanıcıya sorar. İşlemi nedeniyle başarısız olabilir:
    * Hizmet oldu.
    * Seçili parola kuruluşun ilkelerini karşılamadı.
    * Yerel Active Directory kullanıcı bulma yapılamıyor.

      Yönetici müdahalesi olmadan çözümlenecek deneyebilirsiniz için hata iletileri kullanıcılara rehberlik sağlar.

## <a name="password-writeback-security"></a>Parola geri yazma güvenlik

Parola geri yazma özelliğini bir yüksek oranda güvenli bir hizmettir. Bilgileriniz korunur emin olmak için aşağıda açıklandığı gibi bir dört katmanlı güvenlik modeli etkinleştirilir:

* **Kiracıya özgü service bus geçişi**
   * Hizmet ayarlarsanız, bir kiracıya özgü service bus geçişi, Microsoft hiçbir zaman erişebileceği rastgele oluşturulmuş güçlü bir parola ile korunan ayarlanır.
* **Kilitli, şifreleme açısından güçlü bir parola şifreleme anahtarı**
   * Service bus geçişi oluşturulduktan sonra güçlü bir simetrik anahtar, kablo üzerinden gelen parolayı şifrelemek için kullanılan oluşturulur. Bu anahtar yalnızca yoğun kilitli ve denetlenen dizindeki başka bir parola olduğu gibi bulutta şirketinizin gizli dizi deposu içinde bulunur.
* **Sektörde standart Aktarım Katmanı Güvenliği (TLS)**
   1. Bir parola sıfırlama veya değiştirme işlemi bulutta oluşur, düz metin parolayı ortak anahtarınızı ile şifrelenir.
   1. Şifreli parola, şifrelenmiş bir kanal üzerinden Microsoft SSL sertifikaları için service bus Geçişi'ni kullanarak gönderilen bir HTTPS iletisine yerleştirilir.
   1. Service bus İleti ulaştıktan sonra şirket içi aracınızı uyanır ve service bus-daha önce oluşturulmuş güçlü bir parola kullanarak kimliğini doğrular.
   1. Şirket içi aracı şifreli ileti seçer ve özel anahtarı kullanarak şifresini çözer.
   1. Şirket içi aracı AD DS SetPassword API'sinden parola ayarlama girişiminde bulunur. Bu adım ne Active Directory şirket içi parola ilkenizde (örneğin, karmaşıklık, yaş, geçmiş ve filtreleri) uygulanması sağlar, bulutun içinde bulunuyor.
* **İleti süre sonu ilkeleri**
   * Şirket içi hizmetiniz çalışmadığı için service bus ileti bulunur, zaman aşımına uğrar ve birkaç dakika sonra kaldırılır. Zaman aşımı ve iletinin kaldırılması ve güvenliği daha da artırır.

### <a name="password-writeback-encryption-details"></a>Parola geri yazma şifreleme ayrıntıları

Kullanıcı parola sıfırlama gönderdikten sonra şirket içi ortamınızda gelmeden önce sıfırlama isteği şifreleme adımları boyunca gider. Şifreleme adımları en yüksek hizmet güvenilirliği ve güvenliği emin olun. Bunlar aşağıda açıklanmıştır:

* **1. adım: Parola şifrelemesini 2048 bit RSA anahtarı ile**: Bir kullanıcı şirket içine geri yazılması için bir parola gönderdikten sonra gönderilen parola 2048 bit RSA anahtarıyla şifrelenir.
* **2. adım: Paket düzeyinde şifreleme, AES gcm'ye**: Paketin tamamını, parola ve gerekli meta veriler, AES-GCM kullanılarak şifrelenir. Bu şifreleme, doğrudan erişimi olan herkes görüntülemesini veya içeriğiyle oynama, temel alınan ServiceBus kanala engeller.
* **3. adım: TLS/SSL üzerinden tüm iletişimi gerçekleşir**: Service BUS ile tüm iletişimin bir SSL/TLS kanalda'olmuyor. Bu şifreleme, içeriği yetkisiz Üçüncü taraflardan güvenliğini sağlar.
* **Altı ayda üzerinde otomatik anahtar toplama**: Tüm anahtarları, altı ayda üzerinden veya parola geri yazmayı devre dışı ve ardından Azure AD Connect'in, en yüksek hizmet güvenlik ve güvenilirlik sağlamak için yeniden etkinleştirilen her zaman döndürün.

### <a name="password-writeback-bandwidth-usage"></a>Parola geri yazma bant genişliği kullanımı

Parola geri yazmayı yalnızca aşağıdaki durumlarda şirket içi aracı dön istekleri gönderen düşük bant genişliğine sahip bir hizmettir:

* Bu özellik etkin veya Azure AD Connect devre dışı olduğunda iki ileti gönderilir.
* Hizmet çalışıyor olduğu sürece her beş dakikada bir hizmet sinyali olarak bir ileti gönderilir.
* İki ileti gönderilen her zaman yeni bir parola gönderildi:
   * İlk ileti işlemi gerçekleştirmek için isteğidir.
   * İkinci bir ileti, işlemin sonucunu içerir ve aşağıdaki durumlarda gönderilir:
      * Her bir kullanıcı Self Servis parola sıfırlama sırasında yeni bir parola gönderilir.
      * Her bir kullanıcı parolasını değiştirme işlemi sırasında yeni bir parola gönderilir.
      * Her seferinde yeni bir parola sıfırlama (yalnızca Azure yönetim portallarında) bir yönetici tarafından başlatılan kullanıcı parolası sırasında gönderilir.

#### <a name="message-size-and-bandwidth-considerations"></a>İleti boyutu ve bant genişliği konuları

Genellikle her biri daha önce açıklanan ileti boyutu altında 1 KB ' dir. Aşırı yük altında bile, parola geri yazma hizmetinin kendisi birkaç kilobit / sn bant genişliği tüketiyor. Her ileti gerçek zamanlı olarak gönderdiğinden parola güncelleştirme işlemi tarafından yalnızca gerekli olduğunda ve ileti boyutu kadar küçük olduğu için geri yazma özelliği bant genişliği kullanımını ölçülebilir bir etkisi için çok küçük.

## <a name="supported-writeback-operations"></a>Desteklenen geri yazma işlemleri

Parolalar, aşağıdaki durumlarda geri yazılır:

* **Desteklenen son kullanıcı işlemleri**
   * Tüm son kullanıcı Self Servis gönüllü parola işlemi Değiştir
   * Tüm son kullanıcı Self Servis zorla parola işlemi, örneğin, parola sona erme süresini değiştirme
   * Tüm son kullanıcı Self Servis parola sıfırlama kaynaklanan [parola sıfırlama portalı](https://passwordreset.microsoftonline.com)
* **Desteklenen yönetici işlemleri**
   * Bir yönetici Self Servis gönüllü parola işlemi Değiştir
   * Tüm yönetici zorla Self Servis parola işlemi, örneğin, parola sona erme değiştirme
   * Bir yönetici Self Servis parola sıfırlama kaynaklanan [parola sıfırlama portalı](https://passwordreset.microsoftonline.com)
   * Tüm son kullanıcı yönetici tarafından başlatılan parola gelen sıfırlama [Azure portalı](https://portal.azure.com)

## <a name="unsupported-writeback-operations"></a>Desteklenmeyen geri yazma işlemleri

Parolalar *değil* aşağıdaki durumlarda hiçbirinde yazılır:

* **Desteklenmeyen son kullanıcı işlemleri**
   * PowerShell sürüm 1, sürüm 2 veya Azure AD Graph API'yi kullanarak kendi parolasını sıfırlama herhangi bir son kullanıcı
* **Desteklenmeyen yönetici işlemleri**
   * Tüm son kullanıcı yönetici tarafından başlatılan parola gelen sıfırlama [Office Yönetim Portalı](https://portal.office.com)
   * PowerShell sürüm 1, sürüm 2 veya Azure AD Graph API'si sıfırlama herhangi bir son kullanıcı yönetici tarafından başlatılan parola

> [!WARNING]
> Onay kutusu "kullanıcı sonraki oturumda parola değiştirmelidir Active Directory Kullanıcıları ve Bilgisayarları veya Active Directory Yönetim Merkezi gibi şirket içi Active Directory Yönetimsel Araçlar'daki değiştirmeli" kullanımı desteklenmiyor. Şirket içi, bir parola değiştirirken bu seçeneği işaretlemeyin. 

## <a name="next-steps"></a>Sonraki adımlar

Parola geri yazma öğreticiyi kullanarak etkinleştirin: [Parola geri yazma özelliğini etkinleştirme](tutorial-enable-writeback.md)
