---
title: Microsoft Azure FXT Edge dosyalayıcı genel bakış
description: Azure FXT Edge dosyalayıcı karma depolama önbelleği, etkin Arşiv ve yüksek performanslı bilgi işlem için dosya erişim accelerator çözümünün açıklar
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: overview
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: 58d4a2a52757b6ace1059fcccf379df3de5fd75c
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542896"
---
# <a name="what-is-azure-fxt-edge-filer-hybrid-storage-cache"></a>Azure FXT Edge dosyalayıcı karma depolama önbelleği nedir?

Azure FXT Edge dosyalayıcı, hızlı dosya erişim ve etkin arşiv görevler için yüksek performanslı bilgi işlem (HPC) özellikleri sağlayan Gereci önbelleğe alma karma depolamadır.

Yerel veri merkezinde, uzaktan, bulut saklanıyor olsun ister birden çok veri kaynaklarıyla çalışır. Azure FXT Edge olandan farklı depolama sistemlerinde veriler için birleşik bir ad alanı sağlar.

Üç veya daha fazla FXT Edge dosyalayıcı donanım cihazlarının önbellek sağlamak için kümelenmiş bir dosya sistemi birlikte çalışır. Gerekli donanım satın alma hakkında daha fazla bilgi için Microsoft temsilcinize başvurun. 

Daha fazla bilgi için ürün bilgileri ve veri sayfasını okuyun [Azure FXT Edge dosyalayıcı](https://azure.microsoft.com/services/fxt-edge-filer/).

## <a name="use-cases"></a>Uygulama alanları

Bunlar gibi iş akışları için en iyi üretkenlik Azure FXT Edge dosyalayıcı geliştirir:

* Okuma yoğunluklu dosya erişim iş akışı 
* NFSv3 veya SMB2 protokolleri
* Grupları 100.000 CPU çekirdekleri için 1000 işlem

### <a name="nas-optimization-and-scaling"></a>NAS iyileştirme ve ölçeklendirme

Mevcut NetApp ve Dell EMC Isilon NAS sistemlerine erişimi yumuşak Azure FXT Edge dosyalayıcı Önbelleği'ni kullanabilirsiniz. Ayrıca, Azure Blob veya diğer bulut depolama istemci tarafında veri erişimi işlemleri yeniden gerek kalmadan ölçekleme sağlamak için de ekleyebilirsiniz. 

### <a name="wan-caching"></a>WAN önbelleğe alma

Azure FXT Edge dosyalayıcı, hızlı dosya erişim power kullanıcıların ihtiyaç duydukları verilere başka bir yerde depolandığında desteklemek için kullanılabilir. Yedekleme ve diğer veri yönetim sistemleri merkezi veri merkezinde tutarken erişim sağlar. 

### <a name="active-archive-in-azure-blob"></a>Azure blob'da etkin arşiv

Veri merkezinizde Azure FXT Edge dosyalayıcı ile bulut depolamaya erişim noktası gibi genişletin. 

## <a name="features"></a>Özellikler 

İki donanım modeli kullanılabilir. 

| Model | DİRHEMİ | NVMe SSD | Ağ bağlantı noktaları | 
|-------|------|----------|---------------|
| FXT 6600 | 1536 GB | 25.6 TB | 6 x 25Gb / 10Gb + 2 x 1 Gb |
| FXT 6400 | 768 GB | 12,8 TB | 6 x 25Gb / 10Gb + 2 x 1 Gb |


## <a name="next-steps"></a>Sonraki adımlar

* Okuyarak Azure FXT Edge dosyalayıcı öğrenmeye devam [belirtimleri](fxt-specs.md) veya [yükleme Öğreticisi](fxt-install.md).
* Üzerinde FXT Edge dosyalayıcı Azure satın almayla ilgili bilgi [Azure FXT Edge dosyalayıcı ürün sayfası](https://azure.microsoft.com/services/fxt-edge-filer/).