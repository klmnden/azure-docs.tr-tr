---
title: Düzeyleri yakınlaştırma ve döşeme Azure eşlemeleri kılavuzunda | Microsoft Docs
description: Yakınlaştırma düzeyleri hakkında bilgi edinin ve Azure Maps döşeme
author: jinzh-azureiot
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 55441cda7a6fc65ac8103d19510823a7c84a9cbf
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599934"
---
# <a name="zoom-levels-and-tile-grid"></a>Yakınlaştırma düzeyleri ve kutucuk kılavuzu
Azure eşlemeleri küresel Mercator projeksiyon koordinat sistemini kullanın (EPSG: 3857).

Dünya kare döşemesine ayrılmıştır. İşleme (Izgara) 0 ila 18 numaralı 19 yakınlaştırma düzeyi vardır. İşleme (vektör) 0 ile 20 numaralı 21 yakınlaştırma düzeyi vardır. Yakınlaştırma 0 düzeyinde, tek bir döşeme dünyaya uyar:

![Dünya döşeme](./media/zoom-levels-and-tile-grid/world0.png)

Yakınlaştırma düzeyini 1 world oluşturulacak dört kutucuk kullanır: 2 x 2 kare

![Dünya döşeme sol üst](./media/zoom-levels-and-tile-grid/world1a.png)     ![Dünya döşeme sağ üst](./media/zoom-levels-and-tile-grid/world1c.png) 

![Dünya döşeme sol alt](./media/zoom-levels-and-tile-grid/world1b.png)     ![Dünya döşeme alt sağ](./media/zoom-levels-and-tile-grid/world1d.png) 


Her sonraki yakınlaştırma düzeyini dört-2 oluşan bir kılavuz oluşturma öncekinin döşeme böler<sup>yakınlaştırma</sup> x 2<sup>yakınlaştırma</sup>. Yakınlaştırma düzeyi 20 olan bir kılavuz 2<sup>20</sup> x 2<sup>20</sup>, ya da 1.048.576 x 1.048.576 döşeme (toplam 109,951,162,778 döşemeleri).

Aşağıdaki tabloda tam liste değerleri için yakınlaştırma düzeyleri sağlar:

|yakınlaştırma düzeyi|metre/piksel|metre/yan döşeme|
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
|19|0,3|76.4|
|20|0,15|38.2|

Kutucuklar, o yakınlaştırma düzeyi için kılavuz döşemesinin konumuna karşılık gelen yakınlaştırma düzeyi ve x ve y koordinatları adı verilir.

Kullanmak için hangi yakınlaştırma düzeyini belirlerken, her konum döşemesinin üzerindeki sabit bir konumda olduğunu unutmayın. Bu bölge, belirli bir expanse görüntülemek için gereken döşeme sayısını world yakınlaştırma kılavuzda belirli yerleşimini bağımlı olduğunu gösterir. Vardır işaret ediyorsa, iki örneği için 900 ölçümler birbirinden, onu *olabilir* yalnızca onları yakınlaştırma düzeyi 17 arasında bir yolu görüntülemek için üç döşeme sürer. Ancak, Batı noktası döşemesinin Doğu noktası döşemesinin sol ve sağ taraftaki ise, dört kutucuk alabilir:

![Yakınlaştırma Demo ölçeği](./media/zoom-levels-and-tile-grid/zoomdemo_scaled.png) 

Yakınlaştırma düzeyi belirlendikten sonra x ve y değerleri hesaplanabilir. Her yakınlaştırma kılavuzunda üst sol kutucuğunun x = 0, y = 0; x sağ alt bölme, 2 =<sup>-1 yakınlaştırma</sup>, y = 2<sup>yakınlaştırma-1</sup>.

Yakınlaştırma düzeyini 1 yakınlaştırma kılavuz şöyledir:

![Yakınlaştırma düzeyi 1 için yakınlaştırma kılavuz](./media/zoom-levels-and-tile-grid/api_x_y.png)
