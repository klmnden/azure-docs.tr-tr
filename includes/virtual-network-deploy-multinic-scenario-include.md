---
title: include dosyası
description: include dosyası
services: virtual-network
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 04/13/2018
ms.date: 06/11/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: d20ef44fd5c117e4e3a568542bb022c451ac23fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743276"
---
## <a name="scenario"></a>Senaryo
Bu belgenin birden çok NIC Vm'leri belirli bir senaryoyu kullanan bir dağıtım gösterilmektedir. Bu senaryoda, Azure'da barındırılan iki katmanlı Iaas iş yükü vardır. Her katman, kendi alt ağındaki bir sanal ağdaki (VNet) içinde dağıtılır. Ön uç katmanı için yüksek kullanılabilirlik kümesi bir yük dengeleyici birlikte gruplandırılan birden çok web sunucuları oluşur. Çeşitli veritabanı sunucularının arka uç katmanı oluşur. İki NIC içeren her bir veritabanı erişim yönetimi için diğer veritabanı sunucularına dağıtılır. Senaryo, dağıtımda ağ güvenlik her alt ağa hangi trafiklere izin denetlemek için grupları (Nsg'ler) ve NIC de içerir. Aşağıdaki resimde, bu senaryo temel mimarisini gösterir:

![MultiNIC senaryosu](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)