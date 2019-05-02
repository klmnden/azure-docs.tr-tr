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
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: magoedte
ms.openlocfilehash: 58abe3a3973986ab489456be7958361ad8ab06f4
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64922809"
---
# <a name="overview-of-the-azure-monitoring-agents"></a>Azure İzleme Aracısı genel bakış 
Microsoft Azure, Microsoft Windows ve Azure, veri merkezinizi veya diğer bulut sağlayıcılarında barındırılan Linux çalıştıran sanal makineler farklı veri türleri toplamak için birden çok yol sağlar. Bir VM'yi izlemek için kullanılabilen aracıları üç tür şunlardır:

* Azure tanılama uzantısı
* Linux ve Windows için log Analytics aracısını
* Bağımlılık aracısı

Bu makalede, sırayla, hangi BT Hizmet Yönetimi veya genel izleme gereksinimlerini destekleyecek belirlemek bunları ve bunların özelliklerini arasındaki farklar açıklanmaktadır.  

## <a name="azure-diagnostic-extension"></a>Azure tanılama uzantısı
[Azure tanılama uzantısını](../../azure-monitor/platform/diagnostics-extension-overview.md) (genellikle Windows Azure tanılama (WAD) ya da Linux Azure tanılama (LAD) bir uzantısı olarak adlandırılır), sağlanan Azure bulut Hizmetleri için 2010'da genel kullanıma sunulan duyurulduğu tanılama veri koleksiyonu basit bir VM gibi bir Azure işlem kaynağı sunar ve Azure depolama için kalıcı bir aracıdır. Depolama alanında birkaç kullanılabilir araç biriyle gibi görüntülemek için seçtiğiniz sonra [Visual Studio sunucu Gezgini'ndeki](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) ve [Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md).

Toplanacak seçebilirsiniz:

* İşletim sistemi performans sayaçları ve olay günlüklerini veya önceden tanımlanmış birtakım toplamak belirtebilirsiniz. 
* Tüm istekleri ve/veya bir IIS web sunucusuna başarısız istekler
* .NET uygulama izleme çıkış günlükleri
* Windows için olay izleme (ETW) olayları 
* Syslog günlüğü olaylarını Topla  
* Kilitlenme bilgi dökümleri 

Azure Tanılama aracı, aşağıdakileri yapmak istediğinizde kullanılmalıdır:

* Arşiv günlükleri ve Azure depolama ölçümleri
* İzleme verileri, üçüncü taraf araçlarla tümleştirin. Birçok farklı yöntemle sorgulama için iletilen depolama hesabı dahil olmak üzere bu araçları kullanın [Event Hubs](../../event-hubs/event-hubs-about.md), veya ile sorgulama [Azure izleme REST API'si](../../azure-monitor/platform/rest-api-walkthrough.md)
* Azure portalında ölçüm grafikleri oluşturma veya neredeyse gerçek zamanlı oluşturmak için Azure İzleyici Veril [ölçüm uyarıları](../../azure-monitor/platform/alerts-metric-overview.md). 
* Sanal makine ölçek kümelerini otomatik ölçeklendirme ve klasik bulut Hizmetleri, konuk işletim sistemi ölçümlere göre.
* VM önyükleme sorunlarını denetleyin [önyükleme tanılaması](../../virtual-machines/troubleshooting/boot-diagnostics.md).
* Uygulamalarınızı nasıl performans gösterdiğini anlamak ve bunlarla etkileyen sorunları proaktif olarak tanımlayan [Application Insights](../../azure-monitor/overview.md).
* Azure İzleyici ölçümleri almak ve Cloud Services'dan Klasik VM'ler, toplanan verileri günlüğe yapılandırın ve Service Fabric düğümleri bir Azure depolama hesabında depolanır.

## <a name="log-analytics-agent"></a>Log Analytics Aracısı
Gelişmiş, birden fazla ölçüm ve günlükleri kümesini toplamak gereken izleme için Log Analytics aracısını (Microsoft Monitoring Agent (MMA) olarak da bilinir) Windows ve Linux için gereklidir. Log Analytics aracısını kapsamlı yönetimi için şirket içi fiziksel ve sanal makineler arasında bilgisayarlar System Center Operations Manager tarafından izlenen geliştirilmiştir ve diğer bulutlarda Vm'lerinde barındırılan. Windows ve Linux aracıları hem çözüm tabanlı izleme verilerini, hem de, yapılandırdığınız özel veri kaynaklarını toplamak için Azure İzleyici'de bir Log Analytics çalışma alanına bağlayın.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Log Analytics aracısını istediğinizde kullanılmalıdır:

* Kaynakları hem Azure, diğer bulut sağlayıcıları ve şirket içi kaynaklara içindeki çeşitli veri toplayın. 
* İzleme çözümleri gibi Azure İzleyici birini kullanarak [VM'ler için Azure İzleyici](../insights/vminsights-overview.md), [kapsayıcılar için Azure İzleyici](../insights/container-insights-overview.md)vb.  
* Diğer Azure Yönetim Hizmetleri gibi birini [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md), [Azure Otomasyonu](../../automation/automation-intro.md)vb.

Çeşitli Azure Hizmetleri olarak daha önce toplanmış *Operations Management Suite*, ve bunun sonucunda, Log Analytics aracısını Azure Güvenlik Merkezi ve Azure Otomasyonu gibi hizmetleriyle paylaşılır.  Bu, kapsamlı yönetimini sağlayan Azure Vm'leriniz yaşam döngüleri boyunca teslim sundukları özellik kümesini içerir.  Bu bazı örnekleri şunlardır:

* [Azure Otomasyonu, güncelleştirme yönetimi](../../automation/automation-update-management.md) işletim sistemi güncelleştirmeleri.
* [Azure Otomasyonu Desired State Configuration](../../automation/automation-dsc-overview.md) tutarlı yapılandırma durumunu korumak üzere.
* İzleme yapılandırma değişiklikleriyle [Azure Otomasyon, değişiklik izleme ve stok](../../automation/change-tracking.md).
* Azure Hizmetleri gibi [Application Insights](https://docs.microsoft.com/azure/application-insights/) ve [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/), hangi yerel olarak depolamak verilerini doğrudan Log Analytics'te.  

## <a name="dependency-agent"></a>Bağımlılık aracısı
Bağımlılık Aracısı'nı, başlangıçta Microsoft tarafından geliştirilmiş değil hizmet eşlemesi çözümünün parçası olarak geliştirilmiştir. [Hizmet eşlemesi](../insights/service-map.md) ve [VM'ler için Azure İzleyici](../insights/vminsights-overview.md) gerektiren bir bağımlılık aracısını Windows ve Linux sanal makineleri ve ile tümleştirilir sanal üzerinde çalışan işlemler hakkında bulunan veri toplamak için Log Analytics aracısını makine ve işlem dış bağımlılıkları. Bu verileri bir Log Analytics çalışma alanında depolar ve bulunan birbirine bağlı bileşenleri görselleştirir.

Sanal makinenizin izlemek üzere bu aracı bileşimi gerekebilir. Aracıları yan yana Azure uzantıları ancak yüklenebilir Linux'ta, Log Analytics aracısını *gerekir* yüklenmesi ilk; Aksi takdirde yükleme başarısız olur. 

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Log Analytics aracısını bakış](../../azure-monitor/platform/log-analytics-agent.md) gereksinimleri ve desteklenen yöntemlerden makineleri Azure'da, veri merkezinizi veya başka bir bulut ortamında barındırılan aracı dağıtmak için gözden geçirmek için.

