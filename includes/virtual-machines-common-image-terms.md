---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 02/28/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 47ef014f31c628fed9d7b2b005e67e8138d3e7e9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38945185"
---
## <a name="terminology"></a>Terminoloji

Azure Market görüntüsü aşağıdaki özniteliklere sahiptir:

* **Yayımcı** -kuruluş görüntüsü oluşturuldu. Örnekler: Kurallı, MicrosoftWindowsServer
* **Teklif** -ilgili görüntüleri bir yayımcı tarafından oluşturulan bir grubu adı. Örnekler: Ubuntu Server, Windows Server
* **SKU** - örnek bir ana sürümüne ait bir dağıtım gibi bir teklif. Örnekler: 16.04 LTS, 2016-Datacenter
* **Sürüm** -SKU görüntü sürümü sayısı. 

Program aracılığıyla VM dağıtırken bir Market görüntüsü tanımlamak için bu değerleri tek tek parametre olarak sağlayın veya bazı araçlar görüntüyü kabul *URN*. Bu değerleri iki nokta üst üste (:) karakteriyle ayrılmış URN birleştirir: *yayımcı*:*teklif*:*Sku*:*sürüm*. Bir URN içinde sürüm numarasıyla "son", görüntünün en son sürümünü seçer değiştirebilirsiniz. 

Görüntü yayımcısı, ek lisans ve satın alma koşulları sağlıyorsa, söz konusu hükümleri kabul ve program dağıtımı etkinleştir. Sağlamanız gereken *satın alma planı* program aracılığıyla bir VM dağıtılırken parametreleri. Bkz: [görüntüyü Market koşullarıyla dağıtma](#deploy-an-image-with-marketplace-terms).