---
title: include dosyası
description: include dosyası
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 03/22/2018
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: b40e2ba448236bcef72ffe501b4b14fa0dd64e1c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31434948"
---
| Kaynak | Sınır |
| --- | --- |
| Ölçek birimleri | Bölge başına 10<sup>1</sup> |
| Önbellek | Birim başına 5 GB<sup>1</sup> |
| Eşzamanlı arka uç bağlantı<sup>2</sup> HTTP yetkilisi başına | Birim başına 2048<sup>3</sup> |
| Önbelleğe alınan en büyük yanıt boyutu | 10MB |
| En fazla ilke belge boyutu | 256KB |
| En fazla özel ağ geçidi etki alanları | Hizmet örneği başına 20<sup>4</sup> |


<sup>1</sup>API Management sınırları her fiyatlandırma katmanının için farklıdır. Fiyatlandırma görmek için katmanlarına ve bunların ölçeklendirme sınırlarını Git [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).
<sup>2</sup> bağlantıları havuza alınmış ve arka uç tarafından açıkça kapalı sürece yeniden kullanılır.
<sup>3</sup> temel, standart ve Premium katmanlar, birim başına. Geliştirici katmanı için 1024 sınırlıdır.
<sup>4</sup> yalnızca Premium katmanında kullanılabilir.


