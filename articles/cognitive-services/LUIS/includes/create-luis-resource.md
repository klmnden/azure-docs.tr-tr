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
ms.openlocfilehash: 52d8e1355558b197b193a50c7cde571799541268
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717179"
---
## <a name="create-a-luis-resource"></a>LUIS kaynak oluştur

1. Oturum [Azure portalı](https://portal.azure.com)
1. Tıklayın [oluşturma **dil anlama**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUIS)
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
    |**Genel bakış**|Uç Nokta|Uç nokta kopyalayın. Benzer şekilde görünür `https://luis.cognitiveservices.azure.com/luis/v2.0`|
    |**anahtarları**|API anahtarı|İki anahtar 1 kopyalayın. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
