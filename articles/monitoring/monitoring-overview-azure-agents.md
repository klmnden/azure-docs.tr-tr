---
title: Azure İzleme Aracısı genel bakış | Microsoft Docs
description: Bu makalede, Azure'da veya karma ortamda barındırılan sanal makineleri izleme destekleyen Azure aracıları ayrıntılı bir genel bakış sağlar.
services: azure-monitor
documentationcenter: azure-monitor
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: ce6f40e580595e4f3fbf0519aa1ba8be0355ded1
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621914"
---
# <a name="overview-of-the-azure-monitoring-agents"></a>Azure İzleme Aracısı genel bakış 
Microsoft Azure, Azure'da veya Microsoft Windows ve Linux çalıştıran diğer bulut sağlayıcılarında barındırılan sanal makinelerden farklı türde veri toplamak için birden çok yol sağlar. Bu makalede farklar ve hangisinin, BT Hizmet Yönetimi destekleyecek belirlemek, sırayla her bir aracı ile kullanılabilir veya genel gereksinimleri izleme özellikleri açıklayan yardımcı olur.  

## <a name="comparing-agents"></a>Aracıları karşılaştırma
Bugün Azure'da aracılar - Azure VM'deki izlemek için kullanılabilen Azure tanılama uzantısı, bağımlılık aracısını ve Linux ve Windows için Log Analytics aracısını üç tür vardır.  Temelde, Azure bağımlılık uzantısı ve Log Analytics aracılarını ölçümlerini ve günlüklerini toplamak ve iletmek için bir depo için tasarlanmıştır. Bununla birlikte, burada benzerlikleri son olmasıdır.  

### <a name="azure-diagnostic-extension"></a>Azure tanılama uzantısı
[Azure tanılama uzantısını](../monitoring-and-diagnostics/azure-diagnostics.md) (genellikle Windows Azure tanılama (WAD) ya da Linux Azure tanılama (LAD) bir uzantısı olarak adlandırılır), sağlanan Azure bulut Hizmetleri için 2010'da genel kullanıma sunulan duyurulduğu tanılama veri koleksiyonu basit bir VM gibi bir Azure işlem kaynağı sunar ve Azure depolama için kalıcı bir aracıdır. Depolama alanında birkaç kullanılabilir araç biriyle gibi görüntülemek için seçtiğiniz sonra [Visual Studio sunucu Gezgini'ndeki](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) ve [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).

Toplanacak seçebilirsiniz:

* İşletim sistemi performans sayaçları ve olay günlüklerini veya önceden tanımlanmış birtakım toplamak belirtebilirsiniz. 
* Tüm istekleri ve/veya bir IIS web sunucusuna başarısız istekler
* .NET uygulama izleme çıkış günlükleri
* Windows (ETW) olayları için olay izleme 
* Syslog günlüğü olaylarını Topla  
* Kilitlenme bilgi dökümleri 

Azure tanılama aracısını olmalıdır kullanılır:

