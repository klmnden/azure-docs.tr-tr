---
title: Hizmet kotaları ve sınırları için Azure Batch AI | Microsoft Docs
description: Varsayılan Azure Batch AI kotaları, sınırları ve kısıtlamaları hakkında bilgi edinin ve kota isteğinde nasıl artırır
services: batch-ai
documentationcenter: ''
author: johnwu10
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/08/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 16032ec5ba1e613462f92b86281ce93153b70923
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409726"
---
# <a name="batch-ai-service-quotas-and-limits"></a>Batch AI hizmet kotaları ve limitleri

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

Olarak diğer Azure hizmetleriyle sınırı yoktur Batch AI hizmeti ile ilişkili belirli kaynaklar. Batch AI bu sınırları hizmeti, her bölge için abonelik düzeyinde uygulanan varsayılan kotalardır [kullanılabilir](https://azure.microsoft.com/global-infrastructure/services/). Bu makalede bu Varsayılanları açıklar ve kota isteğinde bulunabilirsiniz nasıl artırır.

Bu kotalar, tasarım ve Batch yapay ZEKA kaynaklarınızı ölçek aklınızda tutun. Kümenizi, belirtilen düğümlerin hedef sayısını ulaşmaz, örneğin, daha sonra bir Batch AI çekirdek, aboneliğiniz için sınırına ulaştınız.

Batch AI üretim iş yükleri çalıştırmayı planlıyorsanız, bir veya daha üstüne varsayılan kotaları artırmak gerekebilir.

> [!NOTE]
> Kota kapasitesini garanti bir kredi sınırına ' dir. Büyük ölçekli kapasite gereksinimleriniz varsa, lütfen Azure desteğine başvurun.
> 
> 

## <a name="resource-quotas"></a>Kaynak kotaları

[!INCLUDE [azure-batch-ai-limits](../../includes/azure-batch-ai-limits.md)]

## <a name="other-limits"></a>Diğer sınırlamaları

Aşağıdakiler aşılamaz katı sınırları, bir kez basın.

| **Kaynak** | **Üst sınırı** |
| --- | --- |
| Kaynak grubu başına en fazla çalışma alanları | 800 |
| Küme boyutu üst sınırı | 100 düğüm |
| Düğüm başına en fazla GPU MPI işler | 1-4 |
| Düğüm başına en fazla GPU çalışanları | 1-4 |
| En fazla iş yaşam süresi | 7 gün<sup>1</sup> |
| Düğüm başına en fazla parametre sunucuları | 1 |

<sup>1</sup> maksimum ömrü süresini gösterir. bir işi çalıştırma ve tamamlandığında başlar. Tamamlanan İşler süresiz olarak kalır; en fazla bir yaşam süresi içinde tamamlanmamış olan zamanlanmamış işleri için verileri erişilebilir değil.

## <a name="view-batch-ai-quotas"></a>Batch AI kotaları görüntüle

İçindeki geçerli Batch AI abonelik kotanızı görüntüleme [Azure portalında][portal].

1. Sol bölmesinde, tıklayarak **tüm hizmetleri**. Ardından aramak **Batch AI** ve hizmet açmak için tıklayın.
2. Tıklayarak **kullanım ve kotalar** Batch AI menüsünde.
3. Kota sınırları görüntülemek için aboneliğinizi seçin.

## <a name="increase-a-batch-ai-cores-quota"></a>Batch AI çekirdek kota artırma

Kota isteği için şu adımları artırmak için Batch AI aboneliğinden izleyin [Azure portalında][portal]. 

1. Sol bölmesinde, tıklayarak **tüm hizmetleri**. Ardından aramak **Batch AI** ve hizmet açmak için tıklayın.
2. Tıklayarak **yeni destek isteği** Batch AI menüsünde.
3. İçinde **Temelleri**:
   
    a. **Sorun türü** > **kota**
   
    b. **Abonelik** > aboneliğinizi seçin.
   
    c. **Kota türü** > **Batch AI**
   
    d. **Destek planınız** > Destek planını seçin.

    **İleri**'ye tıklayın.
4. İçinde **sorun**:
   
    a. Seçin bir **önem derecesi** göre [iş etkisi][support_sev].
   
    b. İçinde **kota ayrıntıları**, konum, kota türünü ve kaynak türü belirtin. İstek istediğiniz yeni sınırını belirtin. Tıklayın **Kaydet ve devam et**.

    c. İsteğe bağlı - nedeninizi artırmak için ilgili daha fazla bilgi ile ilgili tüm dosyaları karşıya yükleme.
   
    **İleri**'ye tıklayın.
5. İçinde **iletişim bilgilerini**:
   
    a. Seçin bir **tercih edilen iletişim yöntemi**.
   
    b. Gerekli kişi ayrıntılarını girin ve doğrulayın.
   
    Destek isteğini göndermek için **Oluştur**'a tıklayın.

Azure desteği, destek isteğinizi gönderdikten sonra sizinle iletişime geçecektir. İstek tamamlanırken en fazla 2 iş günü sürebilir.


## <a name="next-steps"></a>Sonraki adımlar

Kota sınırları ile ilgili bilgi sahibi olma sonra Batch AI'ı kullanmaya başlama için aşağıdaki makalelere göz atın.

> [!div class="nextstepaction"]
> [Batch AI hızlı başlangıç Öğreticisi](quickstart-tensorflow-training-cli.md)
> [Batch AI tarifleri](https://github.com/Azure/BatchAI/tree/master/recipes)
> [Batch AI kaynakları hakkında daha fazla bilgi edinin](resource-concepts.md)

[portal]: https://portal.azure.com
[support_sev]: http://aka.ms/supportseverity