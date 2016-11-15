---
title: Operations Management Suite (OMS) mimarisi | Microsoft Belgeleri
description: "Microsoft Operations Management Suite (OMS), şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan, Microsoft&quot;un bulut tabanlı BT yönetim çözümüdür.  Bu makalede, OMS&quot;de yer alan farklı hizmetler tanımlanmakta ve bunların ayrıntılı içeriklerinin bağlantıları sağlanmaktadır."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 40e41686-7e35-4d85-bbe8-edbcb295a534
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 88c0bd67562111baa5aa5882b7c1a4ef52bc6dd2


---
# <a name="oms-architecture"></a>OMS mimarisi
[Operations Management Suite (OMS)](https://azure.microsoft.com/documentation/services/operations-management-suite/), şirket içi ve bulut ortamlarınızın yönetilmesine yönelik bulut tabanlı hizmetler koleksiyonudur.  Bu makalede, OMS'nin farklı şirket içi ve bulut bileşenlerinin yanı sıra bunların üst düzey bulut bilgi işlem mimarisi açıklanmaktadır.  Ayrıntılı bilgi için her bir hizmete ilişkin belgelere göz atabilirsiniz.

## <a name="log-analytics"></a>Log Analytics
[Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) tarafından toplanan tüm veriler, Azure'da barındırılan OMS deposunda depolanır.  Bağlı Kaynaklar, OMS deposunda toplanan veriler oluşturur.  Şu anda desteklenen üç bağlı kaynak türü mevcuttur.

* Doğrudan OMS'ye bağlı bir [Windows](../log-analytics/log-analytics-windows-agents.md) veya [Linux](../log-analytics/log-analytics-linux-agents.md) bilgisayarda yüklü olan bir aracı.
* [Log Analytics'e bağlı](../log-analytics/log-analytics-om-agents.md) bir System Center Operations Manager (SCOM) yönetim grubu.  SCOM aracıları, olayları ve performans verilerini Log Analytics'e yönlendiren yönetim sunucuları ile iletişim kurmaya devam eder.
* Azure'daki bir çalışan rolünden, web rolünden veya sanal makineden [Azure Tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) verileri toplayan bir [Azure depolama hesabı](../log-analytics/log-analytics-azure-storage.md).

Veri kaynakları; Log Analytics'in, olay günlükleri ve performans sayaçları da dahil olmak üzere bağlı kaynaklardan topladığı verileri tanımlar.  Çözümler OMS'ye işlevsellik katar ve [OMS Çözüm Galerisi](../log-analytics/log-analytics-add-solutions.md)'nden çalışma alanınıza kolayca eklenebilir.  Bazı çözümler SCOM aracılarından Log Analytics ile doğrudan bağlantı kurulmasını gerektirirken diğer çözümler, ek bir aracının yüklenmesini gerektirebilir.

Log Analytics; OMS kaynaklarını yönetmek, OMS çözümleri eklemek ve bunları yapılandırmak, OMS deposundaki verileri görüntülemek ve çözümlemek için kullanabileceğiniz web tabanlı bir portala sahiptir.

![Log Analytics üst düzey mimarisi](media/operations-management-suite-architecture/log-analytics.png)

## <a name="azure-automation"></a>Azure Otomasyonu
[Azure Otomasyonu runbook'ları](http://azure.microsoft.com/documentation/services/automation), Azure bulutunda yürütülür; Azure'da ve diğer bulut hizmetlerinde yer alan kaynaklara erişebilir veya bu runbook'lara genel İnternet'ten erişilebilir.  Ayrıca runbook'ların yerel kaynaklara erişebilmesi için [Karma Runbook Çalışanı](../automation/automation-hybrid-runbook-worker.md)'nı kullanarak yerel veri merkezinizde şirket içi makineler atayabilirsiniz.

Azure Otomasyonu'nda depolanan [DSC yapılandırmaları](../automation/automation-dsc-overview.md), Azure sanal makinelerine doğrudan uygulanabilir.  Diğer fiziksel ve sanal makineler, yapılandırmaları Azure Automation DSC çekme sunucusundan isteyebilir.

Azure Otomasyonu, işlemler için istatistikleri ve Azure portalını başlatma bağlantılarını görüntüleyen bir OMS çözümüne sahiptir.

![Azure Otomasyonu üst düzey mimarisi](media/operations-management-suite-architecture/automation.png)

## <a name="azure-backup"></a>Azure Backup
[Azure Backup](http://azure.microsoft.com/documentation/services/backup)'ta korunan veriler belirli bir coğrafi bölgede yer alan bir yedekleme kasasında depolanır.  Veriler aynı bölge içinde çoğaltılır ve kasa türüne bağlı olarak, daha fazla yedeklilik için başka bir bölgede de çoğaltılabilir.

Azure Backup için üç temel senaryo söz konusudur.

* Azure Backup aracısının bulunduğu Windows makine.  Bu, herhangi bir Windows sunucusundaki veya istemcisindeki dosyaları ve klasörleri doğrudan Azure yedekleme kasanıza yönelik olarak yedeklemenize olanak tanır.  
* System Center Data Protection Manager (DPM) veya Microsoft Azure Backup Sunucusu. Bu, SQL ve SharePoint gibi uygulama iş yüklerinin yanı sıra dosyaları ve klasörleri yerel depolama alanında yedeklemek ve ardından Azure yedekleme kasanızda çoğaltmak için DPM veya Microsoft Azure Backup Sunucusundan yararlanmanıza olanak tanır.
* Azure Sanal Makine Uzantıları.  Bu; Azure sanal makinelerini, Azure yedekleme kasanıza yönelik olarak yedeklemenize olanak sağlar.

Azure Backup, işlemler için istatistikleri ve Azure portalını başlatma bağlantılarını görüntüleyen bir OMS çözümüne sahiptir.

![Azure Backup üst düzey mimarisi](media/operations-management-suite-architecture/backup.png)

## <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery); sanal makine ve fiziksel sunucu çoğaltma, yük devretme ve yeniden çalışma işlemlerini düzenler. Çoğaltma verileri; Hyper-V konakları, VMware hiper yöneticileri ile birincil ve ikincil veri merkezlerindeki fiziksel sunucular arasında veya veri merkezi ile Azure depolama hizmeti arasında aktarılır.  Site Recovery, belirli bir coğrafi Azure bölgesinde yer alan kasalardaki meta verileri depolar. Çoğaltılmış veriler, Site Recovery hizmeti tarafından depolanmaz.

Azure Site Recovery için üç temel çoğaltma senaryosu söz konusudur.

**Hyper-V sanal makinelerini çoğaltma**

* Hyper-V sanal makineleri VMM bulutlarında yönetiliyorsa, ikincil veri merkezine veya Azure depolama hizmetine yönelik olarak çoğaltma gerçekleştirebilirsiniz.  Azure'a yönelik çoğaltma, güvenli bir İnternet bağlantısı üzerinden yapılır.  İkincil veri merkezine yönelik çoğaltma ise LAN üzerinden yapılır.
* Hyper-V sanal makineleri VMM tarafından yönetilmiyorsa yalnızca Azure depolama hizmetine yönelik olarak çoğaltma gerçekleştirebilirsiniz.  Azure'a yönelik çoğaltma, güvenli bir İnternet bağlantısı üzerinden yapılır.

**VMWare sanal makinelerini çoğaltma**

* VMware sanal makinelerini VMware veya Azure depolama hizmetini çalıştıran ikincil bir veri merkezine yönelik olarak çoğaltabilirsiniz.  Azure'a yönelik çoğaltma, siteler arası bir VPN veya Azure ExpressRoute üzerinden ya da güvenli bir İnternet bağlantısı yoluyla gerçekleşebilir. İkincil bir veri merkezine yönelik çoğaltma, InMage Scout veri kanalı üzerinden gerçekleşir.

**Fiziksel Windows ve Linux sunucularını çoğaltma** 

* Fiziksel sunucuları ikincil bir veri merkezine veya Azure depolama hizmetine yönelik olarak çoğaltabilirsiniz. Azure'a yönelik çoğaltma, siteler arası bir VPN veya Azure ExpressRoute üzerinden ya da güvenli bir İnternet bağlantısı yoluyla gerçekleşebilir. İkincil bir veri merkezine yönelik çoğaltma, InMage Scout veri kanalı üzerinden gerçekleşir.  Azure Site Recovery, bazı istatistikler görüntüleyen bir OMS çözümüne sahiptir ancak işlemler için Azure portalını kullanmanız gerekir.

![Azure Site Recovery üst düzey mimarisi](media/operations-management-suite-architecture/site-recovery.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) hakkında bilgi edinme.
* [Azure Otomasyonu](https://azure.microsoft.com/documentation/services/automation) hakkında bilgi edinme.
* [Azure Backup](http://azure.microsoft.com/documentation/services/backup) hakkında bilgi edinme.
* [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) hakkında bilgi edinme.




<!--HONumber=Nov16_HO2-->


