---
title: Azure Active Directory kimlik koruması | Microsoft Docs
description: Azure AD kimlik koruması, güvenliği aşılmış kimlik veya cihaz yararlanma ve güvenli bir kimlik veya daha önce olduğundan şüphelenilen veya tehlikeye bilinen bir cihaz yeteneği bir saldırganın sınırlamak nasıl olanak tanıdığını öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2019
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a1154e6484ebc86743202239dcd94f0772c8011
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67204512"
---
# <a name="what-is-azure-active-directory-identity-protection"></a>Azure Active Directory kimlik koruması nedir?

Azure Active Directory kimlik koruması, kullanıcı kimliklerini ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırmak, kuruluşların sağlar.

## <a name="get-started"></a>başlarken

Microsoft bulut tabanlı kimliklerin aşkın için güvenli. Azure Active Directory kimlik koruması ile ortamınızda kimlikleri güvenliğini sağlamak için Microsoft'un kullandığı aynı koruma sistemlerini kullanabilirsiniz.

Güvenlik ihlallerini büyük çoğunluğu göz önüne bir yerde saldırganların bir kullanıcının kimliğini çalarak bir ortama erişimi elde edin. Yıllar içinde saldırganlar kullanarak gelişmiş kimlik avı saldırıları ve üçüncü taraf ihlallerini yararlanarak, giderek daha etkili hale gelmiştir. Bir saldırganın da düşük ayrıcalıklı kullanıcı hesapları için erişim kazanır hemen sonra bunları yanal hareket önemli şirket kaynaklarına erişim kazanmak oldukça kolay bir işlemdir.

Bu söz konusu kümelerdeki şunları yapmanız gerekir:

- Kendi ayrıcalık düzeyi ne olursa olsun tüm kimlikleri koruyun

- Proaktif olarak güvenliği aşılmış kimlik kötüye gelen engelle

Tehlikeye atılmış kimlik keşfetme hiçbir kolay bir görevdir. Azure Active Directory Uyarlamalı makine öğrenimi algoritmaları kullanır ve anormallikleri ve olası gösterir şüpheli olayları algılamak için buluşsal yöntemler kimlikleri riske. Kimlik koruması, bu verileri kullanarak raporlar ve algılanan sorunlarını değerlendirin ve uygun bir risk azaltma veya düzeltme eylemleri olanak tanıyan uyarılar oluşturur.

