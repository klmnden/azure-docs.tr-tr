---
title: "Hızlı Başlangıç: Python ve Azure REST API'si ile bir görüntüdeki yüzleri algılayın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir resimdeki yüz algılama için Python ile Azure yüz REST API'sini kullanır.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 02/06/2019
ms.author: pafarley
ms.openlocfilehash: db7c3da7d9fff0aa604a73e541be19afc0033a57
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56309039"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-python"></a>Hızlı Başlangıç: Yüz tanıma REST API'si ve Python ile bir resimdeki yüz algılama

Bu hızlı başlangıçta, bir resimdeki İnsan yüzlerini algılamak için Python ile Azure yüz REST API'sini kullanır. Betik çerçevelerinin çevresinde yüzleri çizmek ve cinsiyet ve yaş bilgi görüntüsüne eklemek.

![Bir adam ile bir kadın, her biri kendi yüzleri ve yaş ve görüntünün üzerinde görüntülenen seks çizilmiş bir dikdörtgen](../images/labelled-faces-python.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 


## <a name="prerequisites"></a>Önkoşullar

- Yüz tanıma API'si abonelik anahtarı. Ücretsiz deneme aboneliği anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yüz tanıma API'si hizmete abone ve anahtarınızı alın.

## <a name="run-the-jupyter-notebook"></a>Jupyter not defteri çalıştırma

[MyBinder](https://mybinder.org)’da Jupyter not defteri olarak bu hızlı başlangıcı çalıştırabilirsiniz. Bağlayıcı başlatmak için aşağıdaki düğmeyi seçin. Not defterindeki yönergeleri izleyin.

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=FaceAPI.ipynb)

## <a name="next-steps"></a>Sonraki adımlar

Ardından, desteklenen senaryolar hakkında daha fazla bilgi edinmek için yüz API başvuru belgeleri keşfedin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API’si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
