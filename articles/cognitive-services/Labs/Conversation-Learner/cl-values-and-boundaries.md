---
title: Konuşma Öğrenici varsayılan yapılandırması - Microsoft Bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Varsayılan konuşma Öğrenici yapılandırma hakkında bilgi edinin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8050008bbae4a23f09b5fa94874a6315e798b448
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60688597"
---
# <a name="default-values-and-boundaries"></a>Varsayılan değerler ve sınırlar

Konuşma Öğrenici ve anahtar hizmet sınırları, varsayılan yapılandırma bu belgenin açıklar.

## <a name="default-configuration"></a>Varsayılan yapılandırma

Parametre | Varsayılan değer
--- | --- 
Varsayılan oturum zaman aşımı | 30 dakika

## <a name="boundaries"></a>Sınırlar

Parametre | Sınır
--- | --- 
API geliştirme, aylık maksimum HTTP çağrıları | 5 DK
API geliştirme, saniye başına en fazla HTTP çağrısı | 25
Oturum API, ayda en fazla HTTP çağrısı | 500K
Saniye başına en fazla HTTP oturumu API çağrısı | 10
Özel (programlı olmayan) varlıklar model başına en fazla sayısı | Bkz: [LUIS sınırları doc](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries); uygulama, gerçek sayı biraz daha küçük olmalıdır
Önceden derlenmiş varlıklar model başına en fazla sayısı | Bkz: [LUIS sınırları belge](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries)
Varlıklar (toplam) model başına en fazla sayısı | 100
Eylemler model başına en fazla sayısı | 32
Train iletişim kutuları model başına en fazla sayısı | 1000
En fazla kullanıcı sayısı train iletişim kapatır | 100
Günlük iletişim kutuları model başına en fazla sayısı | Yalnızca hiçbir önceden ayarlanan sınırı ancak günlük iletişim kutuları atılmadan önce sabit bir süre tutulur.  Konuşma Öğrenici UI Ayrıca, bir kerede 100 günlük iletişim kutuları gösterir. 
Modelleri kullanıcı başına en fazla sayısı | Önceden ayarlanmış bir sınır yoktur
Sıralı bekleme olmayan işlemlerinin maksimum sayısı | 5 (*)

(*) 5 sıralı bekleme olmayan Eylemler sonra tüm bekleme olmayan Eylemler maskelenir ve konuşma Öğrenici kullanılabilir bekleme eylemler arasında seçer.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma Öğrenici ile çalışmaya başlama](./quickstart.md)
