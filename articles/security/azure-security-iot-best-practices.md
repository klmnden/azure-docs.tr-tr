---
title: "En iyi güvenlik uygulamalarını, noktalar, Internet | Microsoft Docs"
description: "Makaleyi seçkin Microsoft Internet şeyler en iyi güvenlik uygulamaları ve genel öneriler listesini sağlar."
services: security
documentationcenter: na
author: TomShinder
manager: StevenPo
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: yurid
ms.openlocfilehash: 8efc0053458e338ac1afe98d9ce970c1d5cbfa81
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="internet-of-things-security-best-practices"></a>En iyi güvenlik uygulamalarını, noktalar, Internet
Nesnelerin interneti (IOT) altyapısını koruma çözümlere dahil herkes için kritik bir iş değil. Cihazı dahil sayısı ve bu cihazların dağıtılan yapı, nedeniyle bir güvenlik olayı IOT cihazları milyonlarca tehlikeye ilgili etkisi Önemsiz olmayan ve yaygın etkisi olabilir.

Bu nedenle, bir güvenlik derinlemesine yaklaşım IOT güvenlik gerekir. Verileri bulutta güvenli olması gerekir ve özel ve genel ağları üzerinden hareket ederken. Yöntemleri IOT cihazları güvenli bir şekilde sağlamak için karşılanması gerekir. Her katman, aygıttan ağına arka uç bulut güçlü güvenlik çıkışların gerekir.

IOT en iyi yöntemler aşağıdaki gibi kategorilere ayrılabilir:

* IOT donanım üreticisi veya Tümleştirici
* IOT Çözüm geliştiricisi
* IOT çözüm dağıtıcı
* IOT çözüm işleci

Bu makalede özetlenmektedir [Internet, şeyler en iyi güvenlik uygulamaları](../iot-suite/iot-security-best-practices.md). Lütfen daha ayrıntılı bilgi için bu makalesine bakın.

## <a name="iot-hardware-manufacturer-or-integrator"></a>IOT donanım üreticisi veya Tümleştirici
Bir IOT donanım üreticisine veya donanım Tümleştirici ise en iyi uygulamaları izleyin:

* **Kapsam en düşük gereksinimler için donanım**: donanım tasarımı donanım ve hiçbir şey daha fazla işlem için gereken en düşük özellikleri içermelidir. 
* **Kanıt değiştirmesine donanım olun**: fiziksel aygıt, vb. parçası kaldırarak cihaz Kapak açma gibi donanım, izinsiz algılamak için mekanizmaları oluşturun. 
* **Güvenli donanım yapı**: varsa [SMM](https://en.wikipedia.org/wiki/Cost_of_goods_sold) izin, güvenli ve şifrelenmiş depolama ve Güvenilir Platform Modülü TPM tabanlı önyükleme işlevleri gibi güvenlik özellikleri yapı.
* **Yükseltmeler güvenli hale**: cihaz ömrü boyunca bellenimi yükseltmek kaçınılmaz.

## <a name="iot-solution-developer"></a>IOT Çözüm geliştiricisi
Bir IOT çözüm geliştirici varsa en iyi uygulamaları izleyin:

* **Güvenli yazılım geliştirme metodolojisini izleyin**: Güvenli yazılım geliştirme, kendi uygulama, test ve dağıtım için tüm proje başlangıcından güvenlik başından başlayarak yukarı düşünmek gerektirir.
* **Açık kaynak yazılımının dikkatle seçin**: açık kaynak yazılımının hızla çözümleri geliştirmek için bir fırsat sağlar.
* **Dikkatli tümleştirmek**: çoğu yazılım güvenlik açıkları kitaplıklarını ve API'lerini sınırında mevcut. 

## <a name="iot-solution-deployer"></a>IOT çözüm dağıtıcı
Bir IOT çözüm dağıtıcı varsa en iyi uygulamaları izleyin:

* **Donanım güvenli bir şekilde dağıtmak**: IOT dağıtımları ortak alanları ya da Denetimsiz yerel ayarlara olduğu gibi güvenli olmayan konumlarda dağıtılacak donanım gerektirebilir.
* **Kimlik doğrulama anahtarlarını güvenli kalmasına**: dağıtım sırasında her cihaz cihaz kimlikleri gerektirir ve kimlik doğrulaması anahtarları bulut hizmeti tarafından oluşturulan ilişkili. Bu anahtarları daha sonra dağıtım fiziksel olarak güvenli tutun. Güvenliği aşılmış bir tuşa kötü amaçlı bir aygıt tarafından geçici var olan bir cihaz için kullanılabilir.

## <a name="iot-solution-operator"></a>IOT çözüm işleci
Bir IOT çözüm işleç varsa en iyi uygulamaları izleyin:

* **Sistemleri güncel tutmak**: cihaz işletim sistemleri ve tüm aygıt sürücülerini en son sürümüne güncelleştirilir emin olun. 
* **Kötü amaçlı etkinliği karşı koruma**: işletim sistemi izin veriliyorsa, her cihaz işletim sisteminde en son virüsten koruma ve kötü amaçlı yazılımdan özellikleri yerleştirin. 
* **Sık denetim**: güvenlik sorunları olduğunda anahtar güvenlik olaylarını yanıtlama ilgili IOT altyapı denetleme.
* **Fiziksel olarak IOT altyapısını koruma**: cihazlara fiziksel erişim kullanarak IOT altyapı kötü güvenlik saldırıları başlatılabilir.
* **Bulut kimlik bilgileri korumaya**: Bulut kimlik doğrulaması kimlik bilgilerini yapılandırmak ve bir IOT dağıtım işletim için kullanılan olan büyük olasılıkla erişebilir ve bir IOT sistemden yapmanın en kolay yolu. 

