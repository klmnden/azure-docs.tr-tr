---
title: Azure Container Instances'ı çalıştırma
titleSuffix: Azure Cognitive Services
description: Azure Container Instance için yüz kapsayıcı dağıtın ve bir web tarayıcısında test.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: e67846b6b304b5425f7e8334eb3a4499a029d5ab
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711598"
---
# <a name="deploy-the-face-container-to-azure-container-instances"></a>Yüz tanıma kapsayıcıyı Azure Container Instances'a dağıtacaksınız.

Bilişsel hizmetler dağıtmayı öğrenirsiniz [yüz](../face-how-to-install-containers.md) Azure kapsayıcısına [Container Instances](https://docs.microsoft.com/azure/container-instances/). Bu yordam, bir Azure yüz kaynak oluşturulmasını gösterir. Ardından ilişkili kapsayıcı görüntüsünü çekme tartışın. Son olarak, orchestration bir tarayıcıdan iki çalışma olanağı vurgulayın. Kapsayıcıları kullanarak, bunun yerine, uygulama geliştirme odaklanarak altyapı Yönetimi işlerini hayatınızdan çıkarın, geliştiricilerin dikkat kaydırabilirsiniz.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Özel kapsayıcı kayıt defterine erişim isteği

[!INCLUDE [Request access to private container registry](../../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Create a Cognitive Services Face resource](../includes/create-face-resource.md)]

[!INCLUDE [Create an Face container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]