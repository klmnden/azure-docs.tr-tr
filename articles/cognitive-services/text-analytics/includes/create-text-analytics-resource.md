---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Bir bilişsel hizmetler metin analizi kaynağı oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 022ffcd806d4d4f89f8a8cf256a541518ea12602
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455139"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Bilişsel hizmetler metin analizi kaynak oluştur

1. Oturum [Azure portalı](https://portal.azure.com)
1. Seçin **+ kaynak Oluştur**, gitmek **yapay ZEKA + makine öğrenimi > metin analizi**, veya [Oluştur **metin analizi**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)
1. Tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Ad|İstenen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Location|Yakın bir konum seçin|
    |Fiyatlandırma Katmanı|`S` -Standart fiyatlandırma katmanı|
    |Kaynak Grubu|Kullanılabilir kaynak grubu seçin|

1. Tıklayın **Oluştur** ve kaynak oluşturulmasını bekleyin. Tarayıcınız oluşturulduktan sonra otomatik olarak yeni oluşturulan kaynak sayfasına yönlendirir.
1. Toplama yapılandırılmış `endpoint` ve API anahtarı:

    |Portal'daki kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Uç nokta kopyalayın. Benzer şekilde görünür `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |**anahtarları**|API Anahtarı|İki anahtar 1 kopyalayın. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|