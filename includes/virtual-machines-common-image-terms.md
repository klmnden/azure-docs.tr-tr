---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 50d71a3967e61e5d531f4bfeae3582b56230a0d2
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188364"
---
## <a name="terminology"></a>Terminoloji

Azure Market görüntüsü aşağıdaki özniteliklere sahiptir:

* **Yayımcı**: Görüntüyü oluşturan kuruluş. Örnekler: Canonical, MicrosoftWindowsServer
* **Teklif**: Bir yayımcı tarafından oluşturulan ilgili görüntülerin bir grup adı. Örnekler: UbuntuServer, WindowsServer
* **SKU**: Bir dağıtımın ana sürümü gibi bir teklif örneği. Örnekler: 18.04 LTS, 2019 veri merkezi
* **Sürüm**: SKU görüntü sürümü sayısı. 

Program aracılığıyla VM dağıtırken bir Market görüntüsü tanımlamak için bu değerleri tek tek parametre olarak sağlayın. Bazı araçları bir görüntü kabul *URN*, iki nokta üst üste (:) karakteriyle ayrılmış, bu değerleri birleştirir: *Yayımcı*:*teklif*:*Sku*:*sürüm*. Bir URN içinde sürüm numarasıyla "son", görüntünün en son sürümünü seçer değiştirebilirsiniz. 

Ardından görüntü yayımcısı, ek lisans ve satın alma koşulları sağlıyorsa, söz konusu hükümleri kabul ve program dağıtımı etkinleştir gerekir. Ayrıca sağlamanız gerekir *satın alma planı* program aracılığıyla bir VM dağıtılırken parametreleri. Bkz: [görüntüyü Market koşullarıyla dağıtma](#deploy-an-image-with-marketplace-terms).
