---
title: Azure Container Instances'ı çalıştırma
titleSuffix: Azure Cognitive Services
description: Görüntü işleme için Azure Container örneği dağıtma ve bir web tarayıcısında test.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 859147d23ea78abac2da4a4c2f1fa26a8d976d02
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711620"
---
# <a name="deploy-the-computer-vision-container-to-azure-container-instances"></a>Görüntü işleme kapsayıcıyı Azure Container Instances'a dağıtacaksınız.

Bilişsel hizmetler dağıtmayı öğrenirsiniz [görüntü işleme](computer-vision-how-to-install-containers.md) Azure kapsayıcısına [Container Instances](https://docs.microsoft.com/azure/container-instances/). Bu yordam, görüntü işleme kaynak oluşturmayı göstermektedir. Ardından ilişkili kapsayıcı görüntüsünü çekme tartışın. Son olarak, orchestration bir tarayıcıdan iki çalışma olanağı vurgulayın. Kapsayıcıları kullanarak, bunun yerine, uygulama geliştirme odaklanarak altyapı Yönetimi işlerini hayatınızdan çıkarın, geliştiricilerin dikkat kaydırabilirsiniz.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Create a Cognitive Services Computer Vision resource](includes/create-computer-vision-resource.md)]

[!INCLUDE [Create the Computer Vision on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]