---
title: Azure Container Instances'ı çalıştırma
titleSuffix: Azure Cognitive Services
description: Azure Container Instance için konuşma tanıma hizmeti dağıtma ve bir web tarayıcısında test.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 062765be22135b12abb29ff6f7ce8a772c67adae
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711521"
---
# <a name="deploy-the-speech-service-container-to-azure-container-instances"></a>Konuşma hizmeti kapsayıcıyı Azure Container Instances'a dağıtacaksınız.

Bilişsel hizmetler dağıtmayı öğrenirsiniz [konuşma hizmeti](speech-container-howto.md) Azure kapsayıcısına [Container Instances](https://docs.microsoft.com/azure/container-instances/). Bu yordamı, bir Azure konuşma hizmeti kaynağı oluşturulmasını gösterir. Ardından ilişkili kapsayıcı görüntüsünü çekme tartışın. Son olarak, orchestration bir tarayıcıdan iki çalışma olanağı vurgulayın. Kapsayıcıları kullanarak, bunun yerine, uygulama geliştirme odaklanarak altyapı Yönetimi işlerini hayatınızdan çıkarın, geliştiricilerin dikkat kaydırabilirsiniz.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteği

Önce tamamlamanız ve gönderme gerekir [Bilişsel hizmetler konuşma kapsayıcıları istek formunu](https://aka.ms/speechcontainerspreview/) kapsayıcıya erişim istemek için. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Speech Service resource](includes/create-speech-resource.md)]

[!INCLUDE [Create an Speech Service container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
