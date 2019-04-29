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
ms.openlocfilehash: 1de7221f100077e07a2211bdb94e0198b35cb77c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60776342"
---
## <a name="deployment-considerations"></a>Dağıtma konuları

* N serisi Vm'lerde kullanılabilirliği için bkz: [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).

* N serisi sanal makineler yalnızca Resource Manager dağıtım modelinde dağıtılabilir.

* N serisi sanal makineler, Azure depolama disklerini için destekledikleri türünde farklılık gösterir. NC ve NV VM'ler yalnızca standart Disk Depolama (HDD) tarafından desteklenen VM disklerini destekler. NCv2, NCv3, ND, NDv2 ve NVv2 VM'ler yalnızca Premium Disk depolamayı (SSD) tarafından desteklenen VM disklerini destekler.

* Birkaç taneden fazla N serisi sanal makineler dağıtmak istiyorsanız, bir Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerini göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

* (Bölge başına), Azure aboneliğinizdeki çekirdek kotasını artırmak ve NC, NCv2, NCv3, ND, NDv2, NV veya NVv2 çekirdek için ayrı Kotayı artırmak gerekebilir. Bir kota artırım talebinde bulunmak [bir çevrimiçi müşteri destek isteği açın](../articles/azure-supportability/how-to-create-azure-support-request.md) ücret olmadan. Varsayılan sınır, abonelik kategorisine bağlı olarak değişiklik gösterebilir.




