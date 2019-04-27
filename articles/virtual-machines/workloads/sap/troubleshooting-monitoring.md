---
title: SAP hana (büyük örnekler) azure'da izleme | Microsoft Docs
description: SAP HANA (büyük örnekler) Azure İzleyici.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f046304121e0aed8efa1bbc2535d34186eba3496
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60713716"
---
# <a name="how-to-monitor-sap-hana-large-instances-on-azure"></a>Azure'da SAP HANA (büyük örnekler) izleme

SAP HANA (büyük örnekler) azure'da diğer bir Iaas dağıtımından farklı — işletim sistemi ve uygulama yaptığı ve nasıl uygulamaları aşağıdaki kaynakları kullanma izlemeniz gereken:

- CPU
- Bellek
- Ağ bant genişliği
- Disk alanı

Azure sanal makineler ile yukarıda adlı kaynak sınıfları yeterli olup olmadığını anlamak ihtiyacınız veya tükenmiş. İşte ayrıntılı her farklı sınıflar:

**CPU kaynak tüketimi:** SAP HANA karşı iş yükü belirli tanımlanan oranı bellekte depolanan veriler aracılığıyla çalışması yeterli CPU kaynağı olmalıdır emin olmak için uygulanır. Bununla birlikte, burada HANA eksik dizinleri veya benzer sorunlar nedeniyle sorgular yürütme birçok CPU kullanan durumlar olabilir. Başka bir deyişle, CPU kaynak tüketimi belirli bir HANA Hizmetleri tarafından tüketilen CPU kaynaklarının yanı sıra HANA büyük örnek birim izlemeniz gerekir.

**Bellek tüketimi:** HANA içinde yanı sıra dışında HANA biriminde izlemek önemlidir. HANA içinde nasıl veri HANA SAP içinde gerekli boyutlandırma yönergeleri kalmak için ayrılan bellek tüketiyor izleyin. Ayrıca, ek yüklü olmayan-HANA yazılım değil çok fazla bellek kullanmasına ve bu nedenle HANA bellek için rekabet emin olmak için en büyük örnek düzeyi bellek tüketimi izlemek istiyorsanız.

**Ağ bant genişliği:** Azure VNet ağ geçidinin bant genişliğini ne kadar yakın, Azure ağ geçidi SKU'sunu seçtiğiniz sınırları için anlamasına sanal ağ içindeki tüm Azure Vm'leri tarafından alınan verileri izlemek yararlı olacak şekilde, Azure Vnet'te giren sınırlıdır. Algılama de gelen ve giden ağ trafiğini izlemek ve zaman içinde işlenir birimleri izlemek için HANA büyük örneği biriminde kolaylaştırır.

**Disk alanı:** Disk alanı tüketimini genellikle zamanla artar. En yaygın nedenler: veri hacmi artar, işlem günlüğü yedeklemeleri, izleme dosyalarını depolamak ve depolama anlık görüntüleri gerçekleştirme yürütülmesi. Bu nedenle, disk alanı kullanımını izlemek ve HANA büyük örneği birimi ile ilişkilendirilmiş disk alanını yönetmek önemlidir.

İçin **Type II SKU'lara** HANA büyük örnekleri, sunucu önceden yüklenmiş sistem tanılama araçları ile birlikte gelir. Sistem durumu denetimi gerçekleştirmek için bu tanılama araçları kullanabilir. /Var/log/health_check sistem durumu denetimi günlük dosyasını oluşturur için aşağıdaki komutu çalıştırın.
```
/opt/sgi/health_check/microsoft_tdi.sh
```
Bir sorunu gidermek için Microsoft Support ekibi ile çalışırken, ayrıca bu tanılama araçlarını kullanarak günlük dosyalarını sağlamak istenebilir. Aşağıdaki komutu kullanarak dosyayı zip.
```
tar  -czvf health_check_logs.tar.gz /var/log/health_check
```

**Sonraki adımlar**

- Başvuru [Azure üzerinde SAP HANA (büyük örnekler) izleme](troubleshooting-monitoring.md).