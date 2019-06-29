---
title: Azure Container Instances'ı çalıştırma
titleSuffix: Text Analytics - Azure Cognitive Services
description: Azure Container Instance ve yaklaşım analizi görüntüsüyle metin analizi kapsayıcıları dağıtın ve bir web tarayıcısında test.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: a82b5a1cbed662289d3a406a61fdd19a3ca8861b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454988"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances-aci"></a>Yaklaşım analizi kapsayıcı Azure Container Instances'a (ACI) dağıtma

Bilişsel hizmetler dağıtmayı öğrenirsiniz [metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) azure'a yaklaşım analizi görüntüsüyle kapsayıcı [Container Instances](https://docs.microsoft.com/azure/container-instances/) (ACI). Bu yordam, metin analizi kaynak oluşturmayı, ilgili yaklaşım analizi görüntü ve bu ikisinin bir tarayıcıdan düzenleme çalışma olanağı oluşturulmasını exemplifies. Kapsayıcıları kullanarak, bunun yerine, uygulama geliştirme odaklanarak altyapı Yönetimi işlerini hayatınızdan çıkarın, geliştiricilerin dikkat kaydırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliği kullanın. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances (ACI)](../../containers/includes/create-aci-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Sonraki adımlar 

* Daha fazla kullanmanız [Bilişsel Hizmetleri kapsayıcıları](../../cognitive-services-container-support.md)
* Kullanım [metin analizi bağlı hizmeti](../vs-text-connected-service.md)
