---
title: Azure Danışmanı izinleri
description: Advisor izinleri ve nasıl yeteneğinizi aboneliklerini yapılandırma veya erteleyebilir veya öneri yok sayın engelleyebilir.
services: advisor
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 04/03/2019
ms.author: kasparks
ms.openlocfilehash: cbd2e456c96dbf8ca01387f0c7c17a1541dbfe55
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59052800"
---
# <a name="permissions-in-azure-advisor"></a>Azure Danışmanı izinleri

Azure Danışmanı kullanım ve Azure kaynaklarının ve abonelikleri yapılandırma göre öneriler sağlar. Advisor'ı kullanan [yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) tarafından sağlanan [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/overview) öneriler ve Danışman özelliklerine erişiminizi yönetmek için (RBAC). 

## <a name="roles-and-their-access"></a>Rolleri ve bunların erişim

Aşağıdaki tabloda rolleri ve Danışman içinde sahip oldukları erişimi tanımlar:

| **Rol** | **Önerileri görüntüle** | **Kuralları Düzenle** | **Abonelik yapılandırmasını düzenle** | **Kaynak grubu Yapılandırması Düzenle**| **Kapat ve öneriler ertele**|
|---|:---:|:---:|:---:|:---:|:---:|
|Abonelik sahibi|**X**|**X**|**X**|**X**|**X**|
|Abonelik katkıda bulunanı|**X**|**X**|**X**|**X**|**X**|
|Abonelik okuyucusu|**X**|--|--|--|--|
|Kaynak grubu sahibi|**X**|--|--|**X**|**X**|
|Kaynak grubu Katılımcısı|**X**|--|--|**X**|**X**|
|Kaynak grubu okuyucusu|**X**|--|--|--|--|
|Kaynak sahibi|**X**|--|--|--|**X**|
|Kaynak katkıda bulunan|**X**|--|--|--|**X**|
|Kaynak okuyucu|**X**|--|--|--|--|

> [!NOTE]
> Önerileri görüntüleme erişimi öneri 's etkilenen kaynak erişiminizi bağlıdır.

## <a name="permissions-and-unavailable-actions"></a>Kullanılabilir eylemler ve izinler

Uygun izinlerinde eksiklik Danışmanı'nda eylemleri gerçekleştirme olanağı engelleyebilirsiniz. Bazı yaygın sorunlar aşağıda verilmiştir.

### <a name="unable-to-configure-subscriptions-or-resource-groups"></a>Abonelik veya kaynak grupları yapılandırılamadı

Abonelik veya kaynak grupları Danışmanı'nda yapılandırma denediğinizde, dahil etmek veya hariç tutmak için bu seçeneği devre dışı olduğunu görebilirsiniz. Bu durum, yeterli bir kaynak grubu veya abonelik için izin düzeyini olmadığını gösterir. Bu sorunu çözmek için bilgi nasıl [kullanıcı erişimi](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal).

### <a name="unable-to-postpone-or-dismiss-a-recommendation"></a>Erteleme veya bir öneri Kapat

Erteleyebilir veya bir öneri Kapat çalışırken bir hata alırsanız, yeterli izinlere sahip. En az olduğundan emin olun öneri erteleniyor veya kapatılıyor etkilenen kaynak katkıda bulunan erişimi. Bu sorunu çözmek için bilgi nasıl [kullanıcı erişimi](https://docs.microsoft.com/azure/role-based-access-control/quickstart-assign-role-user-portal).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Advisor RBAC denetim kullanıcı izinleri nasıl kullandığı ve sık karşılaşılan sorunları çözmek nasıl bir genel bakış getirdi. Advisor hakkında daha fazla bilgi için bkz:

- [Azure Advisor nedir?](https://docs.microsoft.com/azure/advisor/advisor-overview)
- [Azure Advisor'ı kullanmaya başlama](https://docs.microsoft.com/azure/advisor/advisor-get-started)
