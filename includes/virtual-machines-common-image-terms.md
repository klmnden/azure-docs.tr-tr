---
title: "include dosyası"
description: "include dosyası"
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 02/28/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 47ef014f31c628fed9d7b2b005e67e8138d3e7e9
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
## <a name="terminology"></a>Terminoloji

Azure Market görüntüsü aşağıdaki özniteliklere sahiptir:

* **Yayımcı** -görüntüyü oluşturan kuruluş. Örnekler: Kurallı, MicrosoftWindowsServer
* **Teklif** -bir yayımcı tarafından oluşturulan ilgili görüntülerinin grubunun adı. Örnekler: Ubuntu Server, Windows Server
* **SKU** - bir dağıtım büyük sürümü gibi bir teklif ait bir örnek. Örnekler: 16.04-LTS, 2016 Datacenter
* **Sürüm** -SKU görüntü sürüm numarası. 

Bir VM programlı olarak dağıttığınızda bir Market görüntüsü tanımlamak için ayrı ayrı parametre olarak bu değerleri sağlayın veya görüntü bazı araçlar kabul *URN*. İki nokta üst üste (:) karakteriyle ayrılmış bu değerleri URN birleştirir: *yayımcı*:*teklif*:*Sku*:*sürüm*. Bir URN içinde hangi görüntüsünün en son sürümü seçer sürüm numarasıyla "son" değiştirebilirsiniz. 

Görüntü Yayımlayıcı ek lisans ve satın alma koşulları sağlıyorsa, söz konusu hükümleri kabul ve programlamayla dağıtımı etkinleştirin. Sağlamanız gereken *satın alma planı* VM program aracılığıyla dağıtırken parametreleri. Bkz: [Market koşullarla Yansıma Dağıtma](#deploy-an-image-with-marketplace-terms).