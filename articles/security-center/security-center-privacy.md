---
title: Azure Güvenlik Merkezi'nde kullanıcı verilerini yönetme | Microsoft Docs
description: " Azure Güvenlik Merkezi'nde kullanıcı verilerini yönetmeyi öğrenin. "
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2018
ms.author: rkarlin
ms.openlocfilehash: fcec410df631a58b76878a4cb327ca2fb04a2105
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60703481"
---
# <a name="manage-user-data-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde kullanıcı verilerini yönetme
Bu makalede, Azure Güvenlik Merkezi kullanıcı verilerini nasıl yönetebileceğiniz hakkında bilgi sağlar. Kullanıcı verilerini yönetme erişimi, silmek veya verileri dışarı aktarma özelliğini içerir.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

Güvenlik Merkezi kullanıcı Okuyucu, sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi, araç içinde müşteri verilerine erişebilir. Bkz: [Azure rol tabanlı erişim denetimi için yerleşik roller](../role-based-access-control/built-in-roles.md) Okuyucu, sahibi ve katkıda bulunan rolleri hakkında daha fazla bilgi edinmek için. Bkz: [Azure aboneliği yöneticileri](../billing/billing-add-change-azure-subscription-administrator.md) hesap yöneticisi rolü hakkında daha fazla bilgi edinmek için.

