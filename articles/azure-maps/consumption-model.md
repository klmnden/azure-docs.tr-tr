---
title: Azure Maps tüketim modelini | Microsoft Docs
description: Azure Maps tüketim modeli hakkında bilgi edinin
author: subbarayudukamma
ms.author: skamma
ms.date: 05/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: cf20c7dbfbf7cd3f09579b03b835148c1c295137
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600638"
---
# <a name="consumption-model"></a>Tüketim modeli

Çevrimiçi yönlendirme araç özgü tüketim modelini ayrıntılı bir açıklaması için bir parametre kümesi sağlar.
Değerine bağlı olarak **vehicleEngineType**, iki asıl tüketim modelleri desteklenmektedir: _yanmalı_ ve _elektrik_. Aynı istekte farklı modele ait parametrelerini belirten bir hata var.
Tüketim modelini birlikte kullanılamaz **travelMode** değerleri _bisiklet_ ve _Yaya_.

## <a name="parameter-constraints-for-consumption-model"></a>Tüketim modelini parametresi kısıtlamaları

Her iki tüketim modellerinde bazı parametreler açıkça belirtilmesi diğerlerinin de belirtilmesi gerekir. Bu bağımlılıklar şunlardır:

* Tüm parametreler gerektiren **constantSpeedConsumption** kullanıcı tarafından belirtilmelidir. Dışında herhangi diğer tüketim modelini parametresini belirtmek için bir hata olduğunu **vehicleWeight**, **constantSpeedConsumption*** belirtilmedi.
* **accelerationEfficiency** ve **decelerationEfficiency** (yani her ikisini birden veya hiçbiri) bir çift her zaman belirtilmesi gerekir.
* Varsa **accelerationEfficiency** ve **decelerationEfficiency** belirtilirse, bunların değerleri çarpımını (perpetual hareket önlemek için) 1'den büyük olmamalıdır.
* **uphillEfficiency** ve **downhillEfficiency** (yani her ikisini birden veya hiçbiri) bir çift her zaman belirtilmesi gerekir.
* Varsa **uphillEfficiency** ve **downhillEfficiency** belirtilirse, bunların değerleri çarpımını (perpetual hareket önlemek için) 1'den büyük olmamalıdır.
* Varsa \* **verimliliği** parametreleri belirtildi kullanıcı tarafından sonra **vehicleWeight** de belirtilmesi gerekir. Zaman **vehicleEngineType** olan _yanmalı_, **fuelEnergyDensityInMJoulesPerLiter** de belirtilmesi gerekir.
* **maxChargeInkWh** ve **currentChargeInkWh** (yani her ikisini birden veya hiçbiri) bir çift her zaman belirtilmesi gerekir.

> [!NOTE]
> Yalnızca **constantSpeedConsumption** belirtilirse, hiçbir tüketim yönleriyle slopes ve araç hızlandırma gibi tüketim hesaplamaları için dikkate alınır.

## <a name="combustion-consumption-model"></a>Yanmalı tüketim modeli

Yanmalı tüketim modeli kullanılan zaman **vehicleEngineType** ayarlanır _yanmalı_.
Bu modele ait parametre listesi olan aşağıda. Ayrıntılı açıklama parametreleri bölümüne bakın.

* constantSpeedConsumptionInLitersPerHundredkm
* VehicleWeight
* currentFuelInLiters
* auxiliaryPowerInLitersPerHour
* fuelEnergyDensityInMJoulesPerLiter
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="electric-consumption-model"></a>Elektrik tüketim modeli

Elektrik tüketim modeli kullanılan zaman **vehicleEngineType** ayarlanır _elektrik_.
Bu modele ait parametre listesi olan aşağıda. Ayrıntılı açıklama parametreleri bölümüne bakın.

* constantSpeedConsumptionInkWhPerHundredkm
* VehicleWeight
* currentChargeInkWh
* maxChargeInkWh
* auxiliaryPowerInkW
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="sensible-values-of-consumption-parameters"></a>Tüketim parametrelerinin duyarlı değerleri

Yukarıda belirtilen tüm açık gereksinimleri karşılamanız olsa bile, belirli bir tüketim parametreleri kümesini reddedilebilir. Belirli bir parametre veya birkaç parametrelerinin değerleri bileşimini değerini tüketim değerleri için aşırı magnitudes için sağlama kabul oluşuyor. Bu durumda, uygun bakım tüketim parametrelerinin tüm duyarlı değerlerini alacak şekilde gerçekleştirilir gibi büyük olasılıkla Giriş bir hata gösterir. Belirli bir tüketim parametreleri kümesini reddedilir durumda ekteki hata iletisine nedenlerden metinsel açıklaması içerir.
Parametrelerin ayrıntılı açıklamaları için her iki modeli duyarlı değeri örnekleri vardır.
