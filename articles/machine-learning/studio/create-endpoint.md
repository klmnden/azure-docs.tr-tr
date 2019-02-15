---
title: Web Hizmeti uç noktaları oluşturma
titleSuffix: Azure Machine Learning Studio
description: Web Hizmeti uç noktalarını Azure Machine Learning'de oluşturuluyor. Her Web Hizmeti uç noktasını ayrı ayrı ele, kısıtlanan yönetilen ve.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/12/2019
ms.openlocfilehash: a6945ac7bfb750916e229ae04376f895f2b3d506
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267284"
---
# <a name="creating-endpoints-for-deployed-azure-machine-learning-studio-web-services"></a>Dağıtılan Azure Machine Learning Studio web hizmetleri için uç nokta oluşturma

> [!NOTE]
> Bu konu, uygulanabilir teknikleri açıklar bir **Klasik** Machine Learning Web hizmeti.

Bir web hizmeti dağıtıldıktan sonra, bu hizmet için varsayılan bir uç noktası oluşturulur. Varsayılan uç noktası, API anahtarı kullanılarak çağrılabilir. Web Hizmetleri portalından, kendi anahtarlarına sahip ek uç noktalar ekleyebilirsiniz.
Her Web Hizmeti uç noktasını ayrı ayrı ele, kısıtlanan yönetilen ve. Her uç nokta, müşterileriniz için dağıtabileceğiniz bir yetkilendirme anahtarına sahip benzersiz bir URL'dir.

## <a name="adding-endpoints-to-a-web-service"></a>Web hizmeti için uç noktaları ekleme

Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmeti için bir uç nokta ekleyebilirsiniz. Uç nokta oluşturulduktan sonra zaman uyumlu API'leri aracılığıyla, batch API'leri, kullanma ve excel çalışma sayfaları.

> [!NOTE]
> Web hizmetine ek uç noktalar eklediyseniz, varsayılan uç nokta nelze odstranit.

1. Machine Learning Studio'da, sol gezinti sütununda, Web Hizmetleri.
2. Web hizmeti Pano altındaki tıklatın **uç noktalarını yönetme**. Azure Machine Learning Web Hizmetleri portalında Web Hizmeti uç noktaları sayfası açılır.
3. **Yeni**’ye tıklayın.
4. Bir ad ve yeni uç nokta için bir açıklama yazın. Uç nokta adları 24 karakter veya uzunluğunda olmalıdır ve küçük harfler veya sayıdan bayraklardan gerekir. Günlüğe kaydetme düzeyini ve örnek veriler etkin olup olmadığını seçin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Machine Learning Web Hizmetleri için günlüğe kaydetmeyi etkinleştirme](web-services-logging.md).

## <a name="next-steps"></a>Sonraki adımlar

[Bir Azure Machine Learning Web hizmetini kullanma](consume-web-services.md).
