---
title: Yükseltme ve Azure API Management örneği ölçeklendirin | Microsoft Docs
description: Bu konu, yükseltme ve Azure API Management örneği ölçeklendirme açıklar.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/18/2018
ms.author: apimpm
ms.openlocfilehash: ca32c72b1582b2a09f9f1754ad778cf1b682a1c2
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293321"
---
# <a name="upgrade-and-scale-an-api-management-instance"></a>Yükseltme ve API Management örneği ölçeklendirme  

Müşterilerin bir API Management (APIM) örneği ekleme ve kaldırma birimleri ölçeklendirebilirsiniz. A **birim** ayrılmış Azure kaynaklarının oluşur ve bir belirli yük-şifrelemeyle sahip kapasite ayda bir dizi API çağrıları olarak ifade edilir. Bu sayı, bir çağrı sınırı ancak kaba kapasite planlaması için izin vermek yerine bir en yüksek verimlilik değeri temsil etmiyor. Geniş çapta gerçek üretilen iş ve gecikmeyi, tür ve yapılandırılmış ilkeleri, istek ve yanıt boyutları ve arka uç gecikme sayısı numarası ve eşzamanlı bağlantı hızı gibi etkenlere bağlı olarak farklılık gösterir.

Kapasite ve her birimi fiyat bağımlı **katmanı** birim bulunduğu içinde. Dört katmanlar arasında seçim yapabilirsiniz: **Geliştirici**, **temel**, **standart**, **Premium**. Bir hizmet katmanı içinde kapasiteyi artırmak gerekiyorsa, bir birim eklemeniz gerekir. APIM örneğinizi seçili katmanı daha fazla birimi eklemeye izin vermiyor, üst düzey bir katmanına yükseltme yapmanız gerekir.

Her birim ve kullanılabilir özellikler (örneğin, çok bölge dağıtımı) bedelinin APIM Örneğiniz için seçtiğiniz katmanı bağlıdır. [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makalesi, birim ve her katmanında alma özellikleri fiyatı açıklar. 

>[!NOTE]
>[Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makalede her katmanında yaklaşık numaraları birim kapasitesi gösterilmektedir. Daha doğru numaraları almak için gerçekçi bir senaryo için Apı'lerinizi aramak gerekir. Bkz: [bir Azure API Management örneğinin kapasite](api-management-capacity.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede adımları için yapmanız gerekir:

+ Etkin bir Azure aboneliğinizin olması.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği vardır. Daha fazla bilgi için bkz: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

+ [Azure API Management örneği kapasitesini] kavramı anlamak (API management capacity.md).

## <a name="upgrade-and-scale"></a>Yükseltme ve ölçeklendirme  

Dört katmanlar arasında seçim yapabilirsiniz: **Geliştirici**, **temel**, **standart** ve **Premium**. **Geliştirici** katmanı hizmet değerlendirmek için kullanılması gerekir; üretim için kullanılmamalıdır. **Geliştirici** katmanı SLA sahip değil ve bu katmanı (Ekle/Kaldır birimleri) ölçeği olamaz. 

**Temel**, **standart** ve **Premium** SLA ve Genişletilebilir üretim katmanlarıdır. **Temel** katmanı SLA olan ucuz katmanı ve ölçeklendirilmiş değerine kadar 2 birimleri çalıştırılabilir **standart** katmanı için en fazla dört birim ölçeklendirilmiş. Herhangi bir sayıda birimlerine ekleyebilirsiniz **Premium** katmanı.

**Premium** katmanı tek bir API management örneği istenen Azure bölgeleri herhangi bir sayıda dağıtmanızı sağlar. Bir API Management hizmeti başlangıçta oluşturduğunuzda örneği yalnızca bir birimi içeriyor ve tek bir Azure bölgesinde bulunur. İlk bölgeye olarak tasarlanmış **birincil** bölge. Ek bölgeler kolayca eklenebilir. Bir bölge eklerken, ayırmak istediğiniz birim sayısını belirtin. Örneğin, tek bir birim olabilir **birincil** bölge ve başka bir bölgede beş birimleri. Her bölgede sahip trafiği birim sayısını uyarlayabilirsiniz. Daha fazla bilgi için bkz: [Azure API Management hizmet örneği için birden fazla Azure bölgesine dağıtma](api-management-howto-deploy-multi-region.md).

Yükseltme ve herhangi bir katmanı gelen ve giden düşürmek. Yükseltme veya eski sürüme düşürmeyi bazı özellikleri - örneğin, sanal ağlar veya bölgeli dağıtım standart ya da temel Premium Katmanı'ndan önceki sürüme indirme zaman kaldırabileceğini unutmayın.

>[!NOTE]
>Yükseltme veya ölçek işlem 15 uygulamak için 45 dakika sürebilir. Bu işlem sona erdiğinde, bildirim alırsınız.

## <a name="use-the-azure-portal-to-upgrade-and-scale"></a>Yükseltme ve ölçeklendirmek için Azure portalını kullanma

![Ölçek APIM Azure portalında](./media/upgrade-and-scale/portal-scale.png)

1. APIM örneğinizi gidin [Azure portal](https://portal.azure.com/).
2. Seçin **ölçek ve fiyatlandırma** menüsünde.
3. İstediğiniz katmanı seçin.
4. Sayısını belirtin **birimleri** eklemek istediğiniz. Kaydırıcıyı kullanın veya birim sayısını yazın.  
    Seçerseniz **Premium** katmanı, ilk gereken bir bölge seçin.
5. Tuşuna **Kaydet**

## <a name="next-steps"></a>Sonraki adımlar

[Azure API Management hizmet örneği birden çok Azure bölgeler ile dağıtma](api-management-howto-deploy-multi-region.md)