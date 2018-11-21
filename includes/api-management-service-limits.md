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
ms.openlocfilehash: 676bd3b3a369e3f834016f626a8e69f6b41d9b23
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52170709"
---
| Kaynak | Sınır |
| --- | --- |
| Ölçek birimleri | Bölge başına 10<sup>1</sup> |
| Önbellek | Birim başına 5 GB<sup>1</sup> |
| Eş zamanlı arka uç bağlantı<sup>2</sup> HTTP yetkilisi başına | Birim başına 2048<sup>3</sup> |
| Önbelleğe alınan en büyük yanıt boyutu | 2MB |
| En fazla ilke belge boyutu | 256KB |
| En fazla özel ağ geçidi etki alanları | Hizmet örneği başına 20<sup>4</sup> |


<sup>1</sup>API Management sınırları her fiyatlandırma katmanının için farklıdır. Fiyatlandırmayı görmek için katmanları ve onların ölçeklendirme sınırlarını Git [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).
<sup>2</sup> bağlantıları havuza ve açıkça arka uç tarafından kapatıldı sürece yeniden kullanılır.
<sup>3</sup> temel, standart ve Premium katmanlarının birim başına. Geliştirici katmanı için 1024 sınırlıdır.
<sup>4</sup> yalnızca Premium katmanda kullanılabilir.


