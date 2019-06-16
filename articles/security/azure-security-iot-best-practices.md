---
title: Nesnelerin interneti en iyi güvenlik uygulamaları | Microsoft Docs
description: Makalede, Microsoft Internet şeyleri en iyi güvenlik uygulamaları ve genel öneriler düzenlenmiş bir listesini sağlar.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: 9413c0503c1b78550776d1c2f6ab8239205a788b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62121634"
---
# <a name="internet-of-things-security-best-practices"></a>Nesnelerin interneti en iyi güvenlik uygulamaları

Nesnelerin interneti (IOT) altyapısını koruma ile IOT çözümleri dahil herkes için önemli bir iştir. Dahil olan cihazların sayısını ve bu cihazlara dağıtılmış doğası nedeniyle milyonlarca IOT cihazı'nın güvenliğini aşmak için bir güvenlik olayı ilgili etkisi önemsiz değil ve yaygın bir etkisi olabilir.

Bu nedenle, IOT güvenlik ayrıntılı güvenlik yaklaşımı gerekiyor. Verileri bulutta güvenli olması gerekir ve özel ve ortak ağlar üzerinden taşır. Yöntemi, IOT cihazları kendilerini güvenli bir şekilde sağlamak için karşılanması gerekir. Her katman, bir CİHAZDAN ağına arka uç bulut güçlü güvenlik Güvenceleri gerekir.

IOT en iyi uygulamalar, aşağıdaki şekilde kategorilere ayrılabilir:

* IOT donanım üreticisinden veya entegratörü
* IOT çözümü geliştirme
* IOT çözüm dağıtıcı
* IOT çözümü işleci

Bu makalede özetlenir [Internet, şeyleri en iyi güvenlik uygulamaları](../iot-suite/iot-security-best-practices.md). Bu makalede daha ayrıntılı bilgi için bkz.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IOT donanım üreticisinden veya entegratörü

Bir IOT donanım üretim veya donanım entegratörü varsa aşağıdaki en iyi uygulamaları izleyin:

* **Kapsam en düşük gereksinimlere donanım**: donanım tasarımı, donanım ve hiçbir şey daha fazla işlem için gereken en düşük özellikleri içermelidir. 
* **Kavram değiştirmesine donanım olun**: cihaz, vb. bir parçası kaldırma cihaz Kapak açma gibi donanım, fiziksel oynama algılamak için mekanizmaları oluşturun. 
* **Güvenli donanım yapı**: varsa [COGS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) izin, güvenli ve şifrelenmiş depolama ve Güvenilir Platform Modülü TPM tabanlı önyükleme işlevleri gibi güvenlik özelliklerini oluşturun.
* **Yükseltmeler güvenli hale getirmek**: cihaz yaşam süresi boyunca bellenimi yükseltmek kaçınılmazdır.

## <a name="iot-solution-developer"></a>IOT çözümü geliştirme

Bir IOT çözümü geliştiriciyseniz, aşağıdaki en iyi uygulamaları izleyin:

* **Güvenli bir yazılım geliştirme metodolojisinin izleyin**: güvenli bir yazılım geliştirme, en başından başlayarak, uygulama, test ve dağıtımı için tüm proje yeni güvenlik düşünmek gerektirir.
* **Açık kaynak yazılım dikkatle seçin**: açık kaynak yazılım çözümlerinizi hızla geliştirin fırsatı sağlar.
* **Dikkatli tümleştirme**: birçok yazılım güvenlik açıkları, kitaplıklarını ve API'lerini sınırında mevcut. 

## <a name="iot-solution-deployer"></a>IOT çözüm dağıtıcı

Bir IOT çözümü dağıtıcı varsa aşağıdaki en iyi uygulamaları izleyin:

* **Donanım güvenli bir şekilde dağıtmak**: IOT dağıtımları gibi ortak alanları ya da Denetimsiz yerel ayarlara güvenli olmayan konumlardaki dağıtılacak donanım gerektirebilir.
* **Kimlik doğrulama anahtarlarını güvende tutun**: dağıtım sırasında her bir cihaz cihaz kimlikleri gerektirir ve kimlik doğrulaması anahtarları bulut hizmeti tarafından oluşturulan ilişkili. Bu anahtarları dağıtımdan sonra bile, fiziksel olarak güvenli tutun. Güvenliği aşılmış bir tuşa kötü amaçlı bir cihaz tarafından geçici var olan bir cihaz için kullanılabilir.

## <a name="iot-solution-operator"></a>IOT çözümü işleci

Bir IOT çözümü işleci varsa aşağıdaki en iyi uygulamaları izleyin:

* **Sistemlerinizi güncel korumayı**: cihaz işletim sistemleri ve tüm cihaz sürücülerini en son sürümüne güncelleştirildiğinden emin olun. 
* **Kötü amaçlı etkinliklere karşı korumaya**: işletim sistemi izin veriyorsa, en son virüsten koruma ve kötü amaçlı yazılımdan koruma özelliklerinden her cihaz işletim sistemine yerleştirin. 
* **Sık denetim**: güvenlik sorunları olduğunda anahtarı güvenlik olaylarını yanıtlama ilgili IOT altyapısı denetimi.
* **Fiziksel olarak IOT altyapısını koruma**: cihazlara fiziksel erişim kullanarak IOT altyapısı kötü güvenlik saldırıları başlatılabilir.
* **Bulut kimlik bilgilerini koruyun**: Bulut kimlik doğrulaması yapılandırma ve işletim IOT dağıtım için kullanılan kimlik bilgileri olan büyük olasılıkla erişmek ve bir IOT sisteminin riske en kolay yolu. 

