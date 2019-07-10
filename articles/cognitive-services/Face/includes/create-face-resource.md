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
ms.openlocfilehash: 3886777b283af35e84683480a59097584b537fea
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711587"
---
## <a name="create-an-face-resource"></a>Yüz tanıma kaynak oluştur

1. Oturum [Azure portalı](https://portal.azure.com)
1. Tıklayın [Oluştur **yüz** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace) kaynak
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
    |**Genel bakış**|Uç Nokta|Uç nokta kopyalayın. Benzer şekilde görünür `https://face.cognitiveservices.azure.com/face/v1.0`|
    |**anahtarları**|API Anahtarı|İki anahtar 1 kopyalayın. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
