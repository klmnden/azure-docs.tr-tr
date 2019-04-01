---
title: Sağlama ve Azure zaman serisi önizlemesi yönetme | Microsoft Docs
description: Sağlama ve Azure zaman serisi öngörüleri önizlemesi yönetme anlama.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 85d5bb822bc9b89c68c70633a22a1bed74118f49
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758180"
---
# <a name="provision-and-manage-azure-time-series-insights-preview"></a>Sağlama ve Azure zaman serisi öngörüleri önizlemesi yönetme

Bu makalede oluşturup kullanarak Azure zaman serisi öngörüleri Önizleme ortamı yönetmek nasıl [Azure portalında](https://portal.azure.com/).

## <a name="overview"></a>Genel Bakış

Azure zaman serisi öngörüleri Önizleme ortamlarında Kullandıkça Öde (PAYG) ortamları ' dir.

Azure zaman serisi öngörüleri Önizleme ortamı sağladığınızda, iki Azure kaynaklarını oluşturun:

* Azure zaman serisi öngörüleri Önizleme ortamı  
* Bir Azure depolama genel amaçlı v1 hesabı
  
Bilgi [ortamınızı planlama nasıl](./time-series-insights-update-plan.md).

>[!IMPORTANT]
> Önizleme için bir Azure depolama genel amaçlı v1 kullandığınızdan emin olun (GPv1) hesabı.  GPv2 ve daha sonra destek yakın gelecekte eklenecektir.  

İsteğe bağlı olarak, her bir Azure zaman serisi öngörüleri Önizleme ortamı bir olay kaynağı ile ilişkilendirebilirsiniz. Daha fazla bilgi için okuma [hub olay kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md) ve [bir IOT hub'ı kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md). Bu adım sırasında bir zaman damgası kimliği özelliği ve benzersiz bir tüketici grubu sağlayın. Bunun yapılması, ortam uygun etkinliklerine erişime sahip olmasını sağlar.

Sağlama tamamlandıktan sonra erişim ilkelerinizi ve işletme gereksinimlerinize uyacak şekilde diğer ortam özniteliklerini değiştirebilirsiniz.

## <a name="create-the-environment"></a>Ortam oluşturma

Aşağıdaki adımlar bir Azure zaman serisi öngörüleri önizlemesi ortamının nasıl oluşturulacağı açıklanmaktadır:

1. Seçin **PAYG** düğmesini **SKU** menüsü. Bir ortam adı sağlayın ve hangi Abonelik grubu ve kaynak grubunu kullanmayı seçin. Ardından barındırılması için ortamı için desteklenen bir konum seçin.

   ![Azure Time Series Insights örneği oluşturun.][1]

1. Zaman serisi girin kimliği

    >[!NOTE]
    > * Zaman serisi kimliği büyük/küçük harfe ve değişmez. (Ayarlandıktan sonra değiştirilemez.)
    > * Zaman serisi kimlikleri en fazla üç anahtarları olabilir.
    > * Zaman serisi kimliği seçme hakkında daha fazla bilgi için okuma [bir zaman serisi kimliği seçin](./time-series-insights-update-how-to-id.md).

1. Azure depolama hesabınız depolama hesabı adı seçerek ve bir çoğaltma seçeneği belirleme oluşturun. Otomatik olarak yapılması, bir Azure depolama genel amaçlı v1 hesabı oluşturur. Daha önce seçtiğiniz Azure zaman serisi öngörüleri Önizleme ortamı ile aynı bölgede oluşturulur.

    ![Örneğiniz için bir Azure depolama hesabı oluşturma][5]

1. İsteğe bağlı olarak, bir olay kaynağı ekleyebilirsiniz.

   * Zaman serisi görüşleri destekler [Azure IOT hub'ı](./time-series-insights-how-to-add-an-event-source-iothub.md) ve [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) seçenekleri. Ortam oluşturma sırasında yalnızca tek bir olay kaynağı ekleyebilirsiniz, ancak başka bir olay kaynağı daha sonra ekleyebilirsiniz. Tüm olayları Azure zaman serisi öngörüleri önizlemesi Örneğinize görünür olmasını sağlamak için benzersiz bir tüketici grubu oluşturmak idealdir. Varolan bir tüketici grubu seçin veya olay kaynağı eklerken, yeni bir tüketici grubu oluşturun.

   * Ayrıca, uygun zaman damgası özelliği de seçmeniz gerekir. Varsayılan olarak, Azure zaman serisi görüşleri her bir olay kaynağı için ileti sıraya süresi kullanır.

     > [!TIP]
     > İleti sıraya süresi, toplu iş olayı veya geçmiş verileri karşıya yükleme senaryoları kullanmak için en iyi şekilde yapılandırılan bir ayarı olmayabilir. Kullanacağınıza karar kullanın veya bir zaman damgası özelliği, bu gibi durumlarda kullanmamanız doğruladığınızdan emin olun.

     ![Olay kaynağı sekmesi][2]

1. Ortamınızı istenen ayarlarla sağlandığını doğrulayın.

    ![Gözden geçir + sekme oluşturma][3]

## <a name="manage-the-environment"></a>Ortam yönetme

Azure portalını kullanarak Azure zaman serisi öngörüleri Önizleme ortamınıza yönetebilirsiniz. PAYG Azure zaman serisi öngörüleri Önizleme ortamı, bir S1 yerine veya S2 ortamı, Azure portalını kullanarak yönetmedeki başlıca farklar şunlardır:

* Azure portalının **genel bakış** dikey aynıdır Azure zaman serisi Öngörülerinde dışında aşağıdaki yollarla:
  * Bu kavramı PAYG ortamları için uygun olmadığı için kapasite kaldırılır.
  * Zaman serisi kimliği özelliği eklenmiştir. Verilerinizi bölümlere nasıl belirler.
  * Başvuru veri kümelerini kaldırılır.
  * Görüntülenen URL, yönlendirir [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md).
  * Azure depolama hesabınızın adını listelenir.

* Azure portalının **yapılandırma** dikey PAYG ortamları yapılandırılabilir değildir çünkü Azure zaman serisi öngörüleri önizlemesinde kaldırıldı.

* Azure portalının **başvuru verileri** dikey başvuru verileri bir bileşen PAYG ortamların olmadığından Azure zaman serisi öngörüleri önizlemesinde kaldırıldı.

![Azure portalında zaman serisi öngörüleri Önizleme ortamı][4]

## <a name="next-steps"></a>Sonraki adımlar

Okuma [ortamınızı planlama](./time-series-insights-update-plan.md).

Okuma [hub olay kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md).

Okuma [bir IOT hub'ı kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md).

<!-- Images -->
[1]: media/v2-update-manage/manage_one.PNG
[2]: media/v2-update-manage/manage_two.PNG
[3]: media/v2-update-manage/manage_three.PNG
[4]: media/v2-update-manage/manage_four.PNG
[5]: media/v2-update-manage/manage_five.PNG
