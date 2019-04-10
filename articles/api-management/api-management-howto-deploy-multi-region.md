---
title: Azure API Management Hizmetleri birden çok Azure bölgesine dağıtma | Microsoft Docs
description: Azure API Management hizmet örneği birden çok Azure bölgesine dağıtma konusunda bilgi edinin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2019
ms.author: apimpm
ms.openlocfilehash: d22da92355616c208c7616b4b0e8c26b7f9e7006
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278658"
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Azure API Management hizmet örneği birden çok Azure bölgesine dağıtma

Azure API yönetimi, istenen Azure bölgeleri arasında herhangi bir sayı tek bir Azure API management hizmeti dağıtmak API yayımcılarının sağlayan çok bölgeli dağıtım destekler. Bu durum, istek tarafından algılanan gecikme API tüketicilerini coğrafi olarak dağıtılan ve tek bir bölge çevrimdışı olması durumunda da hizmet kullanılabilirliği artırır azaltmaya yardımcı olur.

Yeni bir Azure API Management hizmeti başlangıçta yalnızca bir tane içeriyor [birim] [ unit] tek bir Azure bölgesinde, birincil bölge. Ek bölgeler Azure Portalı aracılığıyla kolayca eklenebilir. Bir API Management ağ geçidi sunucusu, her bir bölgeye dağıtılır ve çağrı trafik, gecikme süresi açısından en yakın ağ geçidine yönlendirilir. Bir bölgeyi çevrimdışı olması durumunda, trafiği otomatik olarak sonraki en yakın ağ geçidine yönlendirilir.

> [!NOTE]
> Azure API Management, yalnızca API ağ geçidi bileşenini bölgeler arasında çoğaltır. Hizmet Yönetimi bileşeni, yalnızca birincil bölgede barındırılır. Birincil bölgede kesinti olması durumunda, bir Azure API Management hizmet örneği için yapılandırma değişikliklerini uygulama ayarları veya ilkeleri güncelleştirmeleri dahil olmak üzere - yapılamaz.

[!INCLUDE [premium.md](../../includes/api-management-availability-premium.md)]

## <a name="add-region"> </a>API Management hizmet örneği için yeni bir bölgeye dağıtın.

> [!NOTE]
> Henüz bir API Management hizmet örneği oluşturmadıysanız bkz [bir API Management hizmet örneği oluşturma][Create an API Management service instance].

Azure portalında gidin **ölçeklendirme ve fiyatlandırma** API Management hizmet örneğinizin sayfası. 

![Ölçek sekmesini][api-management-scale-service]

Yeni bir bölgeye dağıtmak için tıklayın **+ Ekle bölge** araç çubuğundan.

![Bölge ekle][api-management-add-region]

Aşağı açılan listeden konumu seçin ve kaydırıcı ile için birim sayısını ayarlayın.

![Birimler belirtin][api-management-select-location-units]

Tıklayın **Ekle** seçiminizi konumlar tablosunda yerleştirmek için. 

Yapılandırılan tüm konumları bulunana kadar bu işlemi yineleyin ve'ı tıklatın **Kaydet** dağıtım işlemini başlatmak için araç çubuğundan.

## <a name="remove-region"> </a>Bir konumdan bir API Management hizmet örneği silme

Azure portalında gidin **ölçeklendirme ve fiyatlandırma** API Management hizmet örneğinizin sayfası. 

![Ölçek sekmesini][api-management-scale-service]

Kaldırmak istediğiniz konum için bağlam menüsünü açın **...**  tablonun sonuna doğru düğmesi. Seçin **Sil** seçeneği.

Silme işlemini onaylayın ve tıklayın **Kaydet** değişiklikleri uygulamak için.

## <a name="route-backend"> </a>Bölgesel bir arka uç Hizmetleri için yol API çağrıları

Azure API Management, yalnızca bir arka uç hizmeti URL'si sunar. Çeşitli bölgelerdeki Azure API Management örneği olsa bile API ağ geçidi istekleri hala iletmek üzere tek bir bölgede dağıtılan aynı arka uç hizmetine olur. Bu durumda, performans kazanç isteği belirli bir bölgede Azure API Management içinde önbelleğe alınan yanıtları kaynağından gelir, ancak arka uç, dünya çapında iletişim kurmasını hala yüksek gecikmeye neden olabilir.

Coğrafi dağıtım, sisteminizin tam olarak yararlanmak için Azure API Management örneği ile aynı bölgede dağıtılan arka uç Hizmetleri olmalıdır. Ardından, ilkeleri kullanarak ve `@(context.Deployment.Region)` özelliği, yerel bir arka uç örneklerine trafiği yönlendirebilirsiniz.

1. Azure API Management Örneğinize gidin ve tıklayarak **API'leri** sol menüden.
2. İstenen API'nizi seçin.
3. Tıklayın **Kod Düzenleyicisi** ok açılır menüde gelen **gelen işlem**.

    ![API Kod Düzenleyicisi](./media/api-management-howto-deploy-multi-region/api-management-api-code-editor.png)

4. Kullanım `set-backend` koşullu ile birleştirilmiş `choose` içinde uygun bir yönlendirme ilkesi oluşturmak için ilkeleri `<inbound> </inbound>` dosyasının.

    Örneğin, XML dosyası Batı ABD ve Doğu Asya bölgelerinde için işe yarar:

    ```xml
    <policies>
        <inbound>
            <base />
            <choose>
                <when condition="@("West US".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-us.com/" />
                </when>
                <when condition="@("East Asia".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-asia.com/" />
                </when>
                <otherwise>
                    <set-backend-service base-url="http://contoso-other.com/" />
                </otherwise>
            </choose>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>
    ```

> [!TIP]
> Ayrıca, arka uç Hizmetleri ile ön [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/)doğrudan API çağrıları için Traffic Manager ve bunu otomatik olarak Yönlendirme çözmek olanak tanır.

## <a name="custom-routing"> </a>Özel API Management için bölgesel ağ geçidi yönlendirme kullanın

API Management istekleri için bir bölge yönlendiren *ağ geçidi* göre [en düşük gecikme](../traffic-manager/traffic-manager-routing-methods.md#performance). API Management bu ayarı geçersiz kılmak mümkün olmasa da, kendi Traffic Manager ile özel yönlendirme kuralları kullanabilirsiniz.

1. Kendi uzantınızı oluşturun [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/).
1. Özel bir etki alanı kullanıyorsanız [Traffic Manager ile kullanmayı](../traffic-manager/traffic-manager-point-internet-domain.md) API Management hizmeti yerine.
1. [API Management bölgesel uç noktaları Traffic Manager'da yapılandırma](../traffic-manager/traffic-manager-manage-endpoints.md). Bölgesel uç noktaları URL desene uyar `https://<service-name>-<region>-01.regional.azure-api.net`, örneğin `https://contoso-westus2-01.regional.azure-api.net`.
1. [API Management bölgesel durum uç noktaları Traffic Manager'da yapılandırma](../traffic-manager/traffic-manager-monitoring.md). Bölgesel durum uç noktaları URL desene uyar `https://<service-name>-<region>-01.regional.azure-api.net/status-0123456789abcdef`, örneğin `https://contoso-westus2-01.regional.azure-api.net/status-0123456789abcdef`.
1. Belirtin [yönlendirme yöntemini](../traffic-manager/traffic-manager-routing-methods.md) Traffic Manager'ın.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: get-started-create-service-instance.md
[Get started with Azure API Management]: get-started-create-service-instance.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: https://azure.microsoft.com/pricing/details/api-management/
[Premium]: https://azure.microsoft.com/pricing/details/api-management/
