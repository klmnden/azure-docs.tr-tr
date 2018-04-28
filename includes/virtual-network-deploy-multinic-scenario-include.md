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
ms.openlocfilehash: cab04a7eafbc21e0d26cd5a287f3dbee8d3d22b7
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
## <a name="scenario"></a>Senaryo
Bu belge Vm'lerde belirli bir senaryoda birden çok NIC kullanan bir dağıtımda anlatılmaktadır. Bu senaryoda, Azure üzerinde barındırılan bir iki katmanlı Iaas iş yükü vardır. Her katman kendi alt ağda bir sanal ağ (VNet) dağıtılır. Bir yük dengeleyici için yüksek kullanılabilirlik kümesi içinde bir arada gruplandırılmış birkaç web sunucuları, ön uç katmanı oluşur. Birkaç veritabanı sunucularının arka uç katmanından oluşur. İki NIC içeren her birini diğer yönetim için veritabanı erişimi için veritabanı sunucularına dağıtılır. Senaryo, dağıtımda ağ güvenlik her alt ağda hangi trafiğe izin denetlemek için grupları (Nsg'ler) ve NIC de içerir. Aşağıdaki resimde bu senaryo temel mimarisini gösterir:

![MultiNIC senaryosu](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

