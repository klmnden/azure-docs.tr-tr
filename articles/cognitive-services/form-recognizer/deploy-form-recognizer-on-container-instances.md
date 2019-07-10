---
title: Azure Container Instances'ı çalıştırma
titleSuffix: Azure Cognitive Services
description: Azure Container Instance için Form tanıyıcı kapsayıcı dağıtın ve bir web tarayıcısında test.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 3c424465678a9989940d92910c5d288fa2fb1cab
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711400"
---
# <a name="deploy-the-form-recognizer-container-to-azure-container-instances"></a>Form tanıyıcı kapsayıcıyı Azure Container Instances'a dağıtacaksınız.

Bilişsel hizmetler dağıtmayı öğrenirsiniz [Form tanıyıcı](form-recognizer-container-howto.md) Azure kapsayıcısına [Container Instances](https://docs.microsoft.com/azure/container-instances/). Bu yordamı, bir Azure Form tanıyıcı kaynak oluşturulmasını gösterir. Ardından ilişkili kapsayıcı görüntüsünü çekme tartışın. Son olarak, orchestration bir tarayıcıdan iki çalışma olanağı vurgulayın. Kapsayıcıları kullanarak, bunun yerine, uygulama geliştirme odaklanarak altyapı Yönetimi işlerini hayatınızdan çıkarın, geliştiricilerin dikkat kaydırabilirsiniz.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteği

Önce tamamlamanız ve gönderme gerekir [Bilişsel Hizmetleri Form tanıyıcı kapsayıcılara erişimine istek formunu](https://aka.ms/FormRecognizerRequestAccess) kapsayıcıya erişim istemek için. Bunun yapılması için görüntü işleme, imzalar. Görüntü işleme istek formu için ayrı ayrı oturum gerek yoktur. 

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Form Recognizer resource](includes/create-resource.md)]

[!INCLUDE [Create an Form Recognizer container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
