---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 1a9c4dc5a4d21f8837bde171283cd8a070297674
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47043871"
---
## <a name="deployment-considerations"></a>Dağıtma konuları

* N serisi Vm'lerde kullanılabilirliği için bkz: [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).

* N serisi sanal makineler yalnızca Resource Manager dağıtım modelinde dağıtılabilir.

* N serisi sanal makineler, Azure depolama disklerini için destekledikleri türünde farklılık gösterir. NC ve NV VM'ler yalnızca standart Disk Depolama (HDD) tarafından desteklenen VM disklerini destekler. NCv2, NCv3, ND ve NVv2 VM'ler yalnızca Premium Disk depolamayı (SSD) tarafından desteklenen VM disklerini destekler.

* Birkaç taneden fazla N serisi sanal makineler dağıtmak istiyorsanız, bir Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerini göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

* (Bölge başına), Azure aboneliğinizdeki çekirdek kotasını artırmak ve NC, NCv2, NCv3, ND, NV veya NVv2 çekirdek için ayrı Kotayı artırmak gerekebilir. Bir kota artırım talebinde bulunmak [bir çevrimiçi müşteri destek isteği açın](../articles/azure-supportability/how-to-create-azure-support-request.md) ücret olmadan. Varsayılan sınır, abonelik kategorisine bağlı olarak değişiklik gösterebilir.




