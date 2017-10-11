---
title: "Birden çok Azure bölgeler ile Azure API Management services dağıtma | Microsoft Docs"
description: "Azure API Management hizmet örneği için birden fazla Azure bölgesine dağıtmayı öğrenin."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 47389ad6-f865-4706-833f-846115e22e4d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 1c39fee739c2f5fd4b928e1e76e1ea57f072b5f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Azure API Management hizmet örneği birden çok Azure bölgeler ile dağıtma
API Management istenen Azure bölgeleri herhangi bir sayıda arasında tek bir API management hizmeti dağıtmak API yayımcılar sağlayan bölgeli dağıtımını destekler. Bu istek tarafından algılanan gecikme API tüketicileri coğrafi olarak dağıtılmış ve bir bölge çevrimdışı olursa hizmet kullanılabilirliği de geliştirir azaltılmasına yardımcı olur. 

Bir API Management hizmeti başlangıçta oluşturulduğunda, yalnızca bir tane içeriyor [birim] [ unit] ve birincil bölge belirlenmiş tek bir Azure bölgesi bulunur. Ek bölgeler kolayca Azure Portalı aracılığıyla eklenebilir. Bir API Management ağ geçidi sunucusu her bölgeye dağıtılır ve arama trafiği için en yakın ağ geçidi yönlendirilir. Bir bölge çevrimdışı olursa, otomatik olarak sonraki en yakın ağ geçidine yeniden yönlendirilmiş bir trafiğidir. 

> [!IMPORTANT]
> Bölgeli dağıtım bulunan yalnızca  **[Premium] [ Premium]**  katmanı.
> 
> 

## <a name="add-region"></a>API Management hizmet örneği için yeni bir bölge dağıtma
> [!NOTE]
> Henüz bir API Management hizmeti örneği oluşturmadıysanız, [Azure API Management'i kullanmaya başlama][Get started with Azure API Management] öğreticisinde [API Management hizmet örneği oluşturma][Create an API Management service instance]'ya bakın.
> 
> 

Azure Portalı'nda gidin **ölçek ve fiyatlandırma** , API Management hizmet örneğinizin sayfası. 

![Ölçek sekmesi][api-management-scale-service]

Yeni bir bölgeye dağıtmayı tıklayın **+ Ekle bölge** araç çubuğundan.

![Bölgesi ekleme][api-management-add-region]

Aşağı açılan listeden konumu seçin ve kaydırıcı ile birim sayısını ayarlayın.

![Birimler belirtin][api-management-select-location-units]

Tıklatın **Ekle** konumlar tablosunda seçiminizi yerleştirilecek. 

Yapılandırılan tüm konumları elde edene kadar bu işlemi yineleyin ve'ı tıklatın **kaydetmek** dağıtım işlemini başlatmak için araç çubuğundan.

## <a name="remove-region"></a>Bir konumdan bir API Management hizmeti örneği Sil
Azure Portalı'nda gidin **ölçek ve fiyatlandırma** , API Management hizmet örneğinizin sayfası. 

![Ölçek sekmesi][api-management-scale-service]

Kaldırmak istediğiniz konumu için bağlam menüsünü kullanarak açın **...**  tablo sağ ucunda düğmesi. Seçin **silmek** seçeneği.

![Bölgeyi Kaldır][api-management-remove-region]

Silme işlemini onaylamak ve tıklayın **kaydetmek** değişiklikleri uygulamak için.

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

