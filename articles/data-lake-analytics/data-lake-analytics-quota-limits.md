---
title: "Azure Data Lake Analytics kota sınırları | Microsoft Docs"
description: "Ayarlama ve Azure Data Lake Analytics (ADLA) hesaplarındaki kota sınırları artırmak hakkında bilgi edinin."
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: omidm
ms.openlocfilehash: 957f306ea0e80b5830ad64e5ef06c6d122d9eccc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a>Azure Data Lake Analytics kota sınırları

Ayarlama ve Azure Data Lake Analytics (ADLA) hesaplarındaki kota sınırları artırmak hakkında bilgi edinin. Bu sınırlar bilerek, U-SQL işi davranışı anlamanıza yardımcı olabilir. Bize ulaşmasını tarafından maksimum sınırları artırmak için tüm kota sınırları yumuşak,.

## <a name="azure-subscriptions-limits"></a>Azure abonelikleri sınırları

**ADLA en fazla abonelik başına hesapları:** 5

 Bu ADLA hesapları abonelik başına oluşturabileceğiniz en fazla sayısıdır. Altıncı bir ADLA hesabı oluşturmayı denerseniz, "en fazla izin Data Lake Analytics hesabı sayısını (5) altında abonelik adını bölgesindeki ulaştığınız" bir hata alırsınız. Bu durumda, tüm kullanılmayan ADLA hesapları silmek ya da tarafımızca ulaşmak [bir destek bileti açmadan](#increase-maximum-quota-limits).

## <a name="adla-account-limits"></a>ADLA hesabı sınırları

**Hesap başına en fazla Analytics birimi (Avustralya) sayısı:** 250

Hesabınızı çalışabilir Avustralya üst sınırını budur. Avustralya toplam tüm işleri arasında bu sınırı aşarsa, yeni işleri otomatik olarak kuyruğa alınır. Örneğin:

* Yalnızca bir işe varsa 250 ile ikinci bir gönderdiğinizde Avustralya, işi çalıştırılırken iş kuyruğundaki ilk iş tamamlanana kadar bekler.
* Çalışan beş işleri zaten varsa ve her 50 kullanarak 20 gereken altıncı bir işi gönderdiğinizde Avustralya kalmayana kadar 20 iş kuyruğundaki bekler Avustralya Avustralya kullanılabilir.

**Hesap başına eşzamanlı U-SQL işi sayısı üst sınırı:** 20

Maksimum sayıda hesabınızda çalışabilir işi budur. Bu değer yeni işleri otomatik olarak kuyruğa alınır.

## <a name="adjust-adla-quota-limits-per-account"></a>Hesap başına ADLA kota sınırları ayarlama

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Varolan ADLA hesabı seçin.
3. **Özellikler**'e tıklayın.
4. Ayarlama **paralellik** ve **eşzamanlı iş** gereksinimlerinize uygun olarak.

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a>En yüksek kota sınırları artırmak

1. Azure portalında bir destek isteği açın.

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Sorun türü seçin **kota**.
3. Seçin, **abonelik** ("Deneme" aboneliği olmadığından emin olun).
4. Kota türü seçin **Data Lake Analytics**.

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. Sorun dikey penceresinde istenen artış sınırınızı açıklayan **ayrıntıları** bu ek kapasite neden ihtiyacınız olan.

    ![Azure Data Lake Analytics portalı dikey penceresi](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. İletişim bilgilerinizi doğrulayın ve destek isteği oluşturun.

Microsoft, isteğinizi gözden geçirir ve mümkün olan en kısa sürede iş gereksinimlerinizi karşılamak çalışır.

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme](data-lake-analytics-manage-use-powershell.md)
* [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
