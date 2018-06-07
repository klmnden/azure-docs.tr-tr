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
ms.date: 05/23/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 4ea23ed01e6432f24024d7e8cc07c2dfe42ac639
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605578"
---
# <a name="the-azure-stack-marketplace-overview"></a>Azure yığın Market genel bakış

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Market, hizmetleri, uygulamaları ve Azure yığını için özelleştirilmiş kaynakları koleksiyonudur. Kaynaklar, ağlar, sanal makineler, depolama vb. içerir. Kullanıcılar yeni uygulamalar yeni kaynaklar oluşturma ve dağıtma için buraya gelin. Bunu, kullanıcıların göz atın ve bunlar kullanmak istediğiniz öğeleri seçin bir alışveriş katalog olarak düşünün. Market öğesi kullanmak için kullanıcılar öğesine erişimi veren bir teklif için abone olmalısınız.

Bir Azure yığın operatör olarak eklemek için öğeleri karar verin (Marketinde yayımlama). Veritabanları, uygulama hizmetleri ve benzeri gibi yayımlayabilirsiniz. Yayımlama bunlar tüm kullanıcılar için görünür olur. Oluşturduğunuz özel öğeler yayımlayabilirsiniz. Bir büyümesini öğeleri de yayımlayabilirsiniz [Azure Market öğelerinin listesini](azure-stack-marketplace-azure-items.md). Kullanıcılar Markete bir öğe yayımladığınızda, beş dakika içinde görebilir.

> [!Caution]  
> Görüntüleri ve json dosyaları bilinen tüm galeri öğesi yapıları Azure yığın marketi'ndeki kullanılabilir hale getirme sonra kimlik doğrulaması olmadan erişilebilir. Özel Market öğesi yayımlarken daha fazla konuları için bkz: [oluşturma ve yayımlama Market öğesi](azure-stack-create-and-publish-marketplace-item.md).

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

