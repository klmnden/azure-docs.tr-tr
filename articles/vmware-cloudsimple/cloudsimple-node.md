---
title: CloudSimple - Azure ile VMware çözümü için düğümleri genel bakış
description: CloudSimple düğümleri ve kavramları hakkında bilgi edinin.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b3c8fca1dd93f379860cc3b084fbb14d4a0c6380
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64577368"
---
# <a name="cloudsimple-nodes-overview"></a>CloudSimple düğümleri genel bakış

Bir düğümü olduğu:

* Adanmış bir çıplak bilgisayardan konak VMware ESXi hiper yöneticisine yüklendiği işlem  
* Özel Bulutlar oluşturmak için bir birim, satın alabileceğiniz bilgi işlem veya ayrılmış  
* Satın alma veya CloudSimple hizmetin kullanılabildiği bir bölgede yer ayırmak kullanılabilir

Düğüm, özel bulut yapı taşlarıdır.  Özel bir bulut oluşturmak için en az üç düğüm aynı SKU'ların gerekir.  Özel bir buluta genişletmek için ek düğümler ekleyin.  Mevcut bir kümeye düğüm ekleyebilirsiniz. Veya Azure portalında düğümleri satın alma ve bunları CloudSimple hizmetiyle ilişkilendirme yeni bir küme oluşturabilirsiniz.  Satın alınan tüm düğümleri CloudSimple hizmetinin altında görünür.  Özel bir bulut CloudSimple portalında satın alınan düğümlerden oluşturun.

## <a name="purchased-nodes"></a>Satın alınan düğümleri

Satın alınan düğümleri Kullandıkça Öde kapasitesi sağlar. Düğümleri satın alma, isteğe bağlı olarak VMware kümenizi ölçeği hızla yardımcı olur. Düğümleri gerektiği gibi ekleyin veya VMware kümenizin satın alınan bir düğümü sil. Satın alınan düğümleri aylık olarak faturalandırılır ve burada satın aboneliğine ücret:

* Kredi kartı ile Azure aboneliğinizin ödeme yaparsanız kart hemen faturalandırılır.
* Fatura ile faturalandırılırsınız ücretleri, bir sonraki fatura üzerinde görünür.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>VMware çözümüyle CloudSimple düğümleri SKU

Aşağıdaki türler düğümler, satın alma veya ayırma için kullanılabilir.

| SKU | CS28 - düğüm | CS36 - düğüm |
|-----|-------------|-------------|
| CPU | 2x2.2 GHz, 28 Çekirdeği (56 HT) | 2x2.3 GHz ve 36 çekirdek (72 HT) |
| RAM | 256 GB | 512 GB |
| Önbellek diski |  1.6 TB NVMe | 3.2-TB NVMe |
| Disk kapasitesi | 5.625 ham TB | 11,25 TB ham |
| Depolama türü | Tüm Flash | Tüm Flash |

## <a name="limits"></a>Limits

Aşağıdaki düğüm sınırlar özel Bulutlar için geçerlidir.

| Resource | Sınır |
|----------|-------|
| Özel bir bulut oluşturmak için düğüm sayısı alt sınırı | 3 |
| En fazla özel bulut üzerinde bir kümedeki düğüm sayısını | 16 |
| En fazla özel bulutta düğüm sayısı | 64 |
| Yeni kümede düğüm sayısı alt sınırı | 3 |

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [satın düğümleri](create-nodes.md)
* Hakkında bilgi edinin [özel bulut](cloudsimple-private-cloud.md)