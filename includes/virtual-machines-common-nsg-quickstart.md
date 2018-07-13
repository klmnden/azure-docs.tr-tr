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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38942119"
---
Bağlantı noktası açma veya bir alt ağ veya VM ağ arabirimi bir ağ filtresi oluşturarak Azure'da sanal makine (VM) için bir uç nokta oluşturma. Trafiği alan kaynağına bağlı bir ağ güvenlik grubu gelen ve giden trafiği denetlemek, bu filtreler yerleştirin.

Yaygın olarak karşılaşılan örneklerden web trafiği 80 numaralı bağlantı noktasında kullanalım. Hizmet için yapılandırılmış bir VM oluşturduktan sonra web isteklerini standart TCP bağlantı noktası 80, (unutmayın uygun hizmetleri başlatın ve VM'de de herhangi bir işletim sistemi güvenlik duvarı kuralları'nı açmak için):

1. Bir ağ güvenlik grubu oluşturun.
2. İle trafiğe izin veren bir gelen kuralı oluşturun:
   * "80" hedef bağlantı noktası aralığı
   * Kaynak bağlantı noktası aralığı "*" (tüm kaynak bağlantı verme)
   * (olması daha yüksek öncelik varsayılan catch tüm daha reddetmek için gelen kuralı) daha az 65,500 öncelik değeri
3. Ağ güvenlik grubu, VM ağ arabirimi ya da alt ağı ile ilişkilendirin.

Ağ güvenlik gruplarını ve kurallarını kullanarak ortamınızın güvenliğini sağlamak için karmaşık ağ yapılandırmaları oluşturabilirsiniz. Bizim örneğimizde, HTTP trafiğini veya uzaktan yönetimi yalnızca bir veya iki kurallarını kullanır. Daha fazla bilgi için aşağıdakilere bakın ['Daha fazla bilgi'](#more-information-on-network-security-groups) bölüm veya [bir ağ güvenlik grubu nedir?](../articles/virtual-network/security-overview.md)

