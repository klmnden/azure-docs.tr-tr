---
title: Özel Market öğesi Azure yığın (bulut işleci) yayımlama | Microsoft Docs
description: Bir Azure yığın operatör olarak Azure yığınında özel Market öğesi yayımlama hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 37587cf5f99ce105413382cbd5a0de9cacc76b7d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="the-azure-stack-marketplace-overview"></a>Azure yığın Market genel bakış

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Market, hizmetleri, uygulamaları ve ağlar, sanal makineler, depolama ve benzerleri gibi Azure yığını için özelleştirilmiş kaynakları koleksiyonudur. Kullanıcılar yeni uygulamalar yeni kaynaklar oluşturma ve dağıtma için buraya gelin. Bunu, kullanıcıların göz atın ve bunlar kullanmak istediğiniz öğeleri seçin bir alışveriş katalog olarak düşünün. Market öğesi kullanmak için kullanıcılar öğesine erişimi veren bir teklif için abone olmalısınız.

Bir Azure yığın operatör olarak eklemek için öğeleri karar verin (Marketinde yayımlama). Veritabanları, uygulama hizmetleri ve benzeri gibi yayımlayabilirsiniz. Bu, tüm kullanıcılar için görünür sağlar. Oluşturduğunuz özel öğeler yayımlayabilirsiniz. Bir büyümesini öğeleri de yayımlayabilirsiniz [Azure Market öğelerinin listesini](azure-stack-marketplace-azure-items.md). Kullanıcılar Markete bir öğe yayımladığınızda, beş dakika içinde görebilir.

Market'ı açmak için **yeni**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Marketplace öğeleri
Bir Azure yığın Market hizmeti, uygulama veya kullanıcılarınızın indirin ve kullanın kaynak öğedir. Tüm Azure yığın Market öğesi, tüm kullanıcılar tarafından görülebilir.

Her bir Market öğe vardır:

* Kaynak sağlama için bir Azure Resource Manager şablonu
* Dizeleri, simgeler ve diğer pazarlama malzemeleri gibi meta veriler
* Portalda öğeyi görüntülemek için bilgi biçimlendirme

Market'te yayımlanan her öğenin Azure galeri paketi (azpkg) adlı bir biçim kullanır. Dağıtım veya çalışma zamanı kaynakları (örneğin, kod, ZIP dosyalarıyla yazılım ya da sanal makine görüntülerini) Azure yığınına Market öğesi bir parçası değil ayrı olarak ekleyin. 

1803 ile başlayarak, görüntüleri seyrek dosyaları indirme azure'dan veya özel resimler karşıya yükleme sırasında dönüştürülür. Bu işlem zaman ekler görüntü ekleme, ancak alanı kaydeder ve bu görüntüleri dağıtımını hızlandırır. Dönüştürme, yalnızca yeni yansımaları için geçerlidir. Var olan görüntüleri değiştirilmez. 

## <a name="next-steps"></a>Sonraki adımlar
[Oluşturma ve bir Market öğesi yayımlama](azure-stack-create-and-publish-marketplace-item.md)

