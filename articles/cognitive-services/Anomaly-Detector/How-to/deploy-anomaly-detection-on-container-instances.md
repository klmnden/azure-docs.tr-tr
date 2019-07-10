---
title: Azure Container Instances'ı çalıştırma
titleSuffix: Azure Cognitive Services
description: Azure Container Instance için Anomali algılayıcısı kapsayıcı dağıtın ve bir web tarayıcısında test.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: b5adc3ed9234050d3977e812202717a0ce83e842
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711697"
---
# <a name="deploy-an-anomaly-detector-container-to-azure-container-instances"></a>Bir Anomali algılayıcısı kapsayıcı Azure Container Instances'a dağıtma

Bilişsel hizmetler dağıtmayı öğrenirsiniz [Anomali algılayıcısı](../anomaly-detector-container-howto.md) Azure kapsayıcısına [Container Instances](https://docs.microsoft.com/azure/container-instances/). Bu yordam, bir Anomali algılayıcısı kaynak oluşturmayı göstermektedir. Ardından ilişkili kapsayıcı görüntüsünü çekme tartışın. Son olarak, orchestration bir tarayıcıdan iki çalışma olanağı vurgulayın. Kapsayıcıları kullanarak, bunun yerine, uygulama geliştirme odaklanarak altyapı Yönetimi işlerini hayatınızdan çıkarın, geliştiricilerin dikkat kaydırabilirsiniz.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

Önce tamamlamanız ve gönderme gerekir [Anomali algılayıcısı kapsayıcı istek formunu](https://aka.ms/adcontainer) kapsayıcıya erişim istemek için.

[!INCLUDE [Request access](../../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Anomaly Detector resource](../includes/create-anomaly-detector-resource.md)]

[!INCLUDE [Create an Anomaly Detector container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]