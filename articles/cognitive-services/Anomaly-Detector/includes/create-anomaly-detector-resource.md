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
ms.openlocfilehash: b40f1833f08074cb0a8d45fe3afc6bac7cbac7f0
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717111"
---
## <a name="create-an-anomaly-detector-resource"></a>Bir Anomali algılayıcısı kaynağı oluşturun

1. Oturum [Azure portalı](https://portal.azure.com)
1. Tıklayın [Oluştur **Anomali algılayıcısı** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) kaynak
1. Tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Ad|İstenen ad (2-64 karakter)|
    |Subscription|Uygun aboneliği seçin|
    |Location|Herhangi bir yakındaki ve kullanılabilir konumu seçin|
    |Fiyatlandırma Katmanı|`F0` -en az bir fiyatlandırma katmanı|
    |Kaynak Grubu|Kullanılabilir kaynak grubu seçin|
    |Önizleme onay kutusunu (gerekli)|Olup olmadığını okuduğunuz **Önizleme** dikkat edin|

1. Tıklayın **Oluştur** ve kaynak oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin
1. Toplama yapılandırılmış `endpoint` ve API anahtarı:

    |Portal'daki kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Uç nokta kopyalayın. Benzer şekilde görünür `https://westus2.api.cognitive.microsoft.com/`|
    |**anahtarları**|API anahtarı|İki anahtar 1 kopyalayın. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|



