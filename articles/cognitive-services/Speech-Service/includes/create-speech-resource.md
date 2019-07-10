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
ms.openlocfilehash: 3c42bf2b2acc2472741bd603ea9d653a314ecc40
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711829"
---
## <a name="create-a-speech-resource"></a>Konuşma kaynak oluştur

1. Oturum [Azure portalı](https://portal.azure.com)
1. Tıklayın [Oluştur **konuşma** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices) kaynak
1. Tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Ad|İstenen ad (2-64 karakter)|
    |Subscription|Uygun aboneliği seçin|
    |Location|Herhangi bir yakındaki ve kullanılabilir konumu seçin|
    |Fiyatlandırma Katmanı|`F0` -en az bir fiyatlandırma katmanı|
    |Kaynak Grubu|Kullanılabilir kaynak grubu seçin|

1. Tıklayın **Oluştur** ve kaynak oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin
1. Toplama yapılandırılmış `endpoint` ve API anahtarı:

    |Portal'daki kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Uç nokta kopyalayın. Benzer şekilde görünür `https://speech.cognitiveservices.azure.com/sts/v1.0/issuetoken`|
    |**anahtarları**|API Anahtarı|İki anahtar 1 kopyalayın. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
