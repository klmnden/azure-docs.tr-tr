---
title: Privileged Identity Management nedir? -Azure Active Directory | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) genel bir bakış sağlar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: be8b9fe027a023cec6c816fa641beb41e5849741
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59496087"
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management nedir?

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) yönetebilir, denetleyebilir ve kuruluşunuzdaki önemli kaynaklara erişimini izlemeye olanak sağlayan bir hizmettir. Azure AD, Azure kaynakları ve Office 365 veya Microsoft Intune gibi diğer Microsoft Online Services hizmetlerindeki kaynaklara erişim de bu kapsama dahildir.

## <a name="why-should-i-use-pim"></a>PIM neden kullanmalıyım?

Kuruluşlar, bilgi veya kaynaklarına erişimin alma kötü amaçlı bir aktör olasılığını azaltacak olmadığından veya yetkili bir kullanıcı yanlışlıkla hassas kaynak etkileyen güvenliğini sağlamak için erişimi olan kişi sayısını en aza indirmek istiyorsunuz. Ancak kullanıcıların Azure AD, Azure, Office 365 veya SaaS uygulamalarında yine de ayrıcalıklı işlemler gerçekleştirmesi gerekir. Kuruluşlar, kullanıcıların verebilirsiniz just-in-time (JIT) ayrıcalıklı erişim için Azure kaynaklarını ve Azure AD. Yönetici ayrıcalıklarını bu kullanıcıların ne yaptıklarını için gözetim gerek yoktur. PIM aşırı, gereksiz riskini azaltmaya yardımcı olur ve erişim hakları kullandı.

## <a name="what-can-i-do-with-pim"></a>PIM ile ne yapabilirim?

PIM temelde, yönetmenize yardımcı olur kimin, ne, ne zaman, nerede ve neden, önem verdiğiniz kaynakları için. PIM önemli özelliklerinden bazıları şunlardır:

- Sağlamak **just-ın-time** ayrıcalıklı erişim için Azure AD ve Azure kaynakları
- Ata **zamana bağlı** başlangıç ve bitiş tarihlerini kullanarak kaynaklarına erişimi
- Gerekli **onay** ayrıcalıklı rolleri etkinleştirmek için
- Zorunlu **çok faktörlü kimlik doğrulaması** herhangi bir rolü etkinleştirmek için
- Kullanım **gerekçe** neden kullanıcılar etkinleştirme anlamak için
- Alma **bildirimleri** ayrıcalıklı rolleri zaman etkinleşir
- Kuralları **erişim gözden geçirmeleriyle** kullanıcılar yine rollerini sağlamak için
- İndirme **denetim geçmişi** iç veya dış denetim için

## <a name="prerequisites"></a>Önkoşullar

PIM'i kullanmak için aşağıdaki ücretli veya deneme sürümü lisansları biri olması gerekir. Daha fazla bilgi için bkz. [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md).

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Kullanıcıların lisansları hakkında daha fazla bilgi için bkz: [lisans PIM kullanmak için gereksinimler](subscription-requirements.md).

## <a name="terminology"></a>Terminoloji

PIM ve belgelerin daha iyi anlamak için aşağıdaki koşulları gözden geçirmelidir.

