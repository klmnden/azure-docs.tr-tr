---
title: Özel görüntüleri ve formülleri DevTest labs'deki karşılaştırma | Microsoft Docs
description: Gereksinimlerinize hangisinin ortamınızı karar verebilmek için VM tabanları gibi özel görüntüleri ve formülleri arasındaki farklar hakkında bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: ae7556eda817b9eb7be84f9d4a23ea91d3d5440d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64680292"
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Özel görüntüleri ve formülleri DevTest Labs ile karşılaştırma
Her ikisi de [özel görüntüleri](devtest-lab-create-template.md) ve [formülleri](devtest-lab-manage-formulas.md) bankaları için olarak kullanılabilir [oluşturulan yeni VM](devtest-lab-add-vm.md). Ancak, özel görüntüleri ve formülleri arasındaki temel fark özel bir görüntü yalnızca bir formülü bir VHD'de temel alan bir görüntü olduğu sürece bir VHD'de temel alan bir görüntü olmasıdır *ek olarak* ayarları - VM boyutunu, sanal ağ gibi önceden yapılandırılmış alt ağ ve yapıtları. Bu önceden yapılandırılmış ayarları ile VM oluşturma sırasında geçersiz kılınabilir varsayılan değerler ayarlanır. Bu makalede bazı (olumlu) olumlu ve olumsuz yönleri (olumsuz) formülleri kullanarak karşı özel görüntüleri kullanarak açıklanır.

## <a name="custom-image-pros-and-cons"></a>Özel görüntü avantajları ve dezavantajları
Özel görüntüler, istenen bir ortamdan VM'ler oluşturmak için statik, sabit bir yol sağlar. 

**Uzmanları**

* Özel bir görüntüden VM sağlama görüntüsünden VM hazırlandığında sonra hiçbir şey değiştikçe hızlıdır. Diğer bir deyişle, özel görüntü ayarları olmadan yalnızca bir görüntü olarak uygulamak için hiçbir ayar vardır. 
* Tek özel görüntüden oluşturulan sanal makineler birbirinin aynıdır.

**Simgeler**

* Özel görüntü bazı yönlerini güncelleştirmeniz gerekiyorsa, görüntü yeniden oluşturulmalıdır.  

## <a name="formula-pros-and-cons"></a>Formül avantajları ve dezavantajları
Formül, istenen yapılandırma/ayarlarla VM'ler oluşturmak için dinamik bir yol sağlar.

**Uzmanları**

* Ortamındaki değişiklikler hareket halindeyken yapıtlar aracılığıyla yakalanabilir. Örneğin, yayın işlem hattınızı son bitten ile yüklü bir VM veya en son kod deponuzdan listeleme istiyorsanız, en son parçaları dağıtır veya bir formülde bir hedefi temel ile birlikte en son kodu kaydeder bir yapıya yalnızca belirtebilirsiniz görüntü. Şu formül olarak ayarlayın, sanal makineler oluşturmak için kullanıldığında, en yeni bit/kod dağıtılan/kayıtlı VM. 
* Formül, özel görüntüleri - VM boyutları ve sanal ağ ayarları gibi sağlayamaz varsayılan ayarlarını tanımlayabilirsiniz. 
* Bir formülde kaydedilen ayarlar, varsayılan değer olarak gösterilir, ancak VM oluşturulduğunda değiştirilebilir. 

**Simgeler**

* Bir formülle bir VM oluşturma, özel bir görüntüden bir VM oluşturmaktan daha fazla zaman alabilir.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [Özel görüntü veya formül?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Sonraki adımlar
- [DevTest Labs SSS](devtest-lab-faq.md)