---
title: Konuşma öğrenen varsayılan yapılandırması - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Varsayılan konuşma öğrenen yapılandırma hakkında bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 56e2140b83bf1c5722a459c14f31b2b4b0ba6b15
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353950"
---
# <a name="default-values-and-boundaries"></a>Varsayılan değerler ve sınırlar

Bu belge konuşma öğrenen ve anahtar hizmet sınırları varsayılan yapılandırmasını açıklar.

## <a name="default-configuration"></a>Varsayılan yapılandırma

Parametre | Varsayılan değer
--- | --- 
Varsayılan oturum zaman aşımı | 30 dakika

## <a name="boundaries"></a>Sınırları

Parametre | Sınır
--- | --- 
API geliştirme, max HTTP ayda çağırır | 5M
API yazma, saniye başına en fazla HTTP çağırır | 25
Oturum API, her ay max HTTP çağrıları | 500K
Oturum API, saniye başına en fazla HTTP çağrıları | 10
Uygulama başına özel (programlı olmayan) varlıkların sayısı üst sınırı | Bkz: [HALUK sınırları belge](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries); uygulamada, gerçek sayı biraz daha küçük olmalıdır
Uygulama başına önceden derlenmiş varlıkların sayısı üst sınırı | Bkz: [HALUK sınırları belge](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-boundaries)
Varlıklarda (toplam) uygulama başına maksimum sayısı | 100
Uygulama başına Eylemler sayısı üst sınırı | 32
Tren iletişim kutuları uygulama başına maksimum sayısı | 1000
Tren iletişim en fazla kullanıcı sayısı kapatır | 100
Uygulama başına günlük iletişim kutuları sayısı üst sınırı | Yalnızca hiçbir önceden ayarlanmış sınırı, ancak günlük iletişim kutuları atılmadan önce sabit bir süre için korunur.  Ayrıca, konuşma öğrenen UI bir kerede 100 günlük iletişim kutuları gösterir. 
Uygulama kullanıcı başına maksimum sayısı | Önceden ayarlanmış bir sınırlama yoktur
Sıralı olmayan bekleme Eylemler sayısı üst sınırı | 5 (*)

(*) 5 sıralı bekleme olmayan Eylemler sonra tüm bekleme olmayan eylemleri maskelenmiş ve konuşma öğrenen kullanılabilir bekleme eylemler arasında seçeceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma öğrenen ile çalışmaya başlama](./quickstart.md)
