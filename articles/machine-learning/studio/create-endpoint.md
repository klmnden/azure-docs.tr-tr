---
title: Azure Machine Learning Studio Web Hizmeti uç noktalarını - oluşturma | Microsoft Docs
description: Web Hizmeti uç noktalarını Azure Machine Learning'de oluşturuluyor. Her Web Hizmeti uç noktasını ayrı ayrı ele, kısıtlanan yönetilen ve.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.component: studio
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.openlocfilehash: 58418357d6e96204485887cca5d7724c42183db7
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275899"
---
# <a name="creating-endpoints-for-deployed-azure-machine-learning-studio-web-services"></a>Dağıtılan Azure Machine Learning Studio web hizmetleri için uç nokta oluşturma
> [!NOTE]
>  Bu konu, uygulanabilir teknikleri açıklar bir **Klasik** Machine Learning Web hizmeti.
> 
> 

Müşterilerinize İleri Satış Web Hizmetleri oluşturduğunuzda, Web hizmeti oluşturulduğu denemeye hala bağlı olan her müşteri için eğitilen modelleri vermeniz gerekir. Ayrıca, herhangi bir güncelleştirme deneme seçerek bir uç noktaya özelleştirmelerin üzerine yazmadan uygulanmalıdır.

Bunu gerçekleştirmek için Azure Machine Learning Studio, dağıtılan bir Web hizmeti için birden fazla uç nokta oluşturmanıza olanak sağlar. Her Web Hizmeti uç noktasını ayrı ayrı ele, kısıtlanan yönetilen ve. Her uç benzersiz URL'yi ve müşterilerinize dağıtabilirsiniz yetkilendirme anahtar noktasıdır.



## <a name="adding-endpoints-to-a-web-service"></a>Web hizmeti için uç noktaları ekleme
Bir Web hizmeti için uç nokta ekleme iki yolu vardır.

* Programlı olarak
* Azure Machine Learning Web Hizmetleri portalından

Uç nokta oluşturulduktan sonra zaman uyumlu API'leri aracılığıyla, batch API'leri, kullanma ve excel çalışma sayfaları. Bu kullanıcı Arabirimi aracılığıyla uç noktaları eklemenin yanı sıra, uç nokta yönetim API'lerini program aracılığıyla uç noktalarını eklemek için de kullanabilirsiniz.

> [!NOTE]
> Web hizmetine ek uç noktalar eklediyseniz, varsayılan uç nokta nelze odstranit.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Bir uç nokta programlı olarak ekleme
Program aracılığıyla kullanarak Web hizmetiniz için bir uç nokta ekleyebilirsiniz [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) örnek kodu.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Azure Machine Learning Web Hizmetleri portalını kullanarak bir uç nokta ekleme
1. Machine Learning Studio'da, sol gezinti sütununda, Web Hizmetleri.
2. Web hizmeti Pano altındaki tıklatın **uç noktalarını yönetme**. Azure Machine Learning Web Hizmetleri portalında Web Hizmeti uç noktaları sayfası açılır.
3. **Yeni**’ye tıklayın.
4. Bir ad ve yeni uç nokta için bir açıklama yazın. Uç nokta adları 24 karakter veya uzunluğunda olmalıdır ve küçük harfler veya sayıdan bayraklardan gerekir. Günlüğe kaydetme düzeyini ve örnek veriler etkin olup olmadığını seçin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Machine Learning Web Hizmetleri için günlüğe kaydetmeyi etkinleştirme](web-services-logging.md).

## <a name="next-steps"></a>Sonraki adımlar
[Bir Azure Machine Learning Web hizmetini kullanma](consume-web-services.md).

