---
title: "Azure kapsayıcı örnekleri bölge ve kaynak kullanılabilirliğini | Azure belgeleri"
description: "Hangi Azure bölgeleri için bu örnek kapsayıcı örnekleri ve CPU ve bellek sınırları dağıtımını destekleyen bulur."
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 2b9b1b864bbfd73383759212dd7d91f8e4941544
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Azure kapsayıcı örnekleri için bölge kullanılabilirliği

Önizleme sırasında Azure kapsayıcı örnekleri belirtilen CPU ve bellek sınırları aşağıdaki bölgelerde kullanılabilir.

| Konum | İşletim Sistemi | CPU | Bellek (GB) |
| -------- | -- | :---: | :-----------: |
| Batı Avrupa, Batı ABD, Doğu ABD | Linux | 2 | 7 |
| Batı Avrupa, Batı ABD, Doğu ABD | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Kaynak kullanılabilirliği

Bu kaynak sınırları içinde oluşturulan kapsayıcı örnek dağıtım bölge içindeki kullanılabilirlik tabidir. Bir bölge ağır yük altında olduğunda örnekleri dağıtırken bir hatayla karşılaşabilirsiniz.

Bu tür bir dağıtım hatası azaltmak için daha düşük CPU ve bellek ayarlarını örnekleriyle dağıtmayı deneyin veya dağıtımınızı daha sonra deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcı örnek dağıtım sorunlarını giderme hakkında daha fazla bilgi için bkz: [Azure kapsayıcı örnekleri dağıtım sorunlarını giderme](container-instances-troubleshooting.md).