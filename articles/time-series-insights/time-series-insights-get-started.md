---
title: Azure Zaman Serisi Görüşleri ortamı oluşturma | Microsoft Docs
description: Bu makalede, Azure portalında yeni bir zaman serisi Öngörüler ortamı oluşturmak için nasıl kullanılacağını açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: ef5c194aa462a83cd982adab0a818f0aa095ffa0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654445"
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Azure Portal’da yeni Zaman Serisi Görüşleri ortamı oluşturma
Bu makalede Azure Portalı'nı kullanarak yeni bir zaman serisi Öngörüler ortamının nasıl oluşturulacağı açıklanmaktadır.

Zaman serisi Öngörüler saniye cinsinden sorgu büyük zaman serisi veri birimleri için etkinleştirme Görselleştirme ve Azure IOT hub ve Event Hubs halinde dakika cinsinden akan veri sorgulama başlama olanak tanır.  Nesnelerin interneti (IOT) ölçek için tasarlanmıştır ve terabayt veri işleyebilir.

## <a name="steps-to-create-the-environment"></a>Ortam oluşturma adımları
Bir ortam oluşturmak için aşağıdaki adımları izleyin:

1.  [Azure Portal](https://portal.azure.com) oturum açın.

2.  Seçin **+ yeni** düğmesi.

3.  Seçin **nesnelerin interneti** kategori ve select **zaman serisi Öngörüler**.

   ![Zaman Serisi Görüşleri oluşturma ortam](media/time-series-insights-get-started/1-new-tsi.png)

4.  Üzerinde **zaman serisi Öngörüler** sayfasında, **oluşturma**.

5. Gerekli parametre doldurun. Aşağıdaki tabloda her bir parametreyi açıklar:
   
   ![Zaman Serisi Görüşleri oluşturma kaynak grubu](media/time-series-insights-get-started/2-create-tsi.png)
   
   Ayar|Önerilen değer|Açıklama
   ---|---|---
   Ortam adı | Benzersiz bir ad | Bu ad ortamında temsil eden [zaman serisi Gezgini](https://insights.timeseries.azure.com)
   Abonelik | Aboneliğiniz | Birden çok aboneliğiniz varsa, olay kaynağı tercihen içeren abonelik seçin. Zaman Serisi Görüşleri aynı abonelikteki mevcut Azure IoT Hub ve Event Hub kaynaklarını otomatik olarak algılayabilir.
   Kaynak grubu | Yeni bir oluşturun veya var olanı kullan | Kaynak grubu, birlikte kullanılan Azure kaynakları koleksiyonudur. Varolan bir kaynak grubu, örneğin olay hub'ı veya IOT hub'ı içeren bir seçebilirsiniz. Veya bu kaynak için diğer kaynakları ilgili değilse yeni bir tane yapabilirsiniz.
   Konum | Olay kaynağı en yakın | Tercihen, olay kaynağı verilerinizi önlemek için çaba içinde içeren veri merkezi konumu çapraz bölge ve çapraz bölge bant genişliği maliyetlerini eklenir ve veri bölgesinin dışına taşırken gecikme eklenen seçin.
   Fiyatlandırma katmanı | S1 | Gerekli üretilen işi seçin. Düşük maliyetler ve Başlatıcı kapasite için S1 seçin.
   Kapasite | 1 | Kapasite, giriş oranı, depolama kapasitesi ve seçilen SKU ile ilişkili maliyet çarpanı uygulandığı ' dir.  Oluşturduktan sonra ortam kapasitesini değiştirebilirsiniz. Düşük maliyetler için 1 kapasitesini seçin. 
  
6. Denetleme **panoya Sabitle** en iyi kolayca zaman serisi ortamınızı gelecekte erişmek için.

   ![Zaman Serisi Görüşleri oluşturma panoya sabitleme](media/time-series-insights-get-started/3-pin-create.png)

7. Seçin **oluşturma** sağlama işlemini başlatmak için. Birkaç dakika sürebilir.

8. Dağıtım işlemi izlemek üzere seçmek **bildirimleri** simgesi (zil simgesine).

   ![Bildirimleri izleme](media/time-series-insights-get-started/4-notifications.png)

Dağıtım başarılı olduğunda seçebileceğiniz **kaynağa gidin** diğer özellikleri yapılandırmak için veri erişim ilkeleri ile güvenlik ayarlamak, olay kaynakları ve diğer eylemleri ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Veri erişim ilkeleri tanımlamanıza](time-series-insights-data-access.md) ortamınızın güvenliğini sağlamak için.
* [Bir olay hub'ı olay kaynağı Ekle](time-series-insights-how-to-add-an-event-source-eventhub.md) Azure zaman serisi Öngörüler ortamınıza. 
* [Olayları göndermek](time-series-insights-send-events.md) olay kaynağı.
* Ortamınızdaki görüntülemek [zaman serisi Öngörüler explorer](https://insights.timeseries.azure.com).
