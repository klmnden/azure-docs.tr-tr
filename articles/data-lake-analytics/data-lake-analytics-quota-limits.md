---
title: Azure Data Lake Analytics kota sınırları
description: Ayarlama ve Azure Data Lake Analytics (ADLA) hesaplarındaki kota sınırları artırmak hakkında bilgi edinin.
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: ''
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.topic: article
ms.workload: big-data
ms.date: 03/15/2018
ms.author: omidm
ms.openlocfilehash: 4334a438f09d7c18912262e9c70bfffbcdeb1d9e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="azure-data-lake-analytics-quota-limits"></a>Azure Data Lake Analytics kota sınırları

Ayarlama ve Azure Data Lake Analytics (ADLA) hesaplarındaki kota sınırları artırmak hakkında bilgi edinin. Bu sınırlar bilerek, U-SQL işi davranışı anlamanıza yardımcı olabilir. Tüm kota sınırları yumuşak, olduğundan maksimum sınırları Azure desteğine başvurarak artırabilirsiniz.

## <a name="azure-subscriptions-limits"></a>Azure abonelikleri sınırları

**Sayısının ADLA hesapları her bölge abonelik başına:** 5

Altıncı bir ADLA hesabı oluşturmayı denerseniz, "en fazla izin Data Lake Analytics hesabı sayısını (5) altında abonelik adını bölgesindeki ulaştığınız" bir hata alırsınız. 

Bu sınırı aşan gitmek isterseniz, bu seçenekleri deneyebilirsiniz:
* uygun başka bir bölge seçin
* Azure desteğine başvurun tarafından [bir destek bileti açmadan](#increase-maximum-quota-limits) kota artışı isteği göndermek üzere.

## <a name="default-adla-account-limits"></a>Varsayılan ADLA hesabı sınırları

**Hesap başına en fazla Analytics birimi (Avustralya) sayısı:** 32

Hesabınızı çalışabilir Avustralya üst sınırını budur. Avustralya çalışan tüm işler arasında toplam sayısı bu sınırı aşarsa, yeni işleri otomatik olarak kuyruğa alınır. Örneğin:

* Yalnızca bir işe varsa ile 32 saniyenin gönderdiğinizde Avustralya, işi çalıştırılırken iş kuyruğundaki ilk iş tamamlanana kadar bekler.
* Çalışan dört işleri zaten varsa ve her 8 kullanarak 8 gereken beşinci bir işi gönderdiğinizde Avustralya 8 kadar iş kuyruğundaki bekler Avustralya Avustralya kullanılabilir.

**İş başına en fazla sayısını Analytics birimler (Avustralya):** 32

Bu varsayılan en büyük tek tek her iş hesabınızda atanabilir Avustralya sayısıdır. Gönderenin (job) başına daha fazla Avustralya verir işlem İlkesi (iş gönderim sınırı) tarafından etkilenen sürece bu sınırı birden fazla atanmış olan işleri reddedilir. AU sınırı hesabı için bu değeri üst sınırıdır.

**Hesap başına eşzamanlı U-SQL işi sayısı üst sınırı:** 20

Maksimum sayıda hesabınızda çalışabilir işi budur. Bu değer yeni işleri otomatik olarak kuyruğa alınır.

## <a name="adjust-adla-account-limits"></a>ADLA hesabı sınırları ayarlama

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Varolan ADLA hesabı seçin.
3. **Özellikler**'e tıklayın.
4. Değerleri ayarlama **maksimum Avustralya**, **en fazla sayıda çalışan iş**, ve **iş gönderim sınırları** gereksinimlerinize uygun olarak.

## <a name="increase-maximum-quota-limits"></a>En yüksek kota sınırları artırmak

Azure sınırları hakkında daha fazla bilgi bulabilirsiniz içinde [Azure hizmete özgü belgeleri sınırlar](../azure-subscription-service-limits.md#data-lake-analytics-limits).

1. Azure portalında bir destek isteği açın.

    ![Azure Data Lake Analytics portal sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Data Lake Analytics portal sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Sorun türü seçin **kota**.
3. Seçin, **abonelik** ("Deneme" aboneliği olmadığından emin olun).
4. Kota türü seçin **Data Lake Analytics**.

    ![Azure Data Lake Analytics portal sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. Sorun sayfasında, istenen artış sınırınızı açıklayan **ayrıntıları** bu ek kapasite neden ihtiyacınız olan.

    ![Azure Data Lake Analytics portal sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. İletişim bilgilerinizi doğrulayın ve destek isteği oluşturun.

Microsoft, isteğinizi gözden geçirir ve mümkün olan en kısa sürede iş gereksinimlerinizi karşılamak çalışır.

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme](data-lake-analytics-manage-use-powershell.md)
* [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
