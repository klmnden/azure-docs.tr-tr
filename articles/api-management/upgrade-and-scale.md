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
ms.openlocfilehash: 22cc917eb6f296724bf535e48b0dd6ba8927e5d3
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="upgrade-and-scale-an-api-management-instance"></a>Yükseltme ve API Management örneği ölçeklendirme 

Müşterilerin bir API Management (APIM) örneği ekleme ve kaldırma birimleri ölçeklendirebilirsiniz. A **birim** ayrılmış Azure kaynaklarının oluşur ve bir belirli yük-şifrelemeyle sahip kapasite ayda bir dizi API çağrıları olarak ifade edilir. Gerçek üretilen iş ve gecikmeyi soruna bağlı olarak numarası ve eşzamanlı bağlantı hızı gibi etkenlere, türü ve sayısı yapılandırılmış ilkeleri, istek ve yanıt boyutları ve arka uç gecikme değişir. 

Kapasite ve her birimi fiyat bağımlı bir **katmanı** birim bulunduğu içinde. Üç katmanlar arasında seçim yapabilirsiniz: **Geliştirici**, **standart**, **Premium**. Bir hizmet katmanı içinde kapasiteyi artırmak gerekiyorsa, bir birim eklemeniz gerekir. APIM örneğinizi seçili katmanı daha fazla birimi eklemeye izin vermiyor, üst düzey bir katmanına yükseltme yapmanız gerekir. 

Her birim fiyat, APIM Örneğiniz için seçtiğiniz katmanı ekleyin veya birim, belirli özellikleri (örneğin, çok bölge dağıtımı) sahip olup olmadığına bakılmaksızın Kaldır özelliğini bağlıdır. [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makalesi, her katman Al ne fiyatı birim ve özellikleri açıklar. 

>[!NOTE]
>[Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makalede her katmanında yaklaşık numaraları birim kapasitesi gösterilmektedir. Daha doğru numaraları almak için gerçekçi bir senaryo için Apı'lerinizi aramak gerekir. Aşağıdaki "kapasiteyi planlamak üzere" bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunlara sahip olmalısınız:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için bkz: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).

## <a name="how-to-plan-for-capacity"></a>Kapasiteyi planlamak üzere nasıl?

Trafiğinizi işlemek için yeterli birimler olup olmadığını öğrenmek için beklediğiniz iş yükleri üzerinde sınayın. 

İkinci APIM başına istek sayısı yukarıda belirtildiği gibi işlemi bağlıdır birçok değişkenleri. Örneğin, bağlantı deseni, istek ve yanıt, her API, istekleri gönderen istemci sayısı yapılandırılmış ilkelerinin boyutu.

Kullanım **ölçümleri** (kapasite ne kadar herhangi bir zamanda kullanılan anlamak için kullandığı Azure İzleyicisi Özellikleri).

### <a name="use-the-azure-portal-to-examine-metrics"></a>Ölçümleri incelemek için Azure portalını kullanın 

1. APIM örneğinizi gidin [Azure portal](https://portal.azure.com/).
2. Seçin **ölçümleri**.
3. Seçin **kapasite** gelen ölçüm **kullanılabilir ölçümler**. 

    Kapasite ölçüm kapasite ne kadar kiracınızda kullanılan bazı hakkında fikir verir. Daha fazla koyarak sınayabilirsiniz ve daha fazla yük ve çekme yük bakın. Gerçekleştiği zaman beklenmiyor bir şey size bildirmek için bir ölçüm uyarı ayarlayabilirsiniz. Örneğin, APIM örneğinizi üzerinde 5 dk. kapasiteyi accedes. 

    >[!TIP]
    > Kapasite veya yapma azalıyor hizmetiniz, çağırdığınızda otomatik olarak bir birimi ekleyerek ölçeklendirir bir mantıksal uygulama içine size bildirmek için uyarı yapılandırabilirsiniz..

## <a name="upgrade-and-scale"></a>Yükseltme ve ölçeklendirme 

Daha önce belirtildiği gibi üç katmanlar arasında seçim yapabilirsiniz: **Geliştirici**, **standart**, **Premium**. **Geliştirici** katmanı hizmet değerlendirmek için kullanılması gerekir; üretim için kullanılmamalıdır. **Geliştirici** katmanı SLA sahip değil ve bu katmanı (Ekle/Kaldır birimleri) ölçeği olamaz. 

**Standart** ve **Premium** üretim katmanları SLA sahip ve genişletilebilir. **Standart** katmanı için en fazla dört birim ölçeklendirilmiş. Herhangi bir sayıda birimlerine ekleyebilirsiniz **Premium** katmanı. 

**Premium** katmanı tek bir API management örneği istenen Azure bölgeleri herhangi bir sayıda dağıtmanızı sağlar. Bir API Management hizmeti başlangıçta oluşturduğunuzda örneği yalnızca bir birimi içeriyor ve tek bir Azure bölgesinde bulunur. İlk bölgeye olarak tasarlanmış **birincil** bölge. Ek bölgeler kolayca eklenebilir. Bir bölge eklerken, ayırmak istediğiniz birim sayısını belirtin. Örneğin, tek bir birim olabilir **birincil** bölge ve başka bir bölgede beş birimleri. Uyarlayabileceğiniz hangi trafik her bölgede sahip. Daha fazla bilgi için bkz: [Azure API Management hizmet örneği için birden fazla Azure bölgesine dağıtma](api-management-howto-deploy-multi-region.md).

Yükseltme ve herhangi bir katmanı gelen ve giden düşürmek. Lütfen eski sürüme düşürmeyi bazı özellikleri, örneğin, sanal ağlar veya bölgeli dağıtım Premium katmanı standart olarak önceki sürüme indirme zaman kaldırabileceğini unutmayın.

>[!NOTE]
>Yükseltme veya ölçek işlem 15 uygulamak için 30 dakika sürebilir. Bu işlem sona erdiğinde, bildirim alırsınız.

### <a name="use-the-azure-portal-to-upgrade-and-scale"></a>Yükseltme ve ölçeklendirmek için Azure portalını kullanma

1. APIM örneğinizi gidin [Azure portal](https://portal.azure.com/).
2. Seçin **ölçek ve fiyatlandırma**.
3. İstediğiniz katmanı seçin.
4. Sayısını belirtin **birimleri** eklemek istediğiniz. Kaydırıcıyı kullanın veya birim sayısını yazın.<br/>
    Seçerseniz **Premium** katmanı, ilk gereken bir bölge seçin.
5. Tuşuna **Kaydet**

## <a name="next-steps"></a>Sonraki adımlar

[Azure API Management hizmet örneği birden çok Azure bölgeler ile dağıtma](api-management-howto-deploy-multi-region.md)

