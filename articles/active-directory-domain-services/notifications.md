---
title: 'Azure Active Directory etki alanı Hizmetleri: Bildirim ayarları | Microsoft Docs'
description: Azure AD Domain Services için bildirim ayarları
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: b9af1792-0b7f-4f3e-827a-9426cdb33ba6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: fab680e8e26584e6beeaf4bdb205c721e1b0b91e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246967"
---
# <a name="notification-settings-in-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri'nde bildirim ayarları

Bildirimler için Azure AD Domain Services yönetilen etki alanınızda sistem durumu uyarısı algılandığında hemen sonra güncelleştirilecek sağlar.  

Bu özellik, yalnızca klasik sanal ağlarda olmayan yönetilen etki alanları için kullanılabilir.


## <a name="how-to-check-your-azure-ad-domain-services-email-notification-settings"></a>Azure AD Domain Services e-posta bildirim ayarlarınızı kontrol etme

1. Gidin [Azure AD Domain Services sayfası](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında
2. Yönetilen etki alanınıza tablodan seçin
3. Sol gezinti bölmesinde seçin **bildirim ayarları**

Sayfada tüm Azure AD Domain Services için e-posta bildirimlerinin e-posta alıcılarını listeler.

## <a name="what-does-an-email-notification-look-like"></a>Bir e-posta bildirimi ne gibi görünüyor?

Aşağıdaki resimde, bir e-posta bildirimi örneğidir:

![Örnek e-posta bildirimi](./media/active-directory-domain-services-alerts/email-alert.png)

E-posta uyarı üzerinde mevcut olan yönetilen bir etki alanı hem de Azure portalında Azure AD Domain Services durumu sayfasına zaman algılama ve bağlantı vererek belirtir.

> [!WARNING]
> Her zaman e-posta, e-postaları bağlantıları tıklamadan önce doğrulanmış bir Microsoft gönderenden geldiğinden emin olun. E-postaların e-postadan her zaman gelir. azure-noreply@microsoft.com


## <a name="why-would-i-receive-email-notifications"></a>E-posta bildirimleri almak neden?

Azure AD etki alanı Hizmetleri, etki alanınız hakkında önemli güncelleştirmeleri için e-posta bildirimleri gönderir.  Bu, hizmetiniz etkiler ve hemen ilgilenilmesi Acil sorunları için bildirimlerdir. Her e-posta bildirimi, yönetilen etki alanınızdaki bir uyarı tetiklenir. Bu uyarılar ayrıca Azure portalında görüntülenir ve görüntülenebilir [health sayfasında Azure AD Domain Services](check-health.md).

Azure AD Domain Services göndermez e-posta reklam, güncelleştirmeleri veya satış amaçları için bu listeye.

## <a name="when-will-i-receive-email-notifications"></a>E-posta bildirimleri ne zaman gönderilir mi?

Hemen bir bildirim gönderilir, bir [yeni uyarı](troubleshoot-alerts.md) yönetilen etki alanınızda bulunamadı. Uyarı çözümlenemedi, dört her gün bir anımsatıcı bir e-posta bildirimi gönderilir.

## <a name="who-should-receive-the-email-notifications"></a>Kimlerin e-posta bildirimleri alması gerekir?


 Azure AD etki alanı hizmetleri yönetmek ve yönetilen etki alanında değişiklik kişilerin oluşturulması e-posta alıcı listesi önerilir. Bu e-posta listesi olan, "ilk Yanıtlayıcı" bulunan herhangi bir sorun için düşünülmelidir. Eklemek istediğiniz beşten fazla ek e-postaları varsa, bunun yerine bildirim listesine eklemek için bir dağıtım listesi oluşturmanızı öneririz.

Azure AD Domain Services ile ilgili bildirimler için beş adede kadar ek e-postaları ekleyebilirsiniz. Ayrıca, tüm genel yöneticilerin dizininizin sağlamak de seçebilirsiniz ve her grup 'AAD DC Administrators' üyesi Azure AD Domain Services e-posta bildirimleri alırsınız. Azure AD etki alanı Hizmetleri yalnızca genel Yöneticiler ve AAD DC Administrators listesi dahil olmak üzere en fazla 100 e-posta adreslerine bildirim gönderir.


## <a name="how-to-add-an-additional-email-recipient"></a>Ek e-posta alıcı ekleme

> [!WARNING]
> Bildirim ayarlarını değiştirirken, tüm yönetilen etki alanı, yalnızca kendiniz için bildirim ayarlarını değiştiriyorsunuz.

1. Gidin [Azure AD Domain Services sayfası](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında.
2. Yönetilen etki alanınızda tıklayın.
3. Sol gezinti bölmesinden **bildirim ayarları**.
4. E-posta eklemek, ek alıcılar e-posta adresini yazın.
5. Üst taraftaki gezinti "Kaydet"'e tıklayın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

#### <a name="i-received-an-email-notification-for-an-alert-but-when-i-logged-on-to-the-azure-portal-there-was-no-alert-what-happened"></a>Bir uyarı için bir e-posta bildirimi aldım ancak ben Azure portalında oturum açtığında herhangi bir uyarı oluştu. Ne oldu?

Uyarı, bir uyarı çözümlenir, Azure portalından kaybolur. En olası sebep, başka birinin e-posta bildirimleri alan, yönetilen etki alanınızda uyarı çözümlendi veya Azure AD Domain Services tarafından otomatik olarak çözüldü ' dir.


#### <a name="why-can-i-not-edit-the-notification-settings"></a>Neden bildirim ayarlarını düzenleyebilirim değil?

Bildirim Ayarları sayfasında Azure portalında erişim bulamıyorsanız, Azure AD Domain Services'ı düzenlemek için izinlere sahip değil. Ya da Azure AD Domain Services kaynakları düzenlemek veya alıcı listesinden kaldırılması için izin almak için genel yöneticinize başvurmanız gerekir.

#### <a name="i-dont-seem-to-be-receiving-email-notifications-even-though-i-provided-my-email-address-why"></a>E-posta adresimi verdiğim olsa bile e-posta bildirimleri yararlanıyor olmadığımı. Neden?

Bildirim e-postanıza istenmeyen veya önemsiz klasörünüzü kontrol edin ve izin verilenler listesine gönderen emin olun (azure-noreply@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen etki alanınızda uyarıları çözümleyin](troubleshoot-alerts.md)
- [Azure AD Domain Services hakkında daha fazla bilgi](overview.md)
- [Ürün ekibine başvurun](contact-us.md)

## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](contact-us.md).
