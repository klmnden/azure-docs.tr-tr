---
title: Uygulamalar - Azure Batch işleme
description: Önceden yüklenmiş işleme uygulamalarıyla
services: batch
ms.service: batch
author: laurenhughes
ms.author: lahugh
ms.date: 03/26/2018
ms.topic: conceptual
ms.openlocfilehash: 84b2ca30f1ccd49e18e2f9d42133f8672d3f8ad6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60776178"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>VM görüntüleri işleme üzerinde önceden yüklenmiş uygulamalar

Azure Batch ile tüm işleme uygulamalarını kullanmak mümkündür. Ancak, önceden yüklenmiş ortak uygulamalarla Azure Market VM görüntülerini kullanılabilir.

Kullanım başına ödeme lisanslama uygunsa önceden yüklenmiş işleme uygulamaları için kullanılabilir. Bir Batch havuzu oluştururken, gerekli uygulamaları belirtilebilir ve maliyeti, VM ve uygulamaların dakika başına faturalandırılır. Uygulama fiyatları listelenen [Azure fiyatlandırma sayfasını Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

Bazı uygulamalar, yalnızca Windows destekler, ancak çoğu, hem Windows hem de Linux üzerinde desteklenir.

## <a name="applications-on-centos-7-rendering-images"></a>CentOS 7 görüntüleri işleme uygulamaları

* Autodesk Maya I/O 2017 Güncelleştirme 5 (cut 201708032230)
* Autodesk Maya I/O 2018 güncelleştirme 2'de (201711281015 kes)
* Autodesk Arnold for Maya 2017 (Arnold sürüm 5.0.1.1) MtoA-2.0.1.1-2017
* Autodesk Arnold for Maya 2018 (Arnold sürüm 5.0.1.4) MtoA-2.1.0.3-2018
* Chaos Group V-Ray for Maya 2017 (sürüm 3.60.04)
* Chaos Group V-Ray for Maya 2018 (sürüm 3.60.04)
* Blender (2.68)

## <a name="applications-on-latest-windows-server-2016-rendering-images"></a>Uygulamaları en son Windows Server 2016 görüntüleri işleme

Aşağıdaki liste, Windows Server 2016 görüntüleri sürüm 1.3.4 işleme uygular.

* Autodesk Maya I/O 2017 Güncelleştirme 5 (sürüm 17.4.5459)
* Autodesk Maya g/ç 2018 güncelleştirme 4 (sürüm 18.4.0.7622)
* Autodesk 3ds Max g/ç 2019 güncelleştirme 1'de (sürüm 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Güncelleştirme 4 (sürüm 20.4.0.4254)
* Autodesk Arnold Maya 2017 (sürüm 5.2.0.1 Arnold) MtoA 3.1.0.1 2017
* Autodesk Arnold (Arnold sürümü 5.2.0.1) Maya 2018 MtoA 3.1.0.1 2018 için
* Autodesk Arnold for 3ds Max (sürüm 5.0.2.4)(version Arnold 1.2.926)
* Chaos Group V-Ray (sürüm 3.52.03) Maya 2018 için
* Chaos Group V-Ray 3ds Max 2018'de (sürüm 3.60.02) için
* Chaos Group V-Ray Maya 2019 (sürüm 3.52.03) için
* Chaos Group V-Ray 3ds Max 2019 (sürüm 4.10.01) için
* Blender (2.79)

> [!NOTE]
> Chaos Group V-Ray 3ds Max 2019 (sürüm 4.10.01) için V-ray için bozucu değişiklikler yapılmıştır. Önceki bir sürümü (sürüm 3.60.02) kullanmak için Windows Server 2016, sürüm 1.3.2 işleme düğümleri kullanın.

## <a name="applications-on-previous-windows-server-2016-rendering-images"></a>Uygulamaları önceki Windows Server 2016 görüntüleri işleme

Aşağıdaki liste, Windows Server 2016 görüntüleri sürüm 1.3.2 işleme uygular.

* Autodesk Maya I/O 2017 Güncelleştirme 5 (sürüm 17.4.5459)
* Autodesk Maya g/ç 2018 güncelleştirme 4 (sürüm 18.4.0.7622)  
* Autodesk 3ds Max g/ç 2019 güncelleştirme 1'de (sürüm 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Güncelleştirme 4 (sürüm 20.4.0.4254)
* Autodesk Arnold Maya 2017 (sürüm 5.2.0.1 Arnold) MtoA 3.1.0.1 2017
* Autodesk Arnold (Arnold sürümü 5.2.0.1) Maya 2018 MtoA 3.1.0.1 2018 için
* Autodesk Arnold for 3ds Max (sürüm 5.0.2.4)(version Arnold 1.2.926)
* Chaos Group V-Ray Maya 2019 (sürüm 3.52.03) için
* Chaos Group V-Ray 3ds Max 2018'de (sürüm 3.60.02) için
* Blender (2.79)

## <a name="next-steps"></a>Sonraki adımlar

İşleme VM görüntülerini kullanmak için bir havuz oluşturduğunuzda havuz yapılandırmasına belirtilmesi gerekir; bkz: [toplu işleme için havuz özellikleri](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools).