---
title: Azure Time Series Insights (Önizleme) ortam oluştur | Microsoft Docs
description: Azure Time Series güncelleştirme ortam oluşturma hakkında bilgi edinin
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/26/2018
ms.openlocfilehash: 7e256f032c67649a4a8b01311e70d6bc9307ae92
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52852973"
---
# <a name="provisioning-and-managing-an-azure-time-series-insights-preview-environment"></a>Sağlama ve bir Azure Time Series Insights (Önizleme) ortamını yönetme

Bu belge, sağlamak ve Azure portalında yeni bir Azure Time Series Insights (Önizleme) ortamı yönetmek açıklar.

## <a name="overview"></a>Genel Bakış

Şu zaman serisi görüşleri güncelleştirme sağlama hakkında kısa bir açıklaması:

* Azure zaman serisi öngörüleri (TSI) güncelleştirme ortam sağlayın.
* Oluşturma işleminin bir parçası sağlamanız gerekir bir **zaman serisi kimliği**. En fazla olabilir **üç** (3) anahtarları. Daha fazla bilgi edinin [seçerek zaman serisi kimlikleri](./time-series-insights-update-how-to-id.md).
* Bir Azure TSI güncelleştirme ortam sağlarken, iki Azure kaynakları, TSI güncelleştirme ortamı ve bir Azure depolama genel amaçlı V1 hesabı oluşturun.  
* Gelecekte yeni Azure müşterileri, yalnızca varsayılan olarak bir Azure depolama genel amaçlı V2 hesabına sağlamak için verilir, bu değişiklik meydana geldiğinde bu nedenle, destekleyeceğiz.  
* Kullanacağınız depolama hesabındaki soğuk/arşivleme özellikleri etkinleştirmeyin.
* Ardından isteğe bağlı olarak güncelleştirme ortam bir zaman serisi görüşleri desteklenen olay kaynağına (örneğin, IOT Hub) bağlanabilirsiniz.
* Burada, sağlamak istediğiniz **zaman damgası kimliği** özelliği ve ortam bütün olaylara erişim izni olduğundan emin olmak için benzersiz bir tüketici grubu sağlayın.
* Sağlandıktan sonra erişim ilkelerini ve iş gereksinimlerinizi destekleyen diğer ortam öznitelikler isteğe bağlı olarak yönetebilirsiniz.

## <a name="new-environment-creation"></a>Yeni ortam oluşturma

1. Seçin `PAYG` gelen **SKU** açılır. Sizi ayrıca bir ortam adını girin, hangi abonelik ve kaynak grubu ortamında oluşturmak istediğiniz ve bulunması için ortamı için desteklenen bir konum seçin.  

1. Bir depolama hesabı adı seçerek ve bir çoğaltma seçeneği belirleme yeni bir Azure depolama hesabı oluşturun. Bunun yapılması otomatik olarak yeni bir Azure depolama genel amaçlı V1 hesabı aynı bölgede daha önce seçtiğiniz Azure TSI (Önizleme) ortamı oluşturur.  

1. Giriş **zaman serisi kimliği** özelliği:

    > [!IMPORTANT]
    > **Zaman serisi kimliği** duyarlıdır değişmez ve kez ayarlandıktan sonra değiştirilemez.

    Uygun seçme konusunda daha ayrıntılı bilgi edinin **zaman serisi kimliği** okumayı tarafından [zaman serisi kimlik seçmek için en iyi yöntemler](./time-series-insights-update-how-to-id.md).

    ![environment_details][1]

1. İsteğe bağlı olarak, bir olay kaynağı ekleyebilirsiniz:

    * Azure IOT Hub ve Event Hubs, Azure TSI seçeneklerini destekler. Ortam oluşturma sırasında yalnızca tek bir olay kaynağı ekleyebilirsiniz, ancak daha sonra bu ek olay kaynağı ekleyebilirsiniz. Tüm olaylar için TSI görünür olduğundan emin olmak için benzersiz bir tüketici grubu oluşturmak idealdir. Varolan bir tüketici grubu seçin veya olay kaynağı eklerken, yeni bir tüketici grubu oluşturun.

    * Ayrıca uygun belirlemeniz **zaman damgası** özelliği. Varsayılan olarak, TSI ileti sıraya alınan olayların toplu işleme veya geçmiş verileri karşıya yükleme, doğru olmayabilir her olay kaynağı için kullanır. Bu nedenle, büyük/küçük harfe zaman damgası özelliği, olay kaynağı eklerken girişi için zorunludur.  

     ![environment_event_sources][2]

1. Gözden geçir ve oluştur:

    ![environment_review][3]

    Her şeyin doğru olduğundan emin olun!

## <a name="management"></a>Yönetim

Azure portalını kullanarak TSI güncelleştirilmiş ortamınızı yönetmek için sahipsiniz. Büyük sürümler arasında taşınmasını olduğundan kullanıcılar ile TSI tanıdık TSI güncelleştirmesiyle hemen hissedene.

Bir L1 TSI ortamı karşı bir S1 veya S2 ortamı Azure portalını kullanarak yönetmedeki başlıca farklar aşağıda verilmiştir:

* TSI Azure portalı *genel bakış* dikey penceresinde:

  * Genel Bakış dikey penceresini kullanarak dışında aynı kalır:

    * Bu kavramı L1 ortamları için uygun olmadığından kapasite kaldırılır.
    * **Zaman serisi kimliği** özelliği eklendi. Bu zaman sağlama sırasında eklenen ve verilerinizi nasıl bölümlenmiş tanımlayan bir sabit özelliğidir.
    * Başvuru veri kümelerini kaldırılır.

* TSI Azure portalı *yapılandırma* dikey penceresinde:
  
  * Bekletme bekletme sınırsız ayarlanacak şekilde kaldırılır.

    * Daha fazla denetim için bunu gelecekte eklemek bekliyoruz, ancak şimdilik bir sınır bu ayarlayamazsınız.
    * Kaldırılan tüm davranışı kapasite, hesap makinesi ve depolama sınırı aştı.

* TSI Azure portalı *başvuru* veri dikey penceresi:

  * Tüm bu dikey pencere, başvuru verileri bir bileşen L1 ortamların olmadığından kaldırıldı.

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Sonraki adımlar

TSI depolama yapılandırması hakkında okuyun:

> [!div class="nextstepaction"]
> [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md)

Zaman serisi modelleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Azure TSI TSM (Önizleme)](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/environment_details.png
[2]: media/v2-update-provision/environment_event_sources.png
[3]: media/v2-update-provision/environment_review.png