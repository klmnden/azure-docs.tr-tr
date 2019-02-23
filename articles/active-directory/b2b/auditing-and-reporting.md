---
title: Denetim ve B2B işbirliği kullanıcısı - Azure Active Directory raporlama | Microsoft Docs
description: Azure Active Directory B2B işbirliği Konuk kullanıcı özelliklerini yapılandırılamaz
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: dad4872f9bc32a1978de47a52cea23d6bb2742a1
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56673622"
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

