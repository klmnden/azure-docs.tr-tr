---
title: Kullanıcı verileri Azure Güvenlik Merkezi'nde yönetme | Microsoft Docs
description: " Kullanıcı verilerini Azure Güvenlik Merkezi'nde yönetmeyi öğrenin. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: terrylan
ms.openlocfilehash: 2495bae5c63cdafe049ec39f71ab53106c5f2df7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655815"
---
# <a name="manage-user-data-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde kullanıcı verilerini yönetme
Bu makalede Azure Güvenlik Merkezi kullanıcı verilerini yönetmek hakkında bilgi sağlar. Kullanıcı verileri yönetme erişim, silmek veya verileri dışarı aktarma özelliğini içerir.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

Okuyucu, sahibi, katkıda bulunan rolü atanmış bir güvenlik merkezi kullanıcı veya Hesap Yöneticisi araç içinde müşteri verilerine erişebilir. Bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/built-in-roles.md) okuyucusu, sahibi ve katkıda bulunan rolleri hakkında daha fazla bilgi edinmek için. Bkz: [Azure aboneliği yöneticileri](../billing/billing-add-change-azure-subscription-administrator.md) hesap yöneticisi rolü hakkında daha fazla bilgi edinmek için.

## <a name="searching-for-and-identifying-personal-data"></a>Arama ve kişisel veri tanımlama
Güvenlik Merkezi kullanıcı Azure portalı üzerinden kişisel verilerini görüntüleyebilir. Güvenlik Merkezi, yalnızca e-posta adresleri ve telefon numaraları gibi güvenlik kişi ayrıntıları depolar. Bkz: [Azure Güvenlik Merkezi'nde güvenlik iletişim ayrıntılarını sağlamak](security-center-provide-security-contact-details.md) daha fazla bilgi için.

Azure portalında bir kullanıcı yalnızca zaman VM erişim özelliği Güvenlik Merkezi'nin kullanarak izin verilen IP yapılandırmalarını görüntüleyebilirsiniz. Daha fazla bilgi için bkz. [Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](security-center-just-in-time.md).

Azure portalında bir kullanıcı IP adreslerini ve saldırgan ayrıntılarını da dahil olmak üzere Güvenlik Merkezi tarafından sağlanan güvenlik uyarılarını görüntüleyebilirsiniz. Bkz: [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) daha fazla bilgi için.

## <a name="classifying-personal-data"></a>Kişisel veri sınıflandırma
Güvenlik Merkezi'nin güvenlik kişi özelliğinde bulunan kişisel verileri sınıflandırmaya gerek yoktur. Kaydedilen veriler bir e-posta adresi (veya birden çok e-posta adresi) olduğu ve telefon numarası. [Kişi veri](security-center-provide-security-contact-details.md) Güvenlik Merkezi tarafından doğrulanır.

IP adreslerini sınıflandırmak ve bağlantı noktası numaraları Güvenlik Merkezi tarafından kaydedilen gerekmez [tam zamanında](security-center-just-in-time.md) özelliği.

Yönetici rolü atanmış bir kullanıcı yalnızca kişisel verileri göre sınıflandırabilirsiniz [uyarılarını görüntüleme](security-center-managing-and-responding-alerts.md) Güvenlik Merkezi'nde.

## <a name="securing-and-controlling-access-to-personal-data"></a>Güvenli hale getirme ve kişisel verilere erişimi denetleme
Okuyucu, sahibi, katkıda bulunan rolü atanmış bir güvenlik merkezi kullanıcı ya da Hesap Yöneticisi erişebilir [güvenlik kişi verilerini](security-center-provide-security-contact-details.md).

Okuyucu, sahibi, katkıda bulunan rolü atanmış bir güvenlik merkezi kullanıcı ya da Hesap Yöneticisi erişebilir kendi [tam zamanında](security-center-just-in-time.md) ilkeleri.

Okuyucu, sahibi, katkıda bulunan rolü atanmış bir güvenlik merkezi kullanıcı ya da Hesap Yöneticisi görüntüleyebilir, [uyarıları](security-center-managing-and-responding-alerts.md).

## <a name="updating-personal-data"></a>Kişisel verileri güncelleştirme
Hesap Yöneticisi güncelleştirebilir veya rol sahibinin katkıda bulunan, Güvenlik Merkezi kullanıcı atanmış [güvenlik kişi verilerini](security-center-provide-security-contact-details.md) Azure Portalı aracılığıyla.

Hesap Yöneticisi güncelleştirebilir veya rol sahibinin katkıda bulunan, Güvenlik Merkezi kullanıcı atanmış kendi [zaman ilkelerinde yalnızca](security-center-just-in-time.md).

Bir Hesap Yöneticisi, uyarı olayları düzenleyemezsiniz. Bir [uyarı olayı](security-center-managing-and-responding-alerts.md) security verisi olarak kabul edilir ve salt okunur.

## <a name="deleting-personal-data"></a>Kişisel verileri silme
Güvenlik Merkezi kullanıcı rol sahibinin katkıda bulunan, atanan veya Hesap Yöneticisi silebilirsiniz [güvenlik kişi verilerini](security-center-provide-security-contact-details.md) Azure Portalı aracılığıyla.

Güvenlik Merkezi kullanıcı rol sahibinin katkıda bulunan, atanan veya Hesap Yöneticisi silebilirsiniz [zaman ilkelerinde yalnızca](security-center-just-in-time.md) Azure Portalı aracılığıyla.

Güvenlik Merkezi kullanıcı uyarı olayları silinemiyor. Güvenlik gereksinimleri nedeniyle bir [uyarı olayı](security-center-managing-and-responding-alerts.md) okuma yalnızca verisi olarak kabul edilir.

## <a name="exporting-personal-data"></a>Kişisel verileri dışarı aktarma
Okuyucu, sahibi, katkıda bulunan rolü atanmış bir güvenlik merkezi kullanıcı ya da Hesap Yöneticisi verebilirsiniz [güvenlik kişi verilerini](security-center-provide-security-contact-details.md) göre:

- Azure portalından bir kopyasını gerçekleştirme
- Azure REST API çağrısı, GET HTTP yürütme:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/providers/Microsoft.Security/securityContacts?api-version={api-version}
```

Hesap Yöneticisi'nin rolü atanmış bir güvenlik merkezi kullanıcı verebilirsiniz [zaman ilkelerinde yalnızca](security-center-just-in-time.md) içeren IP adresleri:

- Azure portalından bir kopyasını gerçekleştirme
- Azure REST API çağrısı, GET HTTP yürütme:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Security/locations/{location}/jitNetworkAccessPolicies/default?api-version={api-version}
```

Bir hesap yöneticisi tarafından uyarı ayrıntılarını dışarı aktarabilirsiniz:

- Azure portalından bir kopyasını gerçekleştirme
- Azure REST API çağrısı, GET HTTP yürütme:
```HTTP
GET https://<endpoint>/subscriptions/{subscriptionId}/providers/microsoft.Security/alerts?api-version={api-version}
```

Bkz: [güvenlik uyarıları alın (GET toplama)](https://msdn.microsoft.com/library/mt704050.aspx) daha fazla bilgi için.

## <a name="restricting-the-use-of-personal-data-for-profiling-or-marketing-without-consent"></a>Profil oluşturma veya izniniz olmadan pazarlama kişisel verilerin kullanımını kısıtlama
Güvenlik Merkezi kullanıcı silerek çıkma seçebilir kendi [güvenlik kişi verilerini](security-center-provide-security-contact-details.md).

[Yalnızca zaman verilerdeki](security-center-just-in-time.md) tanımlanabilen olmayan verileri kabul edilir ve 30 gün boyunca tutulur.

[Uyarı veri](security-center-managing-and-responding-alerts.md) security verisi olarak kabul edilir ve iki yıl boyunca tutulur.

## <a name="auditing-and-reporting"></a>Denetim ve Raporlama
Tam zamanında denetim günlüklerini güvenlik iletişim ve güncelleştirmeleri korunur uyarı [Azure etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcı verileri yönetme hakkında daha fazla bilgi için bkz: [bir Azure Güvenlik Merkezi araştırma bulunan kullanıcı verilerini yönetmek](security-center-investigation-user-data.md).