* Günlükleri ve Azure depolama ölçümleri arşivlemek istediğiniz
* İzleme verileri, üçüncü taraf araçlarla tümleştirin. Birçok farklı yöntemle sorgulama için iletilen depolama hesabı dahil olmak üzere bu araçları kullanın [Event Hubs](../event-hubs/event-hubs-about.md), veya ile sorgulama [Azure izleme REST API'si](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md)
* Azure portalında ölçüm grafikleri oluşturma veya neredeyse gerçek zamanlı oluşturmak için Azure İzleyici Veril [ölçüm uyarıları](../monitoring-and-diagnostics/alert-metric-overview.md). 
* Sanal makine ölçek kümelerini otomatik ölçeklendirme ve klasik bulut Hizmetleri, konuk işletim sistemi ölçümlere göre.
* VM önyükleme sorunlarını denetleyin [önyükleme tanılaması](../virtual-machines/troubleshooting/boot-diagnostics.md).
* Uygulamalarınızı nasıl performans gösterdiğini anlamak ve bunlarla etkileyen sorunları proaktif olarak tanımlayan [Application Insights](../azure-monitor/overview.md).
* Ölçümleri alma ve Cloud Services'dan Klasik VM'ler, toplanan verileri günlüğe kaydetmek için log Analytics'i yapılandırmak ve Service Fabric düğümleri bir Azure depolama hesabında depolanır.

### <a name="log-analytics-agent"></a>Log Analytics Aracısı
Gelişmiş birden fazla toplama ölçüm ve günlükleri kümesini gerek duyduğunuz izleme için Windows ve Linux için Log Analytics aracısını gereklidir. Log Analytics aracısını kapsamlı yönetimi için şirket içi fiziksel ve sanal makineler arasında bilgisayarlar System Center Operations Manager tarafından izlenen geliştirilmiştir ve diğer bulutlarda Vm'lerinde barındırılan. Windows ve Linux aracıları hem çözüm tabanlı izleme verilerini, hem de, yapılandırdığınız özel veri kaynaklarını toplamak için bir Log Analytics çalışma alanına bağlayın.

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

Log Analytics aracısını olmalıdır kullanılır:

* Şirket içinde veya diğer bulut ortamında çalışan makineler sahip
* İzleme çözümleri gibi Azure İzleyici birini kullanarak [VM'ler için Azure İzleyici](../monitoring/monitoring-vminsights-overview.md?toc=%2fazure%2fmonitoring%2ftoc.json), [kapsayıcılar için Azure İzleyici](../monitoring/monitoring-container-insights-overview.md?toc=%2fazure%2fmonitoring%2ftoc.json)vb.  
* Diğer Azure yönetim hizmetlerden biri gibi kullanarak [Azure Güvenlik Merkezi](../security-center/security-center-intro.md), [Azure Otomasyonu](../automation/automation-intro.md)vb.
* Günlük karşıya yükleme ve/veya Azure İzleyici ölçüm verileri.

Çeşitli Azure Hizmetleri olarak daha önce toplanmış *Operations Management Suite*, ve bunun sonucunda, Log Analytics aracısını Azure Güvenlik Merkezi ve Azure Otomasyonu gibi hizmetleriyle paylaşılır.  Bu, kapsamlı yönetimini sağlayan Azure Vm'leriniz yaşam döngüleri boyunca teslim sundukları özellik kümesini içerir. Buna aşağıdakiler dahildir:

* [Azure Otomasyonu, güncelleştirme yönetimi](../automation/automation-update-management.md) işletim sistemi güncelleştirmeleri.
* [Azure Otomasyonu Desired State Configuration](../automation/automation-dsc-overview.md) tutarlı yapılandırma durumunu korumak üzere.
* İzleme yapılandırma değişiklikleriyle [Azure Otomasyon, değişiklik izleme ve stok](../automation/automation-change-tracking.md).
* İşletim sistemi ve barındırılan uygulamalar gibi özel günlükleri koleksiyonunu [FluentD](../log-analytics/log-analytics-data-sources-json.md), [özel günlükleri](../log-analytics/log-analytics-data-sources-custom-logs.md), ve [MySQL ve Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) Log Analytics ile.
* Azure Hizmetleri gibi [Application Insights](https://docs.microsoft.com/azure/application-insights/) ve [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/) yerel olarak kendi verilerini doğrudan Log Analytics'te depolar.  

### <a name="dependency-agent"></a>Bağımlılık aracısı
Bağımlılık Aracısı'nı, ilk olarak Microsoft'tan dışarıdan geliştirilmiştir hizmet eşlemesi çözümünün parçası olarak geliştirilmiştir. [Hizmet eşlemesi](../monitoring/monitoring-service-map.md) ve [VM'ler için Azure İzleyici](monitoring-vminsights-overview.md) gerektiren bir bağımlılık aracısını Windows ve Linux sanal makineleri ve ile tümleştirilir sanal üzerinde çalışan işlemler hakkında bulunan toplanan veriler Log Analytics aracısını makine ve işlem dış bağımlılıkları. Bu veriler, Log Analytics'te depolar ve bulunan birbirine bağlı bileşenleri görselleştirir.

Sanal makinenizin izlemek üzere bu aracı bileşimi gerekebilir. Aracıları yan yana Azure uzantıları ancak yüklenebilir Linux'ta, Log Analytics aracısını *gerekir* yüklenmesi ilk; Aksi takdirde yükleme başarısız olur. 

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Log Analytics aracısını bakış](../log-analytics/log-analytics-agent-overview.md) gereksinimleri ve desteklenen yöntem Azure, veri merkezinizi veya başka bir bulut ortamında barındırılan makineleri aracısını dağıtmak için gözden geçirmek için.

