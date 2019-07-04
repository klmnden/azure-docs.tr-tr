---
title: Sağlama ve Azure zaman serisi önizlemesi yönetme | Microsoft Docs
description: Sağlama ve Azure zaman serisi öngörüleri önizlemesi yönetme anlama.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/26/2019
ms.custom: seodec18
ms.openlocfilehash: f626ce2e009a18afcb4d04b7caa6850ea58c7483
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446824"
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
> Önizleme için bir Azure depolama genel amaçlı v1 kullandığınızdan emin olun (GPv1) hesabı.

İsteğe bağlı olarak, her bir Azure zaman serisi öngörüleri Önizleme ortamı bir olay kaynağı ile ilişkilendirebilirsiniz. Daha fazla bilgi için okuma [hub olay kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md) ve [bir IOT hub'ı kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md). Bu adım sırasında bir zaman damgası kimliği özelliği ve benzersiz bir tüketici grubu sağlayın. Bunun yapılması, ortam uygun etkinliklerine erişime sahip olmasını sağlar.

Sağlama tamamlandıktan sonra erişim ilkelerinizi ve işletme gereksinimlerinize uyacak şekilde diğer ortam özniteliklerini değiştirebilirsiniz.

## <a name="create-the-environment"></a>Ortam oluşturma

Aşağıdaki adımlar bir Azure zaman serisi öngörüleri önizlemesi ortamının nasıl oluşturulacağı açıklanmaktadır:

1. Seçin **PAYG** düğmesini **SKU** menüsü. Bir ortam adı sağlayın ve hangi Abonelik grubu ve kaynak grubunu kullanmayı seçin. Ardından barındırılması için ortamı için desteklenen bir konum seçin.

   [![Azure Time Series Insights örneği oluşturun.](media/v2-update-manage/manage-three.png)](media/v2-update-manage/manage-three.png#lightbox)

1. Zaman serisi girin kimliği

    >[!NOTE]
    > * Zaman serisi kimliği büyük/küçük harfe ve değişmez. (Ayarlandıktan sonra değiştirilemez.)
    > * Zaman serisi kimlikleri en fazla üç anahtarları olabilir.
    > * Zaman serisi kimliği seçme hakkında daha fazla bilgi için okuma [bir zaman serisi kimliği seçin](./time-series-insights-update-how-to-id.md).

1. Azure depolama hesabınız depolama hesabı adı seçerek ve bir çoğaltma seçeneği belirleme oluşturun. Otomatik olarak yapılması, bir Azure depolama genel amaçlı v1 hesabı oluşturur. Daha önce seçtiğiniz Azure zaman serisi öngörüleri Önizleme ortamı ile aynı bölgede oluşturulur.

    [![Örneğiniz için bir Azure depolama hesabı oluşturma](media/v2-update-manage/manage-five.png)](media/v2-update-manage/manage-five.png#lightbox)

1. İsteğe bağlı olarak, bir olay kaynağı ekleyebilirsiniz.

   * Zaman serisi görüşleri destekler [Azure IOT hub'ı](./time-series-insights-how-to-add-an-event-source-iothub.md) ve [Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) seçenekleri. Ortam oluşturma sırasında yalnızca tek bir olay kaynağı ekleyebilirsiniz, ancak başka bir olay kaynağı daha sonra ekleyebilirsiniz. Tüm olayları Azure zaman serisi öngörüleri önizlemesi Örneğinize görünür olmasını sağlamak için benzersiz bir tüketici grubu oluşturmak idealdir. Varolan bir tüketici grubu seçin veya olay kaynağı eklerken, yeni bir tüketici grubu oluşturun.

   * Ayrıca, uygun zaman damgası özelliği de seçmeniz gerekir. Varsayılan olarak, Azure zaman serisi görüşleri her bir olay kaynağı için ileti sıraya süresi kullanır.

     > [!TIP]
     > İleti sıraya süresi, toplu iş olayı veya geçmiş verileri karşıya yükleme senaryoları kullanmak için en iyi şekilde yapılandırılan bir ayarı olmayabilir. Kullanacağınıza karar kullanın veya bir zaman damgası özelliği, bu gibi durumlarda kullanmamanız doğruladığınızdan emin olun.

     [![Olay kaynağı sekmesi](media/v2-update-manage/manage-two.png)](media/v2-update-manage/manage-two.png#lightbox)

1. Ortamınızı istenen ayarlarla sağlandığını doğrulayın.

    [![Gözden geçir + sekme oluşturma](media/v2-update-manage/manage-three.png)](media/v2-update-manage/manage-three.png#lightbox)

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

[![Azure portalında zaman serisi öngörüleri Önizleme ortamı](media/v2-update-manage/manage-four.png)](media/v2-update-manage/manage-four.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- Okuma [ortamınızı planlama](./time-series-insights-update-plan.md).

- Bilgi edinmek için nasıl [hub olay kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md).

- Yapılandırma [bir IOT hub'ı kaynak](./time-series-insights-how-to-add-an-event-source-iothub.md).