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
ms.openlocfilehash: fb82e31d58d9955efc3b147eccf2b82b8768aeee
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165795"
---
# <a name="cloudsimple-nodes-overview"></a>CloudSimple düğümleri genel bakış

Bir düğümü olduğu:

* Adanmış bir çıplak bilgisayardan konak VMware ESXi hiper yöneticisine yüklendiği işlem  
* Özel Bulutlar oluşturmak için bir birim sağlayabileceğiniz bilgi işlem veya ayrılmış  
* Sağlama veya CloudSimple hizmetin kullanılabildiği bir bölgede yer ayırmak kullanılabilir

Düğüm, özel bulut yapı taşlarıdır.  Özel bir bulut oluşturmak için en az üç düğüm aynı SKU'ların gerekir.  Özel bir buluta genişletmek için ek düğümler ekleyin.  Mevcut bir kümeye düğüm ekleyebilirsiniz. Veya Azure portalında düğümleri sağlama ve bunları CloudSimple hizmetiyle ilişkilendirme yeni bir küme oluşturabilirsiniz.  Sağlanan tüm düğümleri CloudSimple hizmetinin altında görünür.  Sağlanan düğümlerinden CloudSimple portalında özel bir bulut oluşturun.

## <a name="provisioned-nodes"></a>Sağlanan düğümleri

Sağlanan düğümleri Kullandıkça Öde kapasitesi sağlar. Düğümleri sağlama, hızlı bir şekilde isteğe bağlı olarak VMware kümenizi ölçeklendirme yardımcı olur. Düğümleri gerektiği gibi ekleyin veya VMware kümenizin sağlanan bir düğümü sil. sağlanan düğümleri aylık olarak faturalandırılır ve burada sağlanan abonelik için ücret:

* Kredi kartı ile Azure aboneliğinizin ödeme yaparsanız kart hemen faturalandırılır.
* Fatura ile faturalandırılırsınız ücretleri, bir sonraki fatura üzerinde görünür.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>VMware çözümüyle CloudSimple düğümleri SKU

Aşağıdaki türler düğümleri sağlama veya ayırma için kullanılabilir.

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

* Bilgi edinmek için nasıl [düğümleri sağlama](create-nodes.md)
* Hakkında bilgi edinin [özel bulut](cloudsimple-private-cloud.md)