---
title: Yükseltme ve Azure API Management örneği ölçeklendirme | Microsoft Docs
description: Bu konu, yükseltme ve ölçeklendirme Azure API Management örneği açıklar.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/18/2018
ms.author: apimpm
ms.openlocfilehash: ac8babf3a00c73b942ae64ac4cca00c7be7cfcfa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60859547"
---
# <a name="upgrade-and-scale-an-azure-api-management-instance"></a>Yükseltme ve ölçeklendirme Azure API Management örneği  

Müşteriler birimler ekleyip çıkartarak tarafından bir Azure API Management (APIM) örneği ölçeklendirebilirsiniz. A **birim** adanmış Azure kaynaklarının oluşur ve bir belirli yük-ilgisi ayda birkaç API çağrıları olarak ifade edilen kapasite. Bu sayı, bir çağrı sınırı ancak kaba kapasite planlaması için izin vermek yerine bir en yüksek aktarım değeri temsil etmiyor. Geniş çapta gerçek aktarım hızı ve gecikme süresi sayısı ve oranı eş zamanlı bağlantılar gibi faktörlere bağlı olarak, türü ve sayısı, yapılandırılmış ilkeler, istek ve yanıt boyutları ve arka uç gecikme süresi farklılık gösterir.

Kapasite ve her birim fiyatına bağlıdır **katmanı** birim bulunduğu içinde. Dört katman arasında seçim yapabilirsiniz: **Geliştirici**, **temel**, **standart**, **Premium**. Bir katman içinde bir hizmet için kapasiteyi artırmak gerekiyorsa, bir birim eklemeniz gerekir. APIM Örneğinize şu anda seçili katmanı daha fazla birimi ekleme izin vermediği durumlarda bir üst düzey katmana yükseltmeniz gerekir.

APIM Örneğinize için seçtiğiniz katman fiyatı, her bir birimi ve kullanılabilir özellikleri (örneğin, çok bölgeli dağıtımlar) bağlıdır. [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makalesi, birim ve her katmanında alma özellikleri fiyatı açıklar. 

>[!NOTE]
>[Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makale her katmanında yaklaşık sayıda birim kapasitesi gösterir. Daha doğru numaraları almak için gerçekçi bir senaryo Apı'lerinizi aramak gerekir. Bkz: [bir Azure API Management örneğinin kapasite](api-management-capacity.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede adımları için yapmanız gerekir:

+ Etkin bir Azure aboneliğine sahip.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği vardır. Daha fazla bilgi için [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

+ Kavramı anlamak [bir Azure API Management örneğinin kapasite](api-management-capacity.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="upgrade-and-scale"></a>Yükseltme ve ölçeklendirme  

Dört katman arasında seçim yapabilirsiniz: **Geliştirici**, **temel**, **standart** ve **Premium**. **Geliştirici** katman hizmeti değerlendirmek için kullanılması gerekir; üretim için kullanılmamalıdır. **Geliştirici** katmanı SLA yoktur ve bu katmanda (ekleme/kaldırma birimleri) ölçek genişletilemiyor. 

**Temel**, **standart** ve **Premium** SLA'sına sahip ve ölçeklendirilebilir üretim katmanlarıdır. **Temel** katmanlıdır SLA olan ucuz katmanı ve en fazla 2 birim, ölçeklendirilebilir **standart** katmanı için en fazla dört birim ölçeklenebilen. Herhangi bir sayıda birimine ekleyebilirsiniz **Premium** katmanı.

**Premium** katman tek bir Azure API Management örneği dağıtmak istediğiniz Azure bölgeleri arasında herhangi bir sayı olanak sağlar. Başlangıçta bir Azure API Management hizmeti oluşturduğunuzda, örneği yalnızca bir birim içeren ve tek bir Azure bölgesinde bulunur. İlk bölge olarak tanımlanan **birincil** bölge. Ek bölgeler bir kolayca eklenebilir. Bir bölge eklediğinizde, ayırmak istediğiniz birim sayısını belirtin. Bir birimi, olabilir **birincil** bölge ve başka bir bölgede beş birimleri. Sahip olduğunuz her bölgede trafiği birim sayısını uyarlayabilirsiniz. Daha fazla bilgi için [Azure API Management hizmet örneği birden çok Azure bölgesine dağıtma](api-management-howto-deploy-multi-region.md).

Yükseltme ve herhangi bir katmanı gelen ve giden düşürme. Yükseltme veya indirgeme bazı özellikler - Örneğin, sanal ağları ya da çok bölgeli dağıtımlar, standart veya temel Premium katmanından önceki sürüme indirirken kaldırabileceğini unutmayın.

>[!NOTE]
>Yükseltmesi veya ölçeği işlemi uygulamak için 15 ila 45 dakika sürebilir. İşlem tamamlandığında bildirim alın.

## <a name="use-the-azure-portal-to-upgrade-and-scale"></a>Yükseltme ve ölçeklendirme için Azure portalını kullanma

![Azure portalında APIM ölçek](./media/upgrade-and-scale/portal-scale.png)

1. APIM Örneğinize gidin [Azure portalında](https://portal.azure.com/).
2. Seçin **ölçeklendirme ve fiyatlandırma** menüsünde.
3. İstediğiniz katmanı seçin.
4. Sayısını **birimleri** eklemek istediğiniz. Kaydırıcıyı kullanarak veya birim sayısını yazın.  
    Seçerseniz **Premium** katmanı, önce gereken bir bölge seçin.
5. **Kaydet**’e basın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Azure API Management hizmeti örneğini birden fazla Azure bölgesine dağıtma](api-management-howto-deploy-multi-region.md)
- [Azure API Management hizmet örneği otomatik olarak ölçeklendirme](api-management-howto-autoscale.md)
