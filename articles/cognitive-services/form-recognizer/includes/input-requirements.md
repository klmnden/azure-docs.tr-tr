---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 690219347782aad2140b0a1b0541590c5426c568
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67459740"
---
Form tanıyıcı bu gereksinimleri karşılayan giriş belgeler üzerinde çalışır:

* JPG, PNG veya PDF biçiminde olmalıdır (metin veya taranan). Metin katıştırılmış PDF karakter ayıklama ve konumda hata kaybetme riski olduğu için idealdir.
* Dosya boyutu küçüktür 4 megabayt (MB) olmalıdır.
* Görüntüler için boyutları 4200 x 4200 piksel ve 50 x 50 piksel arasında olmalıdır.
* Kağıt belgelerden taranan forms yüksek kaliteli taramalar olması gerekir.
* Metin Latin alfabesi (İngilizce karakterler) kullanmanız gerekir.
* Veri (değil resimlerdeki el yazısı) yazdırılan gerekir.
* Veri anahtarları ve değerleri içermesi gerekir.
* Anahtarları yukarıda veya değerlerin ancak değil aşağıda sola veya sağa görünür.

Form tanıyıcı şu anda bu giriş veri türlerini desteklemez:

* Karmaşık tablolar (iç içe yerleştirilmiş tablolar, birleştirilen üstbilgi veya hücre vb.).
* Onay kutusu veya radyo düğmeleri.
* PDF belgeleri 50 sayfaları uzun.