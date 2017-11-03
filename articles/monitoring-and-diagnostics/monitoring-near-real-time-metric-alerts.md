---
title: "Yakın gerçek zamanlı ölçüm uyarıları Azure İzleyicisi'nde | Microsoft Docs"
description: "Gerçek zamanlı ölçüm uyarıları Azure kaynak ölçümleri 1 dak sıklıkta izlemenize olanak tanır."
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: aeeb6c2fb87e6c19991ef243ee7230f4e8f4e251
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="near-real-time-metric-alerts-preview"></a>Gerçek zamanlı ölçüm uyarıları (Önizleme)
Azure İzleyici artık ölçüm uyarıları yakın gerçek zamanlı ölçüm uyarıları (Önizleme) olarak adlandırılan yeni bir türünü destekler. Bu özellik şu anda genel önizlemede değil.
Bu uyarılar birkaç yolla normal ölçüm uyarıları farklı

- **Gecikme geliştirilmiş** -gerçek zamanlı ölçüm uyarılar ölçüm değerleri değişiklikleri 1 dak olan en kısa sürede izleyebilirsiniz.
- **Ölçüm koşullar hakkında daha fazla denetime** -gerçek zamanlı ölçüm uyarıları daha zengin uyarı kurallarını tanımlamak kullanıcıların. Uyarıları artık ölçümleri maksimum, minimum, ortalama ve toplam değerler izleme destekler.
- **Birden çok ölçümlerini izleme birleştirilmiş** -gerçek zamanlı ölçüm birden çok ölçümleri (şu anda iki) tek bir kural ile uyarıları izleyebilirsiniz. Her iki ölçümleri belirtilen zaman aralığı için kendi ilgili eşiklerini ihlal, uyarıyı tetikleyen.
- **Modüler bildirim sistemi** - yakın gerçek zamanlı ölçüm uyarıları kullanım [Eylem grupları](monitoring-action-groups.md). Bu işlev kullanıcıların Eylemler modüler bir şekilde oluşturmak ve bunları çok sayıda uyarı kuralları için yeniden izin verir.

> [!NOTE]
> Gerçek zamanlı ölçüm uyarıları özelliği şu anda genel önizlemede değil. İşlevsellik ve kullanıcı deneyimi değiştirilebilir ' dir.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Yakın gerçek zamanlı ölçüm uyarılar için hangi kaynakların oluşturabilir miyim?
Gerçek zamanlı ölçüm uyarıları tarafından desteklenen kaynak türlerinin tam listesi:

* Microsoft.ApiManagement/service
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Sql/servers/elasticpools
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.Timeseriesinsights
* Microsoft.CognitiveServices/accounts


## <a name="create-a-near-real-time-metric-alert"></a>Yakın gerçek zamanlı ölçüm uyarısı oluştur
Şu anda gerçek zamanlı ölçüm uyarıları yalnızca Azure portalı üzerinden oluşturulabilir. PowerShell, komut satırı arabirimi (CLI) ve Azure İzleyici REST API'si ile gerçek zamanlı ölçüm uyarı yakın yapılandırma desteği yakında geliyor.

1. İçinde [portal](https://portal.azure.com/), izleme ilgilenen kaynak bulup seçin. Bu kaynak, listelenen kaynak türlerden birinde olmalıdır [önceki bölümde](#what-resources-can-i-create-near-real-time-metric-alerts-for). Ayrıca aynı tüm desteklenen kaynak türleri için merkezi olarak İzleyicisi'nden yapabilirsiniz > Uyarılar.

2. Seçin **uyarıları** veya **uyarı kuralları** izleme bölümünde. Metin ve simge farklı kaynaklar için biraz değişebilir.
   ![İzleme](./media/insights-alerts-portal/AlertRulesButton.png)

3. Tıklatın **yakın gerçek zamanlı ölçümleri uyarı (Önizleme) Ekle** komutu. Gri komutu, kaynak filtrede seçili olduğundan emin olun.

    ![Gerçek zamanlı ölçümleri uyarı düğme ekleme](./media/monitoring-near-real-time-metric-alerts/AddNRTAlertButton.png)

4. **Ad** , uyarı kuralı ve seçin bir **açıklama**, bildirim e-postalarda da gösterir.
5. Seçin **ölçüm** izlemek ve ardından istediğiniz bir **koşulu**, **zaman toplama**, ve **eşik** ölçüm için değer. İsteğe bağlı olarak başka bir tane seçin **ölçüm** izlemek ve ardından istediğiniz bir **koşulu**, **zaman toplama**, ve **eşik** değeri İkinci ölçüm. 

    ![Gerçek zamanlı ölçümleri Alert1 ekleme](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert1.png) ![gerçek zamanlı ölçümleri Alert2 Ekle](./media/monitoring-near-real-time-metric-alerts/AddNRTAlert2.png)
6. Seçin **süresi** ölçüm kuralları uyarı Tetikleyicileri önce karşılanması gereken süre. Dolayısıyla örneğin dönem "üzerinden son 5 dakika" ve uyarı görünür CPU % 80 (ve NetworkIn 500 MB yukarıda) yukarıdaki kullanırsanız, CPU % 80 5 dakika boyunca sürekli olarak yukarıdaki kaldırıldığında uyarı tetikler. İlk tetikleyici oluşur sonra 5 dakika boyunca % 80 CPU kaldığında oluşan yeniden tetikler. Uyarı göre değerlendirilir **değerlendirme sıklığı**


6. Uygun bir çekme **önem** açılır.

7. Yeni veya varolan kullanmak isteyip istemediğinizi belirtin **eylem grubu**.

8. Oluşturmayı seçerseniz **yeni** eylem grubu eylem grubuna bir ad ve kısa bir ad verin, eylemleri (SMS, e-posta, Web kancası) belirtin ve ilgili ayrıntıları doldurun.


8. Seçin **Tamam** uyarı oluşturmak için yapıldığında.   

Birkaç dakika içinde uyarı etkindir ve daha önce açıklandığı şekilde tetikler.

## <a name="managing-near-real-time-metric-alerts"></a>Gerçek zamanlı ölçüm Uyarıları yönetme
Bir uyarı oluşturduktan sonra bunu seçebilirsiniz ve:

* Ölçüm eşiği ve önceki gün gerçek değerleri gösteren bir grafik görüntüleyin.
* Düzenleyin veya silin.
* **Devre dışı** veya **etkinleştirmek** , geçici olarak durdurmak veya bu uyarı için bildirimleri almaya devam etmek istiyorsanız.



