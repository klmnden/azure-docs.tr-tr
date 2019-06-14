---
title: E-posta bildirimleri PIM - Azure Active Directory | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) e-posta bildirimleri tanımlar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 01/05/2019
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9aa9da83a7a6e97f5b721dad550831fe2645ffd3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60289196"
---
# <a name="email-notifications-in-pim"></a>PIM e-posta bildirimleri

Azure Active Directory (Azure AD) Privileged Identity Management (PIM), ne zaman bir rol atandıktan veya etkinleştirildikten gibi önemli olayları ne zaman oluştuğunu bilmenizi sağlar. PIM, diğer katılımcılarla bildirimleri e-posta göndererek haberdar tutar. Bu e-postaları da ilgili görevler, bu tür etkinleştirme veya bir rolü yenileme bağlantılar içerebilir. Bu makalede, bu e-postaları görünmesi, ne zaman gönderilir ve kimin alır açıklanır.

## <a name="sender-email-address-and-subject-line"></a>Gönderenin e-posta adresi ve konu satırı

E-postalar için her iki Azure AD PIM gönderilen ve Azure kaynağı rolleri aşağıdaki gönderen e-posta adresi vardır:

- E-posta adresi: **azure noreply\@microsoft.com**
- Görünen ad: Microsoft Azure

Bu e-postaları içeren bir **PIM** konu satırı önek. Bir örneği aşağıda verilmiştir:

- PIM: Alain Charon yedekleme okuyucu rolü kalıcı olarak atandı

## <a name="pim-emails-for-azure-ad-roles"></a>PIM e-postalar için Azure AD rolleri

Azure AD rolleri için aşağıdaki olaylar meydana geldiğinde PIM e-posta gönderir:

- Ayrıcalıklı rol Etkinleştirme Onayı Beklemede olduğunda
- Ayrıcalıklı rol etkinleştirme isteği tamamlandığında
- Azure AD PIM etkinleştirildiğinde

Rolünüz, olay ve bildirimleri olan Azure AD rolleri için bu e-postaları alır ayar bağlıdır:

| Kullanıcı | Rol Etkinleştirme Onayı Beklemede değil. | Rol etkinleştirme isteği tamamlandı | PIM etkin |
| --- | --- | --- | --- |
| Ayrıcalıklı Rol Yöneticisi</br>(Etkin/uygun) | Evet</br>(yalnızca hiçbir açık onaylayanlar belirtilirse) | Evet* | Evet |
| Güvenlik Yöneticisi</br>(Etkin/uygun) | Hayır | Evet* | Evet |
| Genel Yönetici</br>(Etkin/uygun) | Hayır | Evet* | Evet |

\* Varsa [ **bildirimleri** ayarı](pim-how-to-change-default-settings.md#notifications) ayarlanır **etkinleştirme**.

Aşağıda kurgusal Contoso kuruluş için Azure AD rolüne kullanıcı etkinleştirirken, gönderilen örnek e-posta gösterilmektedir.

![Yeni PIM e-posta için Azure AD rolleri](./media/pim-email-notifications/email-directory-new.png)

### <a name="weekly-pim-digest-email-for-azure-ad-roles"></a>Haftalık PIM Özet e-posta için Azure AD rolleri

Bir haftalık PIM Özet e-posta için Azure AD rolleri, ayrıcalıklı rol yöneticileri, güvenlik yöneticileri ve PIM etkin genel Yöneticiler gönderilir. Bu bir haftalık e-posta PIM etkinlikleri anlık görüntüsünü hafta yanı sıra ayrıcalıklı rol atamalarını sağlar. Yalnızca genel buluttaki kiracılar için kullanılabilir. Örnek e-posta aşağıda verilmiştir:

![Haftalık PIM Özet e-posta için Azure AD rolleri](./media/pim-email-notifications/email-directory-weekly.png)

E-posta dört kutucuk bulunur:

| kutucuğu | Açıklama |
| --- | --- |
| **Etkin kullanıcılar** | Kullanıcılar Kiracı içinde uygun rol etkin sayısı. |
| **Kalıcı yapılan kullanıcılar** | Uygun bir atamayı kullanıcılarla kalıcı yapılan deneme sayısı. |
| **PIM rol atamaları** | Uygun bir rol içindeki PIM atanan kullanıcılar deneme sayısı. |
| **PIM dışında rol atamaları** | (Azure AD içinde) PIM dışında kalıcı bir role atanan kullanıcılar deneme sayısı. |

**, Üst rollerine genel bakış** her rol için kalıcı ve uygun yönetici toplam sayısı temelinde kiracınıza beş rollerinde üst bölümde listelenmektedir. **Harekete** bağlantı açar [PIM Sihirbazı](pim-security-wizard.md) burada dönüştürebilirsiniz kalıcı yöneticiler uygun yöneticilere toplu.

## <a name="pim-emails-for-azure-resource-roles"></a>Azure kaynak rolleri için PIM postalar

Azure kaynak rolleri için aşağıdaki olaylar meydana geldiğinde PIM sahipleri ve kullanıcı erişim yöneticileri için e-posta gönderir:

- Bir rol ataması onay olduğunda
- Rol atandığında
- Ne zaman bir rol yakında sona erecek
- Bir rol genişletmek uygun olduğunda
- Bir rol, bir son kullanıcı tarafından ne zaman yenileniyor
- Rol etkinleştirme isteği tamamlandığında

Azure kaynak rolleri için aşağıdaki olaylar meydana geldiğinde PIM son kullanıcılara e-posta gönderir:

- Kullanıcıya bir rol atandığında
- Bir kullanıcının rolünü zaman süresi doldu
- Bir kullanıcının rolünü zaman genişletildi
- Bir kullanıcının rol etkinleştirme isteği tamamlandığında

Aşağıda kurgusal Contoso kuruluş için bir Azure Kaynak rolü atanmış bir kullanıcı, gönderilen örnek e-posta gösterilmektedir.

![Yeni PIM için e-posta Azure kaynağı rolleri](./media/pim-email-notifications/email-resources-new.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM'de Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [Onaylayın veya reddedin istekleri için Azure AD PIM rolleri](azure-ad-pim-approval-workflow.md)