Azure Active Directory kimlik koruması bir izleme ve raporlama aracıyla daha büyük. Kuruluşunuzun kimliklerini korumak için belirtilen risk düzeyi ulaşıldığında otomatik olarak algılanan sorunlara yanıt risk tabanlı ilkeler yapılandırabilirsiniz. Azure Active Directory tarafından sağlanan diğer koşullu erişim denetimleri ek olarak bu ilkeleri ve [Enterprise Mobility + Security](https://docs.microsoft.com/enterprise-mobility-security/) (EMS), otomatik olarak engellemek ya da dahil olmak üzere Uyarlamalı düzeltme eylemleri başlatmak parola sıfırlama ve çok faktörlü kimlik doğrulaması zorlama.

### <a name="identity-protection-capabilities"></a>Kimlik koruma özellikleri

**Güvenlik Açıkları ve riskli hesapları algılama:**  

- Güvenlik açıklarını vurgulayarak genel güvenlik duruşunu geliştirmek için özel öneriler sağlama
- Oturum açma risk düzeyleri hesaplanıyor
- Kullanıcı risk düzeyleri hesaplanıyor

**Risk olayları araştırma:**

- Risk olayları için bildirimleri gönderme
- Risk olayları ve ilgili bağlamsal bilgileri kullanarak araştırma
- Araştırmalar izlemek için temel iş akışı sağlama
- Düzeltme eylemleri parola sıfırlama gibi kolay erişim sağlama

**Risk tabanlı koşullu erişim ilkeleri:**

- Riskli oturum açma, oturum açma engelleme veya çok faktörlü kimlik doğrulaması zorluklarını gerektirerek azaltmak için ilke
- Blok veya güvenli riskli kullanıcı hesapları için ilke
- Kullanıcı çok faktörlü kimlik doğrulamasına kaydolacak şekilde zorunlu tutmak için ilke

## <a name="identity-protection-roles"></a>Kimlik koruması rolleri

Yük Dengelemesi için yönetimi etkinlikleri, kimlik koruması uygulamanızın etrafında çeşitli roller atayabilirsiniz. Azure AD kimlik koruması 3 dizin rolünü destekler:

| Rol | Yapabilirsiniz | Bunu yapamazsınız |
| :-- | --- | --- |
| Genel yönetici | Kimlik koruması, yerleşik kimlik koruması tam erişim| |
| Güvenlik yöneticisi | Kimlik koruması tam erişim | Yerleşik kimlik koruması, kullanıcı parolalarını sıfırlama |
| Güvenlik okuyucusu | Kimlik koruması salt okunur erişim | Yerleşik kimlik koruması, kullanıcıları düzeltme, ilkelerini yapılandırma, parolaları sıfırlama |

Daha fazla ayrıntı için [Azure Active Directory'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md)

## <a name="detection"></a>Algılama

### <a name="vulnerabilities"></a>Güvenlik Açıkları

Azure Active Directory kimlik koruması yapılandırmanızı analiz eder ve kullanıcılarınızın kimliklerini üzerinde bir etkisi olan güvenlik açıklarını algılar. Daha fazla ayrıntı için [Azure Active Directory kimlik koruması tarafından algılanan güvenlik açıklarını](vulnerabilities.md).

### <a name="risk-events"></a>Risk olayları

Azure Active Directory, kullanıcılarınızın kimliklerini ilgili kuşkulu eylemleri algılamak için Uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Sistem algılanan her bir şüpheli işlem için bir kayıt oluşturur. Bu kayıtlar risk olayları da verilir.  
Daha ayrıntılı bilgi için bkz. [Azure Active Directory risk olayları](../active-directory-identity-protection-risk-events.md).

## <a name="investigation"></a>Araştırma

Kimlik koruması aracılığıyla yolculuğunuza genellikle kimlik koruması panosu ile başlar.

![Düzeltme](./media/overview/1000.png "düzeltme")

Pano şunlara erişmenizi sağlar:

- Raporları gibi **risk için işaretlenen kullanıcılar**, **Risk olayları** ve **güvenlik açıkları**
- Yapılandırması gibi ayarları, **güvenlik ilkeleri**, **bildirimleri** ve **çok faktörlü kimlik doğrulaması kaydı**

Genellikle, başlangıç noktası olan etkinlikleri, günlükleri ve düzeltme ya da risk azaltma adımlarını gerekli olup olmadığını karar vermek için bir risk olayını ilgili diğer ilgili bilgileri gözden geçirme işlemi, araştırma ve kimlik nasıldı. gizliliği ve güvenliği aşılmış kimlik nasıl kullanıldığını anlayın.

Araştırma etkinliklerinizi bağlayabilirsiniz [bildirimleri](notifications.md) Azure Active Directory koruması e-posta gönderir.

## <a name="policies"></a>İlkeler

Otomatik yanıtlar uygulamak için Azure Active Directory kimlik koruması, üç ilkeleriyle sağlar:

- [Çok faktörlü kimlik doğrulaması kayıt ilkesi](howto-mfa-policy.md)
- [Kullanıcı riski İlkesi](howto-user-risk-policy.md)
- [Oturum açma riski İlkesi](howto-sign-in-risk-policy.md)

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Active Directory P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Kanal 9: Azure AD kimlik gösterin: Kimlik koruması önizlemesi](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
- [Azure Active Directory kimlik Koruması'nı etkinleştirme](enable.md)
