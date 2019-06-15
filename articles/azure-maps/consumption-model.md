---
title: Azure haritalar tüketim modelinde | Microsoft Docs
description: Azure haritalar tüketim modelleri hakkında bilgi edinin
author: subbarayudukamma
ms.author: skamma
ms.date: 05/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 5f75f656312c11a4668ca9ef9fe7b2a61a7d13e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60797908"
---
# <a name="consumption-model"></a>Tüketim modeli

Çevrimiçi yönlendirme vehicle özgü kullanım modeli ayrıntılı bir açıklaması için bir parametre kümesi sağlar.
Değerine bağlı olarak **vehicleEngineType**, iki asıl tüketim modelleri desteklenmektedir: _Yanmalı_ ve _elektrik_. Aynı istekte farklı modellere ait parametrelerin belirtilmesi hataya neden olur.
Tüketim Modeli, _bicycle_ ve _pedestrian_ **travelMode** değerleriyle birlikte kullanılamaz.

## <a name="parameter-constraints-for-consumption-model"></a>Kullanım modeli için parametresi kısıtlamaları

Her iki tüketim modelleri bazı parametreler açıkça belirtilmesi bazıları de belirtilmesi gerekir. Bu bağımlılıklar şunlardır:

* Tüm parametreler gerektiren **constantSpeedConsumption** kullanıcı tarafından belirtilmelidir. Dışında herhangi diğer tüketim modelini parametresini belirtmek için bir hata olduğunu **vehicleWeight**, **constantSpeedConsumption** belirtilmedi.
* **accelerationEfficiency** ve **decelerationEfficiency** (her ikisi de veya hiçbiri) bir çift olarak her zaman belirtilmesi gerekir.
* Varsa **accelerationEfficiency** ve **decelerationEfficiency** belirtilirse, değerin çarpımını (perpetual hareket önlemek için) 1'den büyük olmamalıdır.
* **uphillEfficiency** ve **downhillEfficiency** (her ikisi de veya hiçbiri) bir çift olarak her zaman belirtilmesi gerekir.
* Varsa **uphillEfficiency** ve **downhillEfficiency** belirtilirse, değerin çarpımını (perpetual hareket önlemek için) 1'den büyük olmamalıdır.
* Varsa \* __verimliliği__ parametreleri belirtildi kullanıcı tarafından ardından **vehicleWeight** de belirtilmelidir. Zaman **vehicleEngineType** olduğu _yanmalı_, **fuelEnergyDensityInMJoulesPerLiter** de belirtilmelidir.
* **maxChargeInkWh** ve **currentChargeInkWh** (her ikisi de veya hiçbiri) bir çift olarak her zaman belirtilmesi gerekir.

> [!NOTE]
> Yalnızca **constantSpeedConsumption** belirtilirse, herhangi bir tüketim yönleri slopes ve araç hızlandırma gibi tüketim hesaplamalar için dikkate alınır.

## <a name="combustion-consumption-model"></a>Yanmalı kullanım modeli

**vehicleEngineType** değeri _combustion_ olarak ayarlandığında Combustion Tüketim Modeli kullanılır.
Bu modele ait parametrelerin listesi aşağıda verilmiştir. Ayrıntılı bir açıklaması için parametreleri bölümüne bakın.

* constantSpeedConsumptionInLitersPerHundredkm
* VehicleWeight
* currentFuelInLiters
* auxiliaryPowerInLitersPerHour
* fuelEnergyDensityInMJoulesPerLiter
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="electric-consumption-model"></a>Elektrik kullanım modeli

**vehicleEngineType** değeri _electric_ olarak ayarlandığında Electric Tüketim Modeli kullanılır.
Bu modele ait parametrelerin listesi aşağıda verilmiştir. Ayrıntılı bir açıklaması için parametreleri bölümüne bakın.

* constantSpeedConsumptionInkWhPerHundredkm
* VehicleWeight
* currentChargeInkWh
* maxChargeInkWh
* auxiliaryPowerInkW
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="sensible-values-of-consumption-parameters"></a>Tüketim parametrelerinin mantıklı değerleri

Yukarıda belirtilen tüm açık gereksinimlerini karşılayan olsa da, belirli bir tüketim parametreleri kümesini reddedilebilir. Tüketim değerlerin mantıksız massively için müşteri adayı olarak kabul değeri belirli bir parametre veya birkaç parametre değerlerinin bir birleşimini gerçekleşir. Bu durumda, tüm mantıklı tüketim parametre değerlerini uyum sağlamak için uygun bakım alınmış gibi büyük olasılıkla Giriş bir hata gösterir. Belirli bir tüketim parametreleri kümesini reddedilmesi durumunda belirten ekteki hata iletisine neden(ler) metinsel bir açıklama içerir.
Parametrelerin ayrıntılı açıklamaları için iki modeli de mantıklı değeri örnekleri vardır.
