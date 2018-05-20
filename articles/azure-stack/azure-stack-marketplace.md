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
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 7b1a6020fb8730aee7ed41d8c82358db0945e4ef
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="the-azure-stack-marketplace-overview"></a>Azure yığın Market genel bakış

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Market, hizmetleri, uygulamaları ve Azure yığını için özelleştirilmiş kaynakları koleksiyonudur. Kaynaklar, ağlar, sanal makineler, depolama vb. içerir. Kullanıcılar yeni uygulamalar yeni kaynaklar oluşturma ve dağıtma için buraya gelin. Bunu, kullanıcıların göz atın ve bunlar kullanmak istediğiniz öğeleri seçin bir alışveriş katalog olarak düşünün. Market öğesi kullanmak için kullanıcılar öğesine erişimi veren bir teklif için abone olmalısınız.

Bir Azure yığın operatör olarak eklemek için öğeleri karar verin (Marketinde yayımlama). Veritabanları, uygulama hizmetleri ve benzeri gibi yayımlayabilirsiniz. Yayımlama bunlar tüm kullanıcılar için görünür olur. Oluşturduğunuz özel öğeler yayımlayabilirsiniz. Bir büyümesini öğeleri de yayımlayabilirsiniz [Azure Market öğelerinin listesini](azure-stack-marketplace-azure-items.md). Kullanıcılar Markete bir öğe yayımladığınızda, beş dakika içinde görebilir.

Market açmak için Yönetici konsolunda seçin **yeni**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Marketplace öğeleri
Bir Azure yığın Market hizmeti, uygulama veya kullanıcılarınızın indirin ve kullanın kaynak öğedir. Tüm Azure yığın Market öğesi planları ve teklifleri gibi yönetim öğelerini de dahil olmak üzere tüm kullanıcılara görünür. Bu öğeler, bir abonelik görünümü ancak olan kullanıcılara işlevsel gerekmez.

Her bir Market öğe vardır:

* Kaynak sağlama için bir Azure Resource Manager şablonu
* Dizeleri, simgeler ve diğer pazarlama malzemeleri gibi meta veriler
* Portalda öğeyi görüntülemek için bilgi biçimlendirme

Market'te yayımlanan her öğenin Azure galeri paketi (.azpkg) biçimi kullanır. Dağıtım veya çalışma zamanı kaynakları (örneğin, kod, ZIP dosyalarıyla yazılım ya da sanal makine görüntülerini) Azure yığınına Market öğesi bir parçası değil ayrı olarak ekleyin. 

Azure veya ne zaman özel resimler karşıya yüklediğinizde 1803 ve sonraki sürüm ile Azure yığın görüntüleri seyrek dosyaları dönüştürür. Bu işlem zaman ekler görüntü ekleme, ancak alanı kaydeder ve bu görüntüleri dağıtımını hızlandırır. Dönüştürme, yalnızca yeni yansımaları için geçerlidir.  Var olan görüntüleri değiştirilmez. 

## <a name="next-steps"></a>Sonraki adımlar
[Market öğesi indirin](azure-stack-download-azure-marketplace-item.md)  
[Oluşturma ve bir Market öğesi yayımlama](azure-stack-create-and-publish-marketplace-item.md)