| Kavram veya sözleşme | Rol ataması kategorisi | Açıklama |
| --- | --- | --- |
| Uygun | Type | Bir kullanıcı rolü kullanmak için bir veya daha fazla eylem gerçekleştirmek gereken bir rol ataması. Bir kullanıcı rolü için uygun duruma getirildi, ayrıcalıklı görevleri gerçekleştirmek ihtiyaç duydukları zaman rolünü etkinleştirebilir anlamına gelir. Kalıcı uygun rol atamasını karşı kimseler verilen erişimi hiçbir fark yoktur. Tek fark, bazıları her zaman bu erişim olmanızın gerekmemesidir. |
| etkin | Type | Bir kullanıcı rolü kullanmak için herhangi bir eylemi gerçekleştirmek gerektirmeyen bir rol ataması. Etkin olarak atanan kullanıcılar role atanmış ayrıcalıklarına sahiptir. |
| etkinleştirme |  | Bir kullanıcı için uygun bir rolü kullanmak için bir veya daha fazla eylemler gerçekleştirme işlemi. Bir iş gerekçesi sağlamak veya belirlenmiş onaylayanlar onayı isteyen bir çok faktörlü kimlik doğrulaması (MFA) denetimi gerçekleştirme işlemleri içerebilir. |
| Atanan | Durum | Etkin bir rol atamanız bulunan bir kullanıcı. |
| Etkinleştirildi | Durum | Bir uygun rol ataması bulunan bir kullanıcı rolü etkinleştirmek için eylemleri ve artık etkindir.  Sonra kullanıcı rolü bir önceden yapılandırılmış süre,-yeniden etkinleştirmek ihtiyaç duydukları önce bir süre boyunca kullanabilirsiniz. |
| kalıcı uygun | Süre | Bir kullanıcı her zaman rolü etkinleştirmek uygun olduğu bir rol ataması. |
| kalıcı etkin | Süre | Bir rol ataması burada bir kullanıcı her zaman rol herhangi bir eylem gerçekleştirmeden kullanabilirsiniz. |
| uygun sona | Süre | Bir kullanıcı rolü içinde belirtilen bir başlangıç ve bitiş tarihi etkinleştirmek uygun olduğu bir rol ataması. |
| Etkin süresi dolacak | Süre | Bir rol ataması herhangi bir eylem içinde belirtilen bir başlangıç ve bitiş tarihi yapmadan bir kullanıcı rolü burada kullanabilirsiniz. |
| Just-in-time (JIT) erişim |  | Kötü amaçlı veya yetkisiz kullanıcıların izinleri süresi dolduktan sonra erişmesini engelleyen kullanıcılar ayrıcalıklı görevlerin gerçekleştirilmesi için geçici izinleri aldıkları modeli. Yalnızca kullanıcıların gerektiğinde erişim izni verilir. |
| en az ayrıcalık erişim ilkesi |  | Her kullanıcı en düşük ayrıcalıkları ile yalnızca sağlanan önerilen bir güvenlik uygulaması gerçekleştirmek için yetkili görevleri gerçekleştirmek gerekli. Bu yöntem, genel yönetici sayısı en aza indirir ve bunun yerine özel yönetici rolleri belirli senaryolar için kullanır. |

## <a name="what-does-pim-look-like"></a>PIM ne gibi görünüyor?

PIM ayarladıktan sonra gördüğünüz **görevleri**, **Yönet**, ve **etkinlik** sol gezinti menüsündeki seçenekleri. Bir yönetici olarak yönetme arasında seçeceğiz **Azure AD rolleri** ve **Azure kaynak** rolleri. Yönetmek için rol türünü seçtiğinizde, bu rol türü seçeneklerini benzer bir dizi görürsünüz.

![Azure portalında PIM ekran görüntüsü](./media/pim-configure/pim-overview.png)

## <a name="who-can-do-what-in-pim"></a>PIM'de kimlerin ne yapabilirsiniz?

PIM'i kullanmak ilk kişi siz, otomatik olarak size atanır [Güvenlik Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) ve [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) dizindeki roller.

Azure AD rolleri için PIM diğer yöneticiler için atamaları yalnızca ayrıcalıklı Rol Yöneticisi rolünde olduğu bir kullanıcı yönetebilirsiniz. Yapabilecekleriniz [PIM yönetmek için diğer yöneticilere erişim](pim-how-to-give-access-to-pim.md). Genel Yöneticiler, güvenlik yöneticileri ve güvenlik okuyucuları PIM'de Azure AD rol atamalarını görüntüleyebilirsiniz.

Azure kaynak rolleri, yalnızca bir abonelik Yöneticisi için bir kaynak sahibi veya bir kaynak kullanıcı erişimi Yöneticisi diğer yöneticiler PIM atamalarını yönetebilir. Kullanıcılar ayrıcalıklı rol yöneticileri, güvenlik yöneticileri ve güvenlik okuyucuları varsayılan olarak Azure kaynak rolleri için atamalar PIM içinde görüntülemek için erişim izniniz.

## <a name="scenarios"></a>Senaryolar

PIM aşağıdaki senaryoları destekler:

**Ayrıcalıklı Rol Yöneticisi olarak şunları yapabilirsiniz:**

- Belirli roller için onay etkinleştirmek
- İstekleri onaylayacak onaylayan kullanıcıları ve/veya grupları belirlemek
- Tüm ayrıcalıklı roller için istek ve onay geçmişini görüntülemek

**Onaylayan olarak, şunları yapabilirsiniz:**

- Bekleyen onayları (istekler) görüntülemek
- Rol yükseltme (tek ve/veya toplu) isteklerini onaylamak veya reddetmek
- Onay/red için gerekçe sağlamak 

**Uygun rol kullanıcı olarak, şunları yapabilirsiniz:**

- Onay gerektiren bir rolün etkinleştirilmesini istemek
- Etkinleştirme isteğinizin durumunu görüntülemek
- İstek onaylanmışsa Azure AD'deki görevinizi tamamlamak

## <a name="next-steps"></a>Sonraki adımlar

- [PIM'i kullanmak için lisans gereksinimleri](subscription-requirements.md)
- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](../users-groups-roles/directory-admin-roles-secure.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
- [PIM dağıtma](pim-deployment-plan.md)
