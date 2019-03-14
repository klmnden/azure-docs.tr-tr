---
title: Uygulamalar - Azure Batch işleme
description: Önceden yüklenmiş işleme uygulamalarıyla
services: batch
ms.service: batch
author: laurenhughes
ms.author: lahugh
ms.date: 12/11/2018
ms.topic: conceptual
ms.openlocfilehash: 8ce430d83c52014b3d1d3d2a985f74aeb488caea
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791894"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>VM görüntüleri işleme üzerinde önceden yüklenmiş uygulamalar

Azure Batch ile tüm işleme uygulamalarını kullanmak mümkündür. Ancak, önceden yüklenmiş ortak uygulamalarla Azure Market VM görüntülerini kullanılabilir.

Kullanım başına ödeme lisanslama uygunsa önceden yüklenmiş işleme uygulamaları için kullanılabilir. Bir Batch havuzu oluştururken, gerekli uygulamaları belirtilebilir ve maliyeti, VM ve uygulamaların dakika başına faturalandırılır. Uygulama fiyatları listelenen [Azure fiyatlandırma sayfasını Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

Bazı uygulamalar, yalnızca Windows destekler, ancak çoğu, hem Windows hem de Linux üzerinde desteklenir.

## <a name="applications-on-centos-7-rendering-nodes"></a>CentOS 7 işleme düğümleri üzerinde uygulamalar

* Autodesk Maya I/O 2017 Güncelleştirme 5 (cut 201708032230)
* Autodesk Maya I/O 2018 güncelleştirme 2'de (201711281015 kes)
* Autodesk Arnold for Maya 2017 (Arnold sürüm 5.0.1.1) MtoA-2.0.1.1-2017
* Autodesk Arnold for Maya 2018 (Arnold sürüm 5.0.1.4) MtoA-2.1.0.3-2018
* Chaos Group V-Ray for Maya 2017 (sürüm 3.60.04)
* Chaos Group V-Ray for Maya 2018 (sürüm 3.60.04)
* Blender (2.68)

## <a name="applications-on-windows-server-2016-rendering-nodes"></a>Windows Server 2016 işleme düğümleri üzerinde uygulamalar

* Autodesk Maya I/O 2017 Güncelleştirme 5 (sürüm 17.4.5459)
* Autodesk Maya g/ç 2018 güncelleştirme 4 (sürüm 18.4.0.7622)  
* Autodesk 3ds Max g/ç 2019 güncelleştirme 1'de (sürüm 21.2.0.2219)
* Autodesk 3ds Max I/O 2018 Güncelleştirme 4 (sürüm 20.4.0.4254)
* Autodesk Arnold Maya 2017 (sürüm 5.2.0.1 Arnold) MtoA 3.1.0.1 2017
* Autodesk Arnold (Arnold sürümü 5.2.0.1) Maya 2018 MtoA 3.1.0.1 2018 için
* Autodesk Arnold for 3ds Max (sürüm 5.0.2.4)(version Arnold 1.2.926)
* Chaos Group V-Ray for Maya (sürüm 3.52.03)
* Chaos Group V-Ray for 3ds Max (sürüm 3.60.02)
* Blender (2.79)

## <a name="next-steps"></a>Sonraki adımlar

İşleme VM görüntülerini kullanmak için bir havuz oluşturduğunuzda havuz yapılandırmasına belirtilmesi gerekir; bkz: [toplu işleme için havuz özellikleri](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools).