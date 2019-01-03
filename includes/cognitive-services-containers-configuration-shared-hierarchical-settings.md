---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: e2c8b4e74933df593dbc023dcb886dff80554a1a
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977396"
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
