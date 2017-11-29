---
title: "Yük devretme Azure hataları için sorun giderme | Microsoft Docs"
description: "Bu makalede Azure yapabilmesini içinde sık karşılaşılan sorunları giderme yolları"
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: pratshar
ms.openlocfilehash: 5e1f9a0298c2abd542d7687778716f644a1d0a47
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Bir sanal makineye Azure üzerinden başarısız olduğunda hatalarında sorun giderme
Aşağıdaki hatalardan biri, Azure için sanal makinenin yük devretme yaparken alabilirsiniz. Sorunu gidermek için her bir hata koşulu için açıklanan adımları kullanın.


## <a name="failover-failed-with-error-id-28031"></a>Yük devretme 28031 hata Koduyla başarısız oldu

Site Recovery azure'da sanal makine üzerinde başarısız oluşturmak mümkün değildi. Aşağıdaki nedenlerden biri nedeniyle oluşabilir:

* Sanal makine oluşturmak için kullanılabilir yeterli kotası yok: aboneliğine giderek kullanılabilir kota denetleyebilirsiniz -> kullanım + kotalar. Açabilirsiniz bir [yeni destek isteği](http://aka.ms/getazuresupport) Kotayı artırmak için.
     
* Yük devretme sanal makineler aynı kullanılabilirlik kümesinde farklı boyutu ailelerinin deniyorsunuz. Aynı kullanılabilirlik kümesinde tüm sanal makineler için aynı boyutu ailesi seçtiğinizden emin olun. Sanal makinenin işlem ve ağ ayarlarına giderek boyutunu değiştirin ve sonra Yük devretme işlemini yeniden deneyin.
  
* Bir sanal makineye oluşturulmasını engeller abonelikte bir ilke yoktur. Bir sanal makine oluşturulmasına izin ve yük devretme yeniden denemek için ilkeyi değiştirin. 

## <a name="failover-failed-with-error-id-28092"></a>Yük devretme 28092 hata Koduyla başarısız oldu

Site Recovery birleştiremedi başarısız için bir ağ arabirimi oluşturmak üzere sanal makine üzerinde. Aboneliğindeki ağ arabirimlerini oluşturmak için kullanılabilir yeterli kotası olduğundan emin olun. Aboneliği giderek kullanılabilir kota denetleyebilirsiniz -> kullanım + kotalar. Açabilirsiniz bir [yeni destek isteği](http://aka.ms/getazuresupport) Kotayı artırmak için. Yeterli kotası olması durumunda bu bir aralıklı olabilir vermek, işlemi yeniden deneyin. Denemelere sorun devam ederse, bu belgenin sonuna bir yorum bırakın.  

## <a name="failover-failed-with-error-id-70038"></a>Yük devretme 70038 hata Koduyla başarısız oldu

Site Recovery, azure'da Klasik sanal makine üzerinde başarısız oluşturmak bırakamıyor. Nedeniyle oluşabilir:

* Oluşturulacak sanal makine için gerekli olan bir sanal ağ gibi kaynaklardan biri yok. Sanal makinenin işlem ve ağ ayarlarının altında sağlanan sanal ağ oluşturun veya zaten var olduğundan ve yük devretme yeniden sanal bir ağa ayarı değiştirin. 


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma gereksinim duyarsanız, ardından sorgunuza ileti [Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) veya bu belgenin sonuna bir yorum bırakın. Size yardımcı olmak üzere saptayabilmelisiniz etkin bir topluluk sunuyoruz.