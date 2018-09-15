---
title: Azure Stack (bulut işleci) bir özel Market öğesi yayımlama | Microsoft Docs
description: Azure Stack operatör Azure Stack'te bir özel Market öğesi yayımlama hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 12310c088777d65bef211747806f942433857e40
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632357"
---
# <a name="the-azure-stack-marketplace-overview"></a>Azure Stack Marketini genel bakış

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Market Hizmetleri, uygulamaları ve Azure Stack için özelleştirilmiş bir kaynaklar koleksiyonudur. Kaynaklar, ağları, sanal makineler, depolama vb. içerir. Kullanıcılar yeni kaynaklar oluşturmak ve yeni uygulamalar dağıtmak için buraya gelir. Bunu, kullanıcıların nereden göz atabilir ve kullanmak istediğiniz öğeleri seçin bir alışveriş katalog olarak düşünün. Bir Market öğesi kullanmak için kullanıcılar bunları öğesine erişimi veren bir teklife abone olması gerekir.

Azure Stack operatör eklenecek öğeleri karar verin (Market'te yayımlama). Veritabanları, uygulama hizmetleri ve benzeri gibi yayımlayabilirsiniz. Yayımlama bunları tüm kullanıcılara görünür hale getirir. Oluşturduğunuz özel öğeler yayımlayabilirsiniz. Büyüyen öğesinden öğeleri de yayımlayabilirsiniz [Azure Market öğeleri listesi](azure-stack-marketplace-azure-items.md). Kullanıcıların bir öğe Market'te yayımladığınızda, beş dakika içinde görebilirsiniz.

> [!Caution]  
> Tüm galeri öğesi yapıtları görüntüleri ve json dosyaları bilinen, Azure Stack Market'te kullanılabilir hale getirme sonra kimlik doğrulama olmadan erişilebilir. Özel Market öğesi yayımlama sırasında daha fazla konular için bkz [oluşturun ve bir Market öğesi yayımlama](azure-stack-create-and-publish-marketplace-item.md).

Market açmak için Yönetici konsolunda seçin **+ kaynak Oluştur**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Marketplace öğeleri
Bir Azure Stack Marketini öğe, bir hizmet, uygulama veya kullanıcılarınızın indirip kullanabilirsiniz kaynak değil. Planlar ve teklifler gibi yönetim öğelerini de dahil olmak üzere tüm kullanıcılarınız, tüm Azure Stack Marketini öğeleri görülebilir. Bu öğelerin görünüm, ancak bunlar kullanıcılara işlevsel aboneliği gerektirmez.

Her bir Market öğesi sahiptir:

* Kaynak sağlama için bir Azure Resource Manager şablonu
* Dizeleri, simgeler ve diğer pazarlama Güvenceleri gibi meta verileri
* Portalda öğeyi görüntülemek için biçimlendirme bilgileri

Market'te her öğe, Azure galeri paketi (.azpkg) biçimini kullanır. Azure Stack, dağıtım veya çalışma zamanı kaynakları (örneğin, kod, yazılım ya da sanal makine görüntüleri zip dosyaları) Market öğesi bir parçası olarak değil ayrı ayrı ekleyin. 

Azure veya özel görüntüleri karşıya yüklemek, indirmek, 1803 ve üzeri sürümü ile Azure Stack görüntüleri seyrek dosyalarına dönüştürür. Bu işlem süresini uzatır görüntü eklerken, ancak kazandırır ve bu görüntülerin dağıtımını hızlandırır. Dönüştürme, yalnızca yeni görüntüleri için geçerlidir.  Var olan görüntülerden değiştirilmez. 

## <a name="next-steps"></a>Sonraki adımlar
[Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)  
[Bir Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)

