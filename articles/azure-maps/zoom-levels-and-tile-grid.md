---
title: Yakınlaştırma düzeyleri ve döşeme kılavuzunda Azure haritalar | Microsoft Docs
description: Azure haritalar, döşeme ve yakınlaştırma düzeyleri hakkında bilgi edinin
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: e7dcdb960fbd9196aca8b667269a4c6e5a1fb8f9
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59261283"
---
# <a name="zoom-levels-and-tile-grid"></a>Yakınlaştırma düzeyleri ve kutucuk kılavuzu
Azure haritalar küresel Mercator projeksiyon koordinat sistemini kullanın (EPSG: 3857).

Dünya kare kutucuğa ayrılmıştır. Azure haritalar, 0 ila 22 numaralı 23 yakınlaştırma düzeyleri için Taramalı ve vektör kutucukları sağlar. Yakınlaştırma düzeyinde 0, tek bir döşeme üzerinde dünyaya uygun:

![Dünya kutucuğu](./media/zoom-levels-and-tile-grid/world0.png)

Yakınlaştırma düzeyi 1 dünyanın işlemek için dört kutucuğa kullanır: 2 x 2 kare

![Dünya kutucuk üst sol](media/zoom-levels-and-tile-grid/world1a.png)     ![Dünya kutucuk üst sağ](media/zoom-levels-and-tile-grid/world1c.png) 

![Dünya kutucuk alt sol](media/zoom-levels-and-tile-grid/world1b.png)     ![Dünya kutucuk alt sağ](media/zoom-levels-and-tile-grid/world1d.png) 

Her ek yakınlaştırma düzeyini dört-2 kılavuzunun oluşturma Öncekine kutucuklarında böler<sup>yakınlaştırma</sup> x 2<sup>yakınlaştırma</sup>. Yakınlaştırma düzeyi 22 olan bir kılavuz 2<sup>22</sup> x 2<sup>22</sup>, veya 4,194,304 x 4,194,304 kutucuklar (toplam 17,592,186,044,416 kutucukları).

Azure haritalar etkileşimli harita denetimleri web ve Android desteği için 0 ile 24 numaralı düzeyi 25 yakınlaştırma, yakınlaştırma düzeyleri. Yol verileri yalnızca kutucukları kullanılabilir olduğunda, yakınlaştırma düzeylerinde kullanılabilse.

Aşağıdaki tabloda, yakınlaştırma düzeyi için tam liste değerleri sağlar:

|Yakınlaştırma düzeyi|Ölçümleri/piksel|Ölçümleri/dışarıdan kutucuk|
|--- |--- |--- |
|0|156543|40075008|
|1|78271.5|20037504|
|2|39135.8|10018764.8|
|3|19567.9|5009382.4|
|4|9783.9|2504678.4|
|5|4892|1252352|
|6|2446|626176|
|7|1223|313088|
|8|611.5|156544|
|9|305.7|78259.2|
|10|152.9|39142.4|
|11|76.4|19558.4|
|12|38.2|9779.2|
|13|19.1|4889.6|
|14|9.6|2457.6|
|15|4.8|1228.8|
|16|2.4|614.4|
|17|1.2|307.2|
|18|0,6|152.8|
|19|0.3|76.4|
|20|0.15|38.2|
|21|0.075|19.1|
|22|0.0375|9.55|
|23|0.01875|4.775|
|24|0.009375|2.3875|

Kutucukları kılavuz yakınlaştırma düzeyi için kutucuğun konumuna karşılık gelen yakınlaştırma düzeyi ve x ve y koordinatları tarafından çağrılır.

Kullanmak için hangi yakınlaştırma düzeyi belirlerken, her konumun kendi kutucuğundaki sabit bir konumda unutmayın. Bu bölge, belirli bir expanse görüntülemek için gereken kutucuk sayısını yakınlaştırma Kılavuzu'nun dünyanın belirli yerleşimini bağımlı olduğu anlamına gelir. Varsa iki örneği için uzaklıkta 900 ölçümleri gösteren, *olabilir* yalnızca bir yol 17 yakınlaştırma düzeyinde aralarında görüntülemek için üç kutucukları yararlanın. Ancak, Batı noktası döşemesinin döşemesinin solundaki Doğu noktasında ve sağdaki ise dört kutucuğa alabilir:

![Tanıtım ölçek büyütme](media/zoom-levels-and-tile-grid/zoomdemo_scaled.png) 

Yakınlaştırma düzeyi belirlendikten sonra x ve y değerleri hesaplanabilir. Sol üst kutucuğunda her yakınlaştırma kılavuzunda x = 0, y = 0; sağ alt kutucuğudur x 2 =<sup>-1 yakınlaştırma</sup>, y = 2<sup>yakınlaştırma 1</sup>.

Yakınlaştırma düzeyi 1 için yakınlaştırma kılavuz aşağıda verilmiştir:

![Yakınlaştırma kılavuz yakınlaştırma düzeyi 1](media/zoom-levels-and-tile-grid/api_x_y.png)
