---
title: Azure Stack (bulut işleci) bir özel Market öğesi yayımlama | Microsoft Docs
description: Azure Stack operatör Azure Stack'te bir özel Market öğesi yayımlama hakkında bilgi edinin.
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
ms.date: 09/12/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: c16d8a282d489e7a2b5ee9908f52224aea6118d6
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713400"
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

