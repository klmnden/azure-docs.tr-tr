---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/17/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: d9c8a0e6a3bd6d79a11ee0d0dab0500a209e5571
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34684991"
---
Bir bağlantı noktasını açmak veya bir alt ağ veya VM ağ arabirimine bir ağ filtre oluşturarak Azure'da sanal makine (VM) için bir uç nokta oluşturun. Trafiği alır kaynağa bağlı bir ağ güvenlik grubu hem gelen hem de giden trafiği denetleyen bu filtreler yerleştir.

Bağlantı noktası 80 üzerinde web trafiği yaygın bir örneği kullanalım. Sunmak için yapılandırılmış bir VM olduktan sonra web isteklerinde standart TCP bağlantı noktası 80, (unutmayın uygun hizmetleri başlatmak ve tüm işletim sistemi güvenlik duvarı kurallarını VM'de de açmak için):

1. Ağ güvenlik grubu oluşturun.
2. Trafiğe izin bir gelen kuralı oluşturun:
   * "80" hedef bağlantı noktası aralığı
   * Kaynak bağlantı noktası aralığını "*" (tüm kaynak bağlantı izin verme)
   * daha az 65,500 (olması önceliği varsayılan catch tümünü daha yüksek reddetmek için gelen kuralı) öncelik değeri
3. Ağ güvenlik grubu VM ağ arabirimine veya alt ağ ile ilişkilendirin.

Ağ güvenlik gruplarını ve kurallarını kullanarak ortamınızın güvenliğini sağlamak için karmaşık ağ yapılandırmaları oluşturabilirsiniz. Bizim örneğimizde HTTP trafiği veya uzaktan yönetim izin yalnızca bir veya iki kurallarını kullanır. Daha fazla bilgi için aşağıdakilere bakın ['Daha fazla bilgi'](#more-information-on-network-security-groups) bölüm veya [bir ağ güvenlik grubu nedir?](../articles/virtual-network/security-overview.md)

