---
title: include dosyası
description: include dosyası
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: d20ef44fd5c117e4e3a568542bb022c451ac23fc
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188297"
---
## <a name="scenario"></a>Senaryo
Bu belgenin birden çok NIC Vm'leri belirli bir senaryoyu kullanan bir dağıtım gösterilmektedir. Bu senaryoda, Azure'da barındırılan iki katmanlı Iaas iş yükü vardır. Her katman, kendi alt ağındaki bir sanal ağdaki (VNet) içinde dağıtılır. Ön uç katmanı için yüksek kullanılabilirlik kümesi bir yük dengeleyici birlikte gruplandırılan birden çok web sunucuları oluşur. Çeşitli veritabanı sunucularının arka uç katmanı oluşur. İki NIC içeren her bir veritabanı erişim yönetimi için diğer veritabanı sunucularına dağıtılır. Senaryo, dağıtımda ağ güvenlik her alt ağa hangi trafiklere izin denetlemek için grupları (Nsg'ler) ve NIC de içerir. Aşağıdaki resimde, bu senaryo temel mimarisini gösterir:

![MultiNIC senaryosu](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

