---
title: Azure Machine Learning ve FPGAs
description: Modelleri ve derin sinir ağları FPGAs ile hızlandırmak öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.openlocfilehash: 493b3f8de617146af534349e18ed49b83c46dee8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-a-field-programmable-gate-array-fpga"></a>Bir alan programlanabilir kapısı dizisi (FPGA) nedir?

Alan programlanabilir kapısı (FPGA) gerektiği gibi yeniden tümleşik devreler dizidir. Bir FPGA yeni mantığı uygulamanız gerektiğinde değiştirebilirsiniz. Azure Machine Learning donanım hızlandırılmış modelleri eğitilmiş modeller Azure bulutta FPGAs dağıtmanıza izin verin.

Bu işlevsellik, derin sinir ağları (DNN) bir FPGA programına çevirme işleme proje Brainwave tarafından desteklenmektedir. 

## <a name="why-use-an-fpga"></a>Bir FPGA neden kullanılır?

Son derece düşük gecikme süresi FPGAs sağlayın ve toplu iş boyutu biri (büyük toplu iş boyutu gerekli değildir), veri Puanlama için özellikle etkili olur.  Bunlar, düşük maliyetli ve Azure içinde kullanılabilir.  Proje Brainwave önceden eğitilen DNNs bir hizmetin ölçeğini genişlet bu FPGAs arasında paralel hale.

## <a name="next-steps"></a>Sonraki adımlar

[Model bir FPGA üzerinde bir web hizmeti olarak dağıtma](how-to-deploy-fpga-web-service.md)

[FPGA SDK'sını yükleyin öğrenin](reference-fpga-package-overview.md)