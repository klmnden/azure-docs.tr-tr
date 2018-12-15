---
title: Denetim ve bir Azure Active Directory B2B işbirliği kullanıcısı raporlama | Microsoft Docs
description: Azure Active Directory B2B işbirliği Konuk kullanıcı özelliklerini yapılandırılamaz
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: fbe1cfdcfb1b5ec295748c3c77030df45323ee54
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53434333"
---
# <a name="auditing-and-reporting-a-b2b-collaboration-user"></a>B2B işbirliği kullanıcısı raporlama ve denetleme
Konuk kullanıcılar, denetim özelliklerine benzer şekilde üye kullanıcılarla sahip. 

## <a name="access-reviews"></a>Erişim gözden geçirmeleri
Erişim gözden geçirmeleri, konuk kullanıcıların kaynaklarınıza erişmek için yine de gerekip gerekmediğini düzenli olarak doğrulamak için kullanabilirsiniz. **Erişim gözden geçirmeleriyle** özelliği kullanılabilir **Azure Active Directory** altında **Yönet** > **kuruluş ilişkileri**. ("Erişim gözden geçirmeleri için" den arayabilirsiniz **tüm hizmetleri** Azure portalında.) Erişim gözden geçirmeleri kullanmayı öğrenmek için bkz: [Konuk erişimi yönetme ile Azure AD erişim gözden geçirmeleri](../governance/manage-guest-access-with-access-reviews.md).

## <a name="audit-logs"></a>Denetim günlükleri

Azure AD denetim günlükleri Konuk kullanıcılar tarafından başlatılan etkinlikler dahil olmak üzere, sistem ve kullanıcı etkinliklerinin kayıtlarını sağlar. Denetim günlüklerine erişmek için **Azure Active Directory**altında **izleme**seçin **denetim günlükleri**. Davetli Sam Oogle daveti ve kullanım geçmişini örneği aşağıda verilmiştir:

![denetleme günlüğü](./media/auditing-and-reporting/audit-log.png)

Ayrıntılı bilgi edinmek için bu olayların her biri içinde kullanmaya başlayabilirsiniz. Örneğin, kabul ayrıntıları bakalım.

![Etkinlik ayrıntıları](./media/auditing-and-reporting/activity-details.png)

Ayrıca, Azure AD'den Bu günlükleri dışarı aktarmak ve özelleştirilmiş raporlar almak için raporlama aracını kullanabilirsiniz.

### <a name="next-steps"></a>Sonraki adımlar

- [B2B işbirliği kullanıcı özellikleri](user-properties.md)

