---
title: "Yükseltme ve Azure API Management örneği ölçeklendirin | Microsoft Docs"
description: "Bu konu, yükseltme ve Azure API Management örneği ölçeklendirme açıklar."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: e92c1a44b49c64308438184ab8185a90766c5bcf
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="upgrade-and-scale-an-api-management-instance"></a>Yükseltme ve API Management örneği ölçeklendirme 

Müşterilerin bir API Management (APIM) örneği ekleme ve kaldırma birimleri ölçeklendirebilirsiniz. A **birim** ayrılmış Azure kaynaklarının oluşur ve bir belirli yük-şifrelemeyle sahip kapasite ayda bir dizi API çağrıları olarak ifade edilir. Bu sayı, bir çağrı sınırı ancak kaba kapasite planlaması için izin vermek yerine bir en yüksek verimlilik değeri temsil etmiyor. Geniş çapta gerçek üretilen iş ve gecikmeyi, tür ve yapılandırılmış ilkeleri, istek ve yanıt boyutları ve arka uç gecikme sayısı numarası ve eşzamanlı bağlantı hızı gibi etkenlere bağlı olarak farklılık gösterir.

Kapasite ve her birimi fiyat bağımlı **katmanı** birim bulunduğu içinde. Üç katmanlar arasında seçim yapabilirsiniz: **Geliştirici**, **standart**, **Premium**. Bir hizmet katmanı içinde kapasiteyi artırmak gerekiyorsa, bir birim eklemeniz gerekir. APIM örneğinizi seçili katmanı daha fazla birimi eklemeye izin vermiyor, üst düzey bir katmanına yükseltme yapmanız gerekir. 

Her birim ve kullanılabilir özellikler (örneğin, çok bölge dağıtımı) bedelinin APIM Örneğiniz için seçtiğiniz katmanı bağlıdır. [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makalesi, birim ve her katmanında alma özellikleri fiyatı açıklar. 

>[!NOTE]
>[Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makalede her katmanında yaklaşık numaraları birim kapasitesi gösterilmektedir. Daha doğru numaraları almak için gerçekçi bir senaryo için Apı'lerinizi aramak gerekir. Aşağıdaki "kapasiteyi planlamak üzere" bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunlara sahip olmalısınız:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için bkz: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

## <a name="how-to-plan-for-capacity"></a>Kapasiteyi planlamak üzere nasıl?

Trafiğinizi işlemek için yeterli birimler olup olmadığını öğrenmek için beklediğiniz iş yükleri üzerinde sınayın. 

Yukarıda belirtildiği gibi bir APIM birim işleyebilir saniye başına istek sayısı gibi birçok değişkene bağlıdır. Örneğin, bağlantı deseni, istek ve yanıt, her API, istekleri gönderen istemci sayısı yapılandırılmış ilkelerinin boyutu.

Kullanım **ölçümleri** (kapasite ne kadar herhangi bir zamanda kullanılan anlamak için kullandığı Azure İzleyicisi Özellikleri).

### <a name="use-the-azure-portal-to-examine-metrics"></a>Ölçümleri incelemek için Azure portalını kullanın 

1. APIM örneğinizi gidin [Azure portal](https://portal.azure.com/).
2. Seçin **ölçümleri**.
3. Seçin **kapasite** gelen ölçüm **kullanılabilir ölçümler**. 

    Kapasite ölçüm kullanılabilir işlem kapasitesini ne kadarının kiracınızda kullanılan bazı fikir verir. Değeri, bellek, CPU ve ağ sıra uzunlukları gibi Kiracı tarafından kullanılan işlem kaynakları türetilir. İşlenmekte olan istek sayısının doğrudan bir ölçü değil. Kiracı istek yükü artırarak ve kapasite ölçüm hangi değerini, yoğun yük karşılık gelen izleme test edebilirsiniz. Beklenmeyen bir şey olduğunda gerçekleştiği size bildirmek için bir ölçüm uyarı ayarlayabilirsiniz. Örneğin, APIM örneğinizi üzerinde 10 dakika için beklenen en yüksek kapasitesi aşıldı.

    >[!TIP]
    > Bir birim ekleyerek otomatik olarak ölçeklendirme hizmetinizi kapasite azaldığında biliyorsanız veya bir mantıksal uygulama çağrı olanak uyarılar yapılandırabilirsiniz.

## <a name="upgrade-and-scale"></a>Yükseltme ve ölçeklendirme 

Daha önce belirtildiği gibi üç katmanlar arasında seçim yapabilirsiniz: **Geliştirici**, **standart**, ve **Premium**. **Geliştirici** katmanı hizmet değerlendirmek için kullanılması gerekir; üretim için kullanılmamalıdır. **Geliştirici** katmanı SLA sahip değil ve bu katmanı (Ekle/Kaldır birimleri) ölçeği olamaz. 

**Standart** ve **Premium** SLA ve Genişletilebilir üretim katmanlarıdır. **Standart** katmanı için en fazla dört birim ölçeklendirilmiş. Herhangi bir sayıda birimlerine ekleyebilirsiniz **Premium** katmanı. 

**Premium** katmanı tek bir API management örneği istenen Azure bölgeleri herhangi bir sayıda dağıtmanızı sağlar. Bir API Management hizmeti başlangıçta oluşturduğunuzda örneği yalnızca bir birimi içeriyor ve tek bir Azure bölgesinde bulunur. İlk bölgeye olarak tasarlanmış **birincil** bölge. Ek bölgeler kolayca eklenebilir. Bir bölge eklerken, ayırmak istediğiniz birim sayısını belirtin. Örneğin, tek bir birim olabilir **birincil** bölge ve başka bir bölgede beş birimleri. Her bölgede sahip trafiği birim sayısını uyarlayabilirsiniz. Daha fazla bilgi için bkz: [Azure API Management hizmet örneği için birden fazla Azure bölgesine dağıtma](api-management-howto-deploy-multi-region.md).

Yükseltme ve herhangi bir katmanı gelen ve giden düşürmek. Yükseltme veya eski sürüme düşürmeyi bazı özellikleri - örneğin, sanal ağlar veya bölgeli dağıtım Premium katmanı standart olarak önceki sürüme indirme zaman kaldırabileceğini unutmayın.

>[!NOTE]
>Yükseltme veya ölçek işlem 15 uygulamak için 45 dakika sürebilir. Bu işlem sona erdiğinde, bildirim alırsınız.

### <a name="use-the-azure-portal-to-upgrade-and-scale"></a>Yükseltme ve ölçeklendirmek için Azure portalını kullanma

1. APIM örneğinizi gidin [Azure portal](https://portal.azure.com/).
2. Seçin **ölçek ve fiyatlandırma**.
3. İstediğiniz katmanı seçin.
4. Sayısını belirtin **birimleri** eklemek istediğiniz. Kaydırıcıyı kullanın veya birim sayısını yazın.<br/>
    Seçerseniz **Premium** katmanı, ilk gereken bir bölge seçin.
5. Tuşuna **Kaydet**

## <a name="next-steps"></a>Sonraki adımlar

[Azure API Management hizmet örneği birden çok Azure bölgeler ile dağıtma](api-management-howto-deploy-multi-region.md)

