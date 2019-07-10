---
title: Azure Container Instances'ı çalıştırma
titleSuffix: Azure Cognitive Services
description: Bir Azure Container Instance LUIS kapsayıcı dağıtın ve bir web tarayıcısında test.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 1d19d80bbc334a376f77eb285349fb1a87a91a54
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711576"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>Language Understanding (LUIS) kapsayıcıyı Azure Container Instances'a dağıtacaksınız.

Bilişsel hizmetler dağıtmayı öğrenirsiniz [LUIS](luis-container-howto.md) Azure kapsayıcısına [Container Instances](https://docs.microsoft.com/azure/container-instances/). Bu yordam, bir Anomali algılayıcısı kaynak oluşturmayı göstermektedir. Ardından ilişkili kapsayıcı görüntüsünü çekme tartışın. Son olarak, orchestration bir tarayıcıdan iki çalışma olanağı vurgulayın. Kapsayıcıları kullanarak, bunun yerine, uygulama geliştirme odaklanarak altyapı Yönetimi işlerini hayatınızdan çıkarın, geliştiricilerin dikkat kaydırabilirsiniz.

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

[!INCLUDE [Create LUIS Container Instance resource](../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]
