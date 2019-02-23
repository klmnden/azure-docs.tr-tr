---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: 03ec8740a4cf36bf3d09dade8a24b155c09d1299
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56741581"
---
Kapsayıcı ayarları hiyerarşik ve ana bilgisayarda tüm kapsayıcıları paylaşılan bir hiyerarşi.

Ayarları belirtmek için aşağıdakilerden birini kullanabilirsiniz:

* [Ortam değişkenleri](#environment-variable-settings)
* [Komut satırı bağımsız değişkenleri](#command-line-argument-settings)

Ortam değişkeni değerlerini, kapsayıcı görüntüsü için varsayılan değerleri sırayla geçersiz komut satırı bağımsız değişkeni değerlerini geçersiz kılar. Bir ortam değişkeni ve bir komut satırı bağımsız değişkeni aynı yapılandırma ayarı için farklı değerler belirtirseniz, ortam değişkeninin değeri örneklenmiş kapsayıcı tarafından kullanılır.

|Öncellik|Konum ayarlama|
|--|--|
|1|Ortam değişkeni| 
|2|Komut Satırı|
|3|Kapsayıcı görüntüsü varsayılan değeri|

### <a name="environment-variable-settings"></a>Ortam değişkeni ayarlarının

Ortam değişkenlerini kullanmanın avantajları şunlardır:

* Birden çok ayarlar yapılandırılabilir.
* Birden çok kapsayıcı aynı ayarları kullanabilirsiniz.

### <a name="command-line-argument-settings"></a>Komut satırı bağımsız değişkeni ayarları

Her kapsayıcı farklı ayarlar kullanabileceğiniz komut satırı bağımsız değişkenleri kullanmanın faydası olur.
