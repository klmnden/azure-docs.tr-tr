---
title: "Hataları yeniden çalışma sırasında şirket içi Azure'dan sorun giderme ve Azure'a daha sonra koruyun | Microsoft Docs"
description: "Bu makalede Azure ve yeniden koruma sırasında başarısız olan şirket içi dön içinde sık karşılaşılan sorunları giderme yolları"
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/19/2017
ms.author: rajanaki
ms.openlocfilehash: d487c1fcf7fb6444c2b8489df839a6450715ae0a
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshoot-errors-reported-during-the-process-of-failback"></a>Yeniden çalışma işlemi sırasında bildirilen hatalarında sorun giderme
Yeniden çalışma temelde iki ana adımdan oluşur. Bir Azure sanal makineleri şirket içi yeniden korumak için ikinci gerçekte yeniden çalışma Azure'dan şirket içi gerçekleştirilecek.

## <a name="troubleshoot-errors-when-reprotecting-a-virtual-machine-back-to-on-premises-after-failover"></a>Bir sanal makineye geri şirket içi, yük devretme sonrasında yeniden korumayı zaman hatalarında sorun giderme
Aşağıdaki hatalardan biri, Azure sanal makinesinin yeniden koruma işlemi gerçekleştirilirken alabilirsiniz. Sorunu gidermek için her bir hata koşulu için açıklanan adımları kullanın.


### <a name="error-code-95226"></a>Hata kodu 95226

*Azure sanal makinesi şirket içi yapılandırma sunucusuna erişemediği için olmadığından yeniden koruma başarısız oldu.*

Böyle olduğunda 
1. Azure sanal makinesi şirket içi yapılandırma sunucusuna erişemediği ve bu nedenle değil bulunmalı ve yapılandırma sunucusuna kayıtlı. 
2. Post yük devretme iletişim kurmak için şirket içi yapılandırma sunucusuna çalıştırılması gerekiyor Azure sanal makinede Inmage Scout uygulama hizmeti çalışmıyor.

Bu sorunu gidermek için
1. Sanal makine şirket içi yapılandırma sunucusu ile iletişim kurabilmesi gibi Azure sanal makinesinin ağ yapılandırıldığından emin olmanız gerekir. Bunu yapmak için şirket içi veri merkeziniz dön siteden siteye VPN ayarlamak veya özel Azure sanal makinenin sanal ağ eşleme ile bir ExpressRoute bağlantı yapılandırabilirsiniz. 
2. Azure sanal makinesi şirket içi yapılandırma sunucusu ile iletişim kurabilen sağlayacak şekilde yapılandırılmış bir ağ zaten varsa, sanal makinede oturum açın ve 'Inmage Scout Application Service' denetleyin. Inmage Scout Application Service çalışmadığını görüyorsanız, hizmeti elle başlatın ve hizmet başlangıç türünü otomatik olarak ayarlandığından emin olun.

### <a name="error-code-78052"></a>Hata kodu 78052
Yeniden koruma hata iletisiyle başarısız olur: *sanal makine için koruma tamamlanamadı.*

Bu iki nedenden kaynaklanabilir
1. Sanal makineyi yeniden korumayı bir Windows Server 2016 ' dir. Şu anda bu işletim sistemi için yeniden çalışma desteklenmez, ancak yakında desteklenecek.
2. Zaten var. ana aynı ada sahip bir sanal makine hedef sunucunun geri başarısız oluyor.

Bu sorunu gidermek için yeniden koruma makine nerede adları değil birbiriyle çakışır farklı bir konak üzerinde oluşturacak böylece başka bir ana bilgisayardaki farklı bir ana hedef sunucusu seçebilirsiniz. Ayrıca, VMotion'ı farklı bir konak adı çakışma değil nerede olacağını ana hedefe erişebilirsiniz. Var olan sanal makine parazit makine ise, böylece yeni bir sanal makine aynı ESXi ana bilgisayarda oluşturulan yalnızca onu yeniden adlandırabilirsiniz.

### <a name="error-code-78093"></a>Hata kodu 78093

*VM, askıdaki bir durumda veya erişilebilir çalışmıyor.*

Sanal makineye geri şirket içi üzerinden başarısız koruyun çalıştıran Azure sanal makine gerekir. Mobility hizmetinin yapılandırma sunucusuyla kaydeder böylece budur şirket içi ve işlem sunucusu ile iletişim kurarak çoğaltma başlatabilirsiniz. Makine yanlış bir ağ veya (askıda durumu veya kapatma) çalışmıyor ise, yapılandırma sunucusunu yeniden koruma başlamak için sanal makinede mobility hizmeti ulaşamıyor. Geri şirket içi iletişim başlatılabilmesi sanal makineyi yeniden başlatabilirsiniz. Azure sanal makinesini başlattıktan sonra yeniden koruma işi yeniden başlatın

### <a name="error-code-8061"></a>Hata kodu 8061

*Veri deposu ESXi ana bilgisayardan erişilebilir değil.*

Başvurmak [ana hedef ön koşullar](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) ve [destek datastores](site-recovery-how-to-reprotect.md#what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback) yeniden çalışma için


## <a name="troubleshoot-errors-when-performing-a-failback-of-an-azure-virtual-machine-back-to-on-premises"></a>Bir Azure sanal makinenin geri şirket içi yeniden çalışma gerçekleştirirken hatalarında sorun giderme
Aşağıdaki hatalardan biri, bir Azure sanal makinesi şirket içi dön geri dönmesi gerçekleştirilirken alabilirsiniz. Sorunu gidermek için her bir hata koşulu için açıklanan adımları kullanın.

### <a name="error-code-8038"></a>Hata kodu 8038

*Şirket içi sanal makineyi hata nedeniyle getirilemedi*

Bu, şirket içi sanal makine sağlanan yeterli belleğe sahip değil bir konakta getirilene ortaya çıkar.

Bu sorunu gidermek için

1. Daha fazla bellek ESXi ana bilgisayarda sağlayabilirsiniz.
1. VMotion'ı sanal makine önyüklemesi için yeterli belleğe sahip başka bir ESXi ana VM'ye.