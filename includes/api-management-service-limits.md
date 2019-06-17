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
ms.openlocfilehash: fc945a7e9389c8aec48a6a1dba969fbf92002d3a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66238638"
---
| Resource | Sınır |
| --- | --- |
| Ölçek birimi sayısı | Bölge başına 10<sup>1</sup> |
| Önbellek boyutu | Birim başına 5 GB<sup>2</sup> |
| Eş zamanlı arka uç bağlantı<sup>3</sup> HTTP yetkilisi başına | Birim başına 2.048<sup>4</sup> |
| Önbelleğe alınan en büyük yanıt boyutu | 2 MB |
| En fazla ilke belge boyutu | 256 KB<sup>5</sup> | 
| Hizmet örneği başına en fazla özel ağ geçidi etki alanı<sup>6</sup> | 20 |
| CA sertifikaları hizmet örneği başına en fazla sayısı | 10 | 
| Hizmet örnekleri abonelik başına en fazla sayısını<sup>7</sup> | 20 | 
| Hizmet örneği başına abonelik sayısı<sup>7</sup> | 500 |
| İstemci sertifikaları hizmet örneği başına en fazla sayısını<sup>7</sup> | 50 | 
| Hizmet örneği başına en fazla sayısını API'leri<sup>7</sup> | 50 | 
| Hizmet örneği başına API işlemlerinin maksimum sayısı<sup>7</sup> | 1000 | 
| Maksimum toplam istek süresi<sup>7</sup> | 30 saniye | 
| En fazla yükü boyutu arabelleğe<sup>7</sup> | 2 MB | 


<sup>1</sup>ölçeklendirme sınırları fiyatlandırma katmanına bağlıdır. Fiyatlandırma katmanları ve onların ölçeklendirme sınırlarını görmek için bkz: [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup>birim önbelleği fiyatlandırma katmanında boyutuna bağlıdır. Fiyatlandırma katmanları ve onların ölçeklendirme sınırlarını görmek için bkz: [API Management fiyatlandırması](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup>bağlantıları havuza ve açıkça arka uç tarafından kapatıldı sürece yeniden kullanılabilir.<br/>
<sup>4</sup>temel, standart ve Premium katmanları bu sınırı birimidir. Geliştirici katmanı için 1024 sınırlıdır. Bu sınır, tüketim katmanı için geçerli değildir.<br/> 
<sup>5</sup>bu sınırı temel, standart ve Premium katmanları için geçerlidir. Tüketim katmanında ilke belge boyutu için 4 KB sınırlıdır.<br/>
<sup>6</sup>bu kaynak yalnızca Premium katmanında kullanılabilir.<br/>
<sup>7</sup>bu kaynak tüketimi katmana uygulanır.<br/>



