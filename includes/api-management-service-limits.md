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
ms.openlocfilehash: 609eaa25640e74ffe3b39606051edd7048f72497
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57251875"
---
| Kaynak | Sınır |
| --- | --- |
| Ölçek birimi sayısı | Bölge başına 10<sup>1</sup> |
| Önbellek boyutu | Birim başına 5 GB<sup>2</sup> |
| Eş zamanlı arka uç bağlantı<sup>3</sup> HTTP yetkilisi başına | Birim başına 2048<sup>4</sup> |
| Önbelleğe alınan en büyük yanıt boyutu | 2MB |
| En fazla ilke belge boyutu | 256KB<sup>5</sup> | 
| Hizmet örneği başına en fazla özel ağ geçidi etki alanı<sup>6</sup> | 20 |
| Ca sertifikaları hizmet örneği başına en fazla sayısı | 10 | 
| Hizmet örnekleri abonelik başına en fazla sayısını<sup>7</sup> | 20 | 
| Hizmet örneği başına abonelik sayısı<sup>7</sup> | 500 |
| İstemci sertifikaları hizmet örneği başına en fazla sayısını<sup>7</sup> | 50 | 
| Hizmet örneği başına en fazla sayısını API'leri<sup>7</sup> | 50 | 
| Hizmet örneği başına API işlemlerinin maksimum sayısı<sup>7</sup> | 1000 | 
| Maksimum toplam istek süresi<sup>7</sup> | 30 saniye | 
| En fazla yükü boyutu arabelleğe<sup>7</sup> | 2MB | 


<sup>1</sup> ölçeklendirme sınırları fiyatlandırma katmanına bağlıdır. Fiyatlandırmayı görmek için katmanları ve onların ölçeklendirme sınırlarını Git [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup> birim önbelleği fiyatlandırma katmanında boyutuna bağlıdır. Fiyatlandırmayı görmek için katmanları ve onların ölçeklendirme sınırlarını Git [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup> bağlantıları havuza ve açıkça arka uç tarafından kapatıldı sürece yeniden kullanılır.<br/>
<sup>4</sup> temel, standart ve Premium katmanlarının birim başına. Geliştirici katmanı için 1024 sınırlıdır. Tüketim katmanı için geçerli değildir.<br/> 
<sup>5</sup> , temel, standart ve Premium katmanları. Tüketimini katmanı İlkesi belge boyutu için 4 KB sınırlıdır.<br/>
<sup>6</sup> yalnızca Premium katmanda kullanılabilir.<br/>
<sup>7</sup> yalnızca tüketim katmanı için geçerlidir.<br/>



