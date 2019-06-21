---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 09/12/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: ec6cbcbc93fe87634c87caeb0041b75ec916a22f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188341"
---
Bağlantı noktası açma veya bir alt ağ veya VM ağ arabirimi bir ağ filtresi oluşturarak Azure'da sanal makine (VM) için bir uç nokta oluşturma. Trafiği alan kaynağına bağlı bir ağ güvenlik grubu gelen ve giden trafiği denetlemek, bu filtreler yerleştirin.

Bu makaledeki örnek standart TCP bağlantı noktası 80 (Bunu önceden uygun hizmetlerin çalışmaya ve VM üzerindeki tüm işletim sistemi güvenlik duvarı kurallarını açtığınız varsayılır) kullanan bir ağ filtresi oluşturma işlemini gösterir.

Standart TCP bağlantı noktası 80 üzerinde web isteklerine hizmet vermek için yapılandırılmış bir VM oluşturduktan sonra şunları yapabilirsiniz:

1. Ağ güvenlik grubu oluşturun.

2. Trafiğe izin veren bir gelen güvenlik kuralı oluşturun ve aşağıdaki ayarları için değerler atayın:

   - **Hedef bağlantı noktası aralıkları**: 80

   - **Kaynak bağlantı noktası aralıkları**: * (tüm kaynak bağlantı noktası izin verir)

   - **Öncelik değeri**: Daha az 65,500 ve daha yüksek öncelik tümünü varsayılan reddetme gelen kuralı olan bir değer girin.

3. Ağ güvenlik grubu, VM ağ arabirimi ya da alt ağı ile ilişkilendirin.

Bu örnek HTTP trafiğine izin veren basit bir kuralı kullansa da, daha karmaşık ağ yapılandırmaları oluşturmak için ağ güvenlik gruplarını ve kurallarını da kullanabilirsiniz. 




