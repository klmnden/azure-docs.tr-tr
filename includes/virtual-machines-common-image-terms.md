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
ms.openlocfilehash: 6c7a9857f6d6d57dc9e314bcb1ef848a326b7ed2
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49437005"
---
## <a name="terminology"></a>Terminoloji

Azure Market görüntüsü aşağıdaki özniteliklere sahiptir:

* **Yayımcı**: kuruluş görüntüsü oluşturuldu. Örnekler: Canonical, MicrosoftWindowsServer
* **Teklif**: ilgili görüntüleri bir yayımcı tarafından oluşturulan bir grubun adı. Örnekler: Ubuntu Server, WindowsServer
* **SKU**: bir ana sürümüne ait bir dağıtım gibi bir teklif örneği. Örnekler: 16.04-LTS, 2016-Datacenter
* **Sürüm**: SKU görüntü sürümü sayısı. 

Program aracılığıyla VM dağıtırken bir Market görüntüsü tanımlamak için bu değerleri tek tek parametre olarak sağlayın. Bazı araçları bir görüntü kabul *URN*, iki nokta üst üste (:) karakteriyle ayrılmış, bu değerleri birleştirir: *yayımcı*:*teklif*:*Sku*: *Sürüm*. Bir URN içinde sürüm numarasıyla "son", görüntünün en son sürümünü seçer değiştirebilirsiniz. 

Ardından görüntü yayımcısı, ek lisans ve satın alma koşulları sağlıyorsa, söz konusu hükümleri kabul ve program dağıtımı etkinleştir gerekir. Ayrıca sağlamanız gerekir *satın alma planı* program aracılığıyla bir VM dağıtılırken parametreleri. Bkz: [görüntüyü Market koşullarıyla dağıtma](#deploy-an-image-with-marketplace-terms).