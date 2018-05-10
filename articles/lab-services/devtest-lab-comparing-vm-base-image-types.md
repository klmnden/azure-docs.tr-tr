---
title: Özel resimler ve DevTest Labs formüller karşılaştırma | Microsoft Docs
description: Hangisinin ortamınıza en uygun karar vermem VM taban gibi özel resimler ve formülleri arasındaki farklar hakkında bilgi edinin.
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
ms.openlocfilehash: 37288fd4a9c7558d05728b8ce03df505117e0232
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Özel resimler ve DevTest Labs formüller karşılaştırma
Her ikisi de [özel resimler](devtest-lab-create-template.md) ve [formüller](devtest-lab-manage-formulas.md) için tabanları olarak kullanılan [yeni VM'ler oluşturulan](devtest-lab-add-vm.md). Ancak, özel resimler ve formülleri arasında anahtar fark özel görüntü yalnızca bir formül VHD dayanan bir görüntü olsa da bir VHD'de dayanan bir görüntü olmasıdır *ek olarak* ayarları - VM boyutunu, sanal ağ, alt ağ ve yapıları gibi önceden yapılandırılmış. Önceden yapılandırılmış bu ayarları VM oluşturma sırasında geçersiz kılınabilir varsayılan değerlerle ayarlanır. Bu makalede (uzmanları için) olumlu ve olumsuz (olumsuz) bazıları formülleri kullanma karşı özel resimler kullanmaya açıklanmaktadır.

## <a name="custom-image-pros-and-cons"></a>Özel görüntü Artıları ve eksileri
Özel resimler VM'ler istenen ortamından oluşturmak için statik, sabit bir yol sağlar. 

**Uzmanları**

* VM çalışmaya görüntüden başlar sonra hiçbir şey değiştiğinde, özel bir görüntüden VM sağlamayı hızlıdır. Diğer bir deyişle, özel görüntü ayarları olmadan yalnızca bir resim olarak uygulamak için ayar yok. 
* Tek bir özel görüntüden oluşturulan VM'ler aynıdır.

**Simgeler**

* Özel görüntü bazı yönlerinin güncellemeniz gerekiyorsa, görüntüyü yeniden oluşturulmalıdır.  

## <a name="formula-pros-and-cons"></a>Formül Artıları ve eksileri
Formülleri VM'ler istenen yapılandırma/ayarlarından oluşturmak için dinamik bir yolunu sağlar.

**Uzmanları**

* Ortam değişiklikleri yapıları aracılığıyla kolay bir şekilde yakalanır. Örneğin, en son sürüm hattınızı bitten yüklü olan bir VM istediğiniz veya en son kodu, depodan listeleme, yalnızca, bir hedef temel görüntü birlikte formülünde en son kod kaydeder veya son BITS dağıtan bir yapıya belirtebilirsiniz. Bu formülü VM'ler oluşturmak için kullanılan her durumda, en son BITS/kodu dağıtılan/kayıtlı VM. 
* Formülleri özel resimler - VM boyutları ve sanal ağ ayarları gibi sağlayamaz varsayılan ayarlarını tanımlayabilirsiniz. 
* Bir formüle, kaydedilen ayarlar, varsayılan değerler olarak gösterilir, ancak VM oluşturulduğunda değiştirilebilir. 

**Simgeler**

* Bir formülünden bir VM oluşturma, özel bir görüntüden bir VM oluşturma değerinden daha uzun sürebilir.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [Özel resimler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Sonraki adımlar
- [DevTest Labs SSS](devtest-lab-faq.md)