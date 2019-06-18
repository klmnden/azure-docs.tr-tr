---
title: Kotalar ve sınırlar içinde Azure Data Lake Analytics ayarlama
description: Ayarlama ve kotalar ve sınırlar Azure Data Lake Analytics (ADLA) hesaplarında artırma hakkında bilgi edinin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: omidm1
ms.author: omidm
ms.reviewer: jasonwhowell
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: d3601fd8c32c70cf828cd08fada71258ec8fa5d4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60812659"
---
# <a name="adjust-quotas-and-limits-in-azure-data-lake-analytics"></a>Kotalar ve sınırlar içinde Azure Data Lake Analytics ayarlama

Ayarlama ve Azure Data Lake Analytics (ADLA) hesaplarında sınırı ve kota artırma hakkında bilgi edinin. Bu sınırlar bilerek, U-SQL işi davranışını anlamanıza yardımcı olabilir. Tüm kota sınırları yazılım, olduğundan Azure desteğine başvurularak sınırlarını artırabilirsiniz.

## <a name="azure-subscriptions-limits"></a>Azure abonelik limitleri

**ADLA hesapları bölge başına abonelik başına en fazla sayısı:**  5

Altıncı bir ADLA hesabı oluşturmayı denerseniz, ", Data Lake Analytics hesapları (5) izin verilen en fazla sayısı abonelik adı altındaki bölgede ulaştığınız" bir hata alırsınız. 

Bu sınırı aşan gitmek isterseniz, bu seçenekler deneyebilirsiniz:
* uygun değilse başka bir bölge seçin
* Azure desteğine başvurun tarafından [bir destek çağrısının açılmasını](#increase-maximum-quota-limits) bir kota artırım talebinde bulunmak.

## <a name="default-adla-account-limits"></a>Varsayılan ADLA hesabı sınırları

**Hesap başına en fazla sayı Analytics birimi (AU):** 32

Hesabınızda eş zamanlı olarak çalışabilen AU sayısı budur. AU çalışan tüm işler arasında toplam sayısı bu sınırı aşarsa, yeni işleri otomatik olarak kuyruğa alınır. Örneğin:

* Yalnızca bir iş varsa 32 ile ikinci gönderdiğinizde AU, işi çalıştırılırken ilk iş tamamlanana kadar iş kuyruğunda bekler.
* Zaten sahip olduğunuz çalışan dört işler ve her 8 kullanılıyorsa 8 gereken beşinci bir iş gönderdiğinizde Avustralya, AU 8 kadar iş kuyruğunda bekler Au'lar kullanılabilir.

**İş başına en fazla sayı Analytics birimi (AU):** 32

Bu, her bir iş hesabınızda atanabilecek AU'ların sayısını varsayılan en yüksek sayıdır. Gönderen İş başına daha fazla AU sağlayan bir işlem İlkesi (iş gönderme sınırı) tarafından etkilenen sürece, bu sınır birden fazla atanan işleri reddedilir. AU sınırı hesabı için bu değeri üst sınırıdır.

**Hesap başına eşzamanlı U-SQL işi sayısı üst sınırı:** 20

Hesabınızda eş zamanlı olarak çalışabilen işleri sayısının budur. Bu değer yeni işleri otomatik olarak kuyruğa alınır.

## <a name="adjust-adla-account-limits"></a>ADLA hesabı sınırlarını ayarlama

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Varolan ADLA hesabı seçin.
3. **Özellikler**'e tıklayın.
4. Değerleri ayarlama **maksimum AUs**, **çalışan iş sayısının en fazla number**, ve **iş gönderme sınırları** gereksinimlerinize uyacak şekilde.

## <a name="increase-maximum-quota-limits"></a>En yüksek kota sınırları artırın

Azure limitleri hakkında daha fazla bilgi bulabilirsiniz, [Azure hizmete özgü belgeleri limitleri](../azure-subscription-service-limits.md#data-lake-analytics-limits).

1. Azure portalında bir destek isteği açın.

    ![Azure Data Lake Analytics portalı sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Data Lake Analytics portalı sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Sorun türü seçin **kota**.
3. Seçin, **abonelik** (bir "Deneme" aboneliği olmadığından emin olun).
4. Kota türü seçin **Data Lake Analytics**.

    ![Azure Data Lake Analytics portalı sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. Sorun sayfasında, istenen artış sınırınızı açıklayan **ayrıntıları** , neden bu ek kapasite gerekir.

    ![Azure Data Lake Analytics portalı sayfası](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. İletişim bilgilerinizi doğrulayın ve destek isteği oluşturun.

Microsoft, isteğinizi inceleyene ve iş gereksinimlerinize en kısa zamanda uyum sağlayacak şekilde çalışır.

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics'i Azure PowerShell'i kullanarak yönetme](data-lake-analytics-manage-use-powershell.md)
* [Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme ve sorun giderme](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
