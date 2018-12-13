---
title: Azure zaman serisi Önizleme Yönetimi - sağlamak ve Azure zaman serisi Önizleme yönetme. | Microsoft Docs
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
ms.openlocfilehash: 1834a59ff675f109267a406e3c2b03411e3bb50b
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53273526"
---
# <a name="how-to-provision-and-manage-azure-time-series-insights-preview"></a>Sağlama ve Azure zaman serisi öngörüleri önizlemesi yönetme

Bu belge, oluşturma ve kullanarak bir Azure zaman serisi öngörüleri Önizleme ortamı yönetmek açıklar [Azure portalında](https://portal.azure.com/).

## <a name="overview"></a>Genel Bakış

Azure zaman serisi öngörüleri Önizleme ortamı sağlamasını yapma hakkında kısa bir genel bakış aşağıda açıklanmıştır:

* Azure zaman serisi öngörüleri Önizleme ortamlar **PAYG** ortamları.
  * Oluşturma işleminin bir parçası bir zaman serisi kimliği sağlamanız gerekir Zaman serisi kimlikleri en fazla üç anahtarları olabilir. Zaman serisi kimliği okuyarak seçme hakkında daha fazla bilgi edinin [bir zaman serisi kimliği seçme](./time-series-insights-update-how-to-id.md).
  * Azure zaman serisi öngörüleri Önizleme ortamı sağladığınızda, iki Azure kaynaklarını oluşturun:

    * Azure zaman serisi öngörüleri Önizleme ortamı  
    * Bir Azure depolama genel amaçlı V1 hesabı
  
    Bilgi [ortamınızı planlama nasıl](./time-series-insights-update-plan.md).

    >[!IMPORTANT]
    > V2 hesapları kullanan müşteriler için kullanacağınız depolama hesabındaki soğuk/arşivleme özellikleri etkinleştirmeyin.

* Her Azure zaman serisi öngörüleri Önizleme ortamı (isteğe bağlı olarak) bir olay kaynağı ile ilişkili olabilir. Okuma [bir olay hub'ı kaynak ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md) ve [bir IOT hub'ı kaynak ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md) daha fazla bilgi için.
  * Bu adım sırasında bir zaman damgası kimliği özelliği ve benzersiz bir tüketici grubu sağlayacaktır. Bunun yapılması, ortam uygun etkinliklerine erişime sahip olmasını sağlar.

* Sağlama tamamlandıktan sonra iş gereksinimlerinizi erişim ilkelerinizi ve Suite diğer ortam özniteliklerini değiştirebilir.

## <a name="new-environment-creation"></a>Yeni ortam oluşturma

Aşağıdaki adımlar bir Azure zaman serisi öngörüleri önizlemesi ortamının nasıl oluşturulacağı açıklanmaktadır:

1. Seçin **PAYG** düğmesini **SKU** menüsü. Ortam adı sağlayın, hangi Abonelik grubu ve kullanmak için hangi kaynak grubu belirleyin. Ardından barındırılması için ortamı için desteklenen bir konum seçin.

1. Zaman serisi kimliği girin

    >[!NOTE]
    > * Zaman serisi kimliği büyük/küçük harfe ve sabit (ayarlandıktan sonra değiştirilemez).
    > * Zaman serisi kimlikleri en fazla 3 anahtarları olabilir.
    > * Zaman serisi kimliği okuyarak seçme hakkında daha fazla okuma [bir zaman serisi kimliği seçme](./time-series-insights-update-how-to-id.md).

1. Bir Azure depolama hesabı, bir Azure depolama hesabı adı seçerek ve bir çoğaltma seçeneği belirleme oluşturun. Otomatik olarak yapılması, bir Azure depolama genel amaçlı V1 hesabı oluşturur. Daha önce seçtiğiniz Azure zaman serisi öngörüleri Önizleme ortamı ile aynı bölgede oluşturulur.

    ![Azure Time Series Insights örneği oluşturun.][1]

1. İsteğe bağlı olarak, bir olay kaynağı ekleyebilirsiniz.

   * Zaman serisi görüşleri destekler [Azure IOT hub'ı](./time-series-insights-how-to-add-an-event-source-iothub.md) ve [Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) seçenekleri. Ortam oluşturma sırasında yalnızca tek bir olay kaynağı ekleyebilirsiniz, ancak daha sonra bu ek olay kaynağı ekleyebilirsiniz. Tüm olayları Azure zaman serisi öngörüleri önizlemesi Örneğinize görünür olduğundan emin olmak için benzersiz bir tüketici grubu oluşturmak idealdir. Varolan bir tüketici grubu seçin veya olay kaynağı eklerken, yeni bir tüketici grubu oluşturun.

   * Ayrıca, uygun zaman damgası özelliği de belirlemeniz gerekir. Varsayılan olarak, Azure zaman serisi görüşleri her bir olay kaynağı için ileti sıraya süresi kullanır.

     > [!TIP]
     > İleti sıraya süresi, toplu iş olayı veya geçmiş verileri karşıya yükleme senaryoları kullanmak için en iyi şekilde yapılandırılan bir ayarı olmayabilir. Kullanacağınıza karar kullanın veya bir zaman damgası özelliği, bu gibi durumlarda kullanmamanız doğruladığınızdan emin olun.

    ![Olay kaynağı Örneğinize ekleyin.][2]

1. Gözden geçirme ve oluşturma

    ![Olay kaynağı Örneğinize ekleyin.][3]

Ortamınızı istenen ayarlarla sağlandığını doğrulayın.

## <a name="management"></a>Yönetim

Azure portalını kullanarak Azure zaman serisi öngörüleri önizlemesi ortamınızı yönetmek için sahipsiniz. Yönetmedeki başlıca farklar şunlardır bir **PAYG** Azure portalını kullanarak bir S1 ve S2 ortam aksine Azure zaman serisi öngörüleri Önizleme ortamı:

* Azure portalında *genel bakış* dikey kaldığından Azure Time Series Insights, aşağıdaki yollarla:
  * Kapasite ile ilgili bu kavramı olmadığından kaldırıldı **PAYG** ortamları.
  * Zaman serisi kimliği özelliği eklenmiştir. Verilerinizi bölümlere nasıl belirler.
  * Başvuru veri kümelerini kaldırılır.
  * Görüntülenen URL, yönlendirir [Azure zaman serisi öngörüleri önizlemesi Gezgini](./time-series-insights-update-explorer.md).
  * Azure depolama hesabınızın adını listelenir.

* Azure portalı *yapılandırma* dikey olduğundan Azure zaman serisi öngörüleri önizlemesinde kaldırıldı **PAYG** ortamları yapılandırılabilir değildir.

* Azure portalı *başvuru* veri dikey penceresi başvuru verileri geçerli olmadığından bir bileşeni olan Azure zaman serisi öngörüleri önizlemesinde kaldırıldı **PAYG** ortamları.

![Örneğiniz doğrulayın.][4]

## <a name="next-steps"></a>Sonraki adımlar

Okuma [ortamınızı planlama nasıl](./time-series-insights-update-plan.md).

Okuma [bir olay hub'ı kaynak ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md).

Okuma [bir IOT hub'ı kaynak ekleme](./time-series-insights-how-to-add-an-event-source-iothub.md).

<!-- Images -->
[1]: media/v2-update-manage/manage_one.png
[2]: media/v2-update-manage/manage_two.png
[3]: media/v2-update-manage/manage_three.png
[4]: media/v2-update-manage/manage_four.png
