---
title: Azure Container Instances'ı çalıştırma
titleSuffix: Azure Cognitive Services
description: Azure Container Instance ve yaklaşım analizi görüntüsüyle metin analizi kapsayıcıları dağıtın ve bir web tarayıcısında test.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 9f174d54fcc74eed613eb69412bc0e515f15897b
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711730"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances"></a>Yaklaşım analizi kapsayıcı Azure Container Instances'a dağıtma

Bilişsel hizmetler dağıtmayı öğrenirsiniz [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) azure'a yaklaşım analizi görüntüsüyle kapsayıcı [Container Instances](https://docs.microsoft.com/azure/container-instances/). Bu yordam, metin analizi kaynak oluşturmayı, ilgili yaklaşım analizi görüntü ve bu ikisinin bir tarayıcıdan düzenleme çalışma olanağı oluşturulmasını exemplifies. Kapsayıcıları kullanarak, bunun yerine, uygulama geliştirme odaklanarak altyapı Yönetimi işlerini hayatınızdan çıkarın, geliştiricilerin dikkat kaydırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliği kullanın. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Sonraki adımlar 

* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../../cognitive-services-container-support.md)
* Kullanım [metin analizi bağlı hizmeti](../vs-text-connected-service.md)