## <a name="searching-for-and-identifying-personal-data"></a>Arama ve kişisel verileri tanımlama
Güvenlik Merkezi kullanıcı kişisel verilerini Azure portalından görüntüleyebilirsiniz. Güvenlik Merkezi, yalnızca e-posta adresi ve telefon numaraları gibi güvenlik kişi ayrıntılarını da depolar. Bkz: [Azure Güvenlik Merkezi'nde güvenlik kişi ayrıntılarını sağlama](security-center-provide-security-contact-details.md) daha fazla bilgi için.

Azure portalında bir kullanıcı yalnızca zaman VM erişim özelliği Güvenlik Merkezi'nin kullanarak izin verilen IP yapılandırmaları görüntüleyebilir. Daha fazla bilgi için bkz. [Tam zamanında özelliğini kullanarak sanal makine erişimini yönetme](security-center-just-in-time.md).

Azure portalında bir kullanıcı IP adreslerini ve saldırganın ayrıntılarını da dahil olmak üzere Güvenlik Merkezi tarafından sağlanan güvenlik uyarılarını görüntüleyebilirsiniz. Bkz: [yönetme ve Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md) daha fazla bilgi için.

## <a name="classifying-personal-data"></a>Kişisel veri sınıflandırma
Güvenlik Merkezi'nin güvenlik ilgili kişi özelliğinde bulunan kişisel verileri sınıflandırmak gerekmez. Kaydedilen veriler e-posta adresi (veya birden çok e-posta adresi) olduğu ve bir telefon numarası. [Kişi verileri](security-center-provide-security-contact-details.md) Güvenlik Merkezi tarafından doğrulanır.

IP adreslerini sınıflandırmak ve bağlantı noktası numaraları Güvenlik Merkezi tarafından kaydedilen gerekmez [zamanında](security-center-just-in-time.md) özelliği.

Bir kullanıcıya yönetici rolü atanan yalnızca kişisel verileri göre sınıflandırabilirsiniz [uyarılarını görüntüleme](security-center-managing-and-responding-alerts.md) Güvenlik Merkezi'nde.

## <a name="securing-and-controlling-access-to-personal-data"></a>Güvenliğini sağlama ve kişisel verilere erişimi denetleme
Güvenlik Merkezi kullanıcı Okuyucu, sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi erişim [güvenlik ilgili kişi verilerini](security-center-provide-security-contact-details.md).

Güvenlik Merkezi kullanıcı Okuyucu, sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi erişim kendi [zamanında](security-center-just-in-time.md) ilkeleri.

Güvenlik Merkezi kullanıcı Okuyucu, sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi görüntüleyebilir, [uyarılar](security-center-managing-and-responding-alerts.md).

## <a name="updating-personal-data"></a>Kişisel verileri güncelleştirme
Güvenlik Merkezi kullanıcı sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi güncelleştirebilir [güvenlik ilgili kişi verilerini](security-center-provide-security-contact-details.md) Azure portal aracılığıyla.

Güvenlik Merkezi kullanıcı sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi güncelleştirebilir, [yalnızca zaman ilkelerinde](security-center-just-in-time.md).

Bir Hesap Yöneticisi, uyarı olayları düzenleyemezsiniz. Bir [uyarı olayı](security-center-managing-and-responding-alerts.md) güvenlik verileri olarak kabul edilir ve salt okunur.

## <a name="deleting-personal-data"></a>Kişisel verileri silme
Güvenlik Merkezi kullanıcı sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi silebilirsiniz [güvenlik ilgili kişi verilerini](security-center-provide-security-contact-details.md) Azure portal aracılığıyla.

Güvenlik Merkezi kullanıcı sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi silebilirsiniz [yalnızca zaman ilkelerinde](security-center-just-in-time.md) Azure portal aracılığıyla.

Güvenlik Merkezi kullanıcı uyarı olayları nelze odstranit. Güvenlik ihtiyaçlarını temel alarak nedeniyle bir [uyarı olayı](security-center-managing-and-responding-alerts.md) yalnızca veri okuma olarak kabul edilir.

## <a name="exporting-personal-data"></a>Kişisel verileri dışarı aktarma
Güvenlik Merkezi kullanıcı Okuyucu, sahibi, katkıda bulunan rolü atanmış veya Hesap Yöneticisi dışarı aktarabilir [güvenlik ilgili kişi verilerini](security-center-provide-security-contact-details.md) tarafından:

- Azure portalından bir kopyasını gerçekleştirme
- Azure REST API çağrısı, GET HTTP yürütülüyor:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/Microsoft.Security/securityContacts?api-version={api-version}
  ```

Hesap Yöneticisi, rolü atanmış bir güvenlik merkezi kullanıcı verebilirsiniz [yalnızca zaman ilkelerinde](security-center-just-in-time.md) içeren IP adresleri tarafından:

- Azure portalından bir kopyasını gerçekleştirme
- Azure REST API çağrısı, GET HTTP yürütülüyor:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Security/locations/{location}/jitNetworkAccessPolicies/default?api-version={api-version}
  ```

Bir hesap yöneticisi tarafından uyarı ayrıntılarını dışarı aktarabilirsiniz:

- Azure portalından bir kopyasını gerçekleştirme
- Azure REST API çağrısı, GET HTTP yürütülüyor:
  ```HTTP
  GET https://<endpoint>/subscriptions/{subscriptionId}/providers/microsoft.Security/alerts?api-version={api-version}
  ```

Bkz: [güvenlik uyarıları alın (alma koleksiyonu)](https://msdn.microsoft.com/library/mt704050.aspx) daha fazla bilgi için.

## <a name="restricting-the-use-of-personal-data-for-profiling-or-marketing-without-consent"></a>Profil oluşturma veya izniniz olmadan pazarlama için kişisel verilerin kullanımı kısıtlama
Güvenlik Merkezi kullanıcı silerek geri çevirmek seçebilirsiniz, [güvenlik ilgili kişi verilerini](security-center-provide-security-contact-details.md).

[Yalnızca zaman verilerindeki](security-center-just-in-time.md) tanımlanabilen olmayan verileri kabul edilir ve 30 gün boyunca tutulur.

[Uyarı veri](security-center-managing-and-responding-alerts.md) güvenlik verileri olarak kabul edilir ve iki yıllık bir dönem için korunur.

## <a name="auditing-and-reporting"></a>Denetim ve Raporlama
Denetim Günlükleri güvenlik iletişim, tam zamanında ve güncelleştirmeleri tutulur uyarı [Azure etkinlik günlüklerini](../azure-monitor/platform/activity-logs-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcı verileri yönetme hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi soruşturma bulunan kullanıcı verilerini yönetme](security-center-investigation-user-data.md).
