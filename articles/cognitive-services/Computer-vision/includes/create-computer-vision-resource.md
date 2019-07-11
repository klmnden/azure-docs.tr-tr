---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: cbf11c13bfb5c90739ea67fab92df08796a88e50
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717273"
---
## <a name="create-an-computer-vision-resource"></a>Bir görüntü işleme kaynağı oluşturun

1. Oturum [Azure portalı](https://portal.azure.com)
1. Tıklayın [Oluştur **görüntü işleme** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision) kaynak
1. Tüm gerekli ayarları girin:

    |Ayar|Value|
    |--|--|
    |Ad|İstenen ad (2-64 karakter)|
    |Subscription|Uygun aboneliği seçin|
    |Location|Herhangi bir yakındaki ve kullanılabilir konumu seçin|
    |Fiyatlandırma Katmanı|`F0` -en az bir fiyatlandırma katmanı|
    |Kaynak Grubu|Kullanılabilir kaynak grubu seçin|

1. Tıklayın **Oluştur** ve kaynak oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin
1. Toplama yapılandırılmış `endpoint` ve API anahtarı:

    |Portal'daki kaynak sekmesi|Ayar|Value|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Uç nokta kopyalayın. Benzer şekilde görünür `https://computer-vision.cognitiveservices.azure.com/`|
    |**anahtarları**|API anahtarı|İki anahtar 1 kopyalayın. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
