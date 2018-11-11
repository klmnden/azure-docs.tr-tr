---
title: Azure İzleme Aracısı genel bakış | Microsoft Docs
description: Bu makalede, Azure Vm'leri izleme destekleyen Azure aracıları kullanılabilir ayrıntılı bir bakış sağlar.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: magoedte
ms.openlocfilehash: 81db6720422de111cc5b390c58e9020d7c19f90a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282042"
---
# <a name="overview-of-the-azure-agents-to-monitor-azure-virtual-machines"></a>Azure sanal makinelerini izlemek için Azure aracıları genel bakış
Microsoft Azure, Azure'da veya Microsoft Windows ve Linux çalıştıran diğer bulut sağlayıcılarında barındırılan sanal makinelerden farklı türde veri toplamak için birden çok yol sağlar.  Bu makalede farklar ve sırayla, hangi hizmet yönetim destekleyecek belirlemek her bir aracı ile kullanılabilir veya genel gereksinimleri izleme özellikleri açıklayan yardımcı olur.  

## <a name="comparing-azure-diagnostic-and-log-analytics-agent"></a>Azure tanılama ve Log Analytics aracısını karşılaştırma
Bugün Azure'da aracılar - Azure VM'deki izlemek için kullanılabilen Azure tanılama uzantısı ve Linux ve Windows için Log Analytics aracısını iki tür vardır.  Temelde, bu aracıları ölçümlerini ve günlüklerini toplamak ve iletmek için bir depo için tasarlanmıştır. Bununla birlikte, burada benzerlikleri son olmasıdır.  

[Azure tanılama uzantısını](../monitoring-and-diagnostics/azure-diagnostics.md), 2010'da genel kullanıma sunulan duyurulduğu, Azure bulut Hizmetleri için sağlanmış olan basit bir tanılama veri koleksiyonunu bir VM gibi bir Azure Iaas kaynak sunar ve bir aracı ve Azure depolama için kalıcı.  Depolama alanında birkaç kullanılabilir araç biriyle gibi görüntülemek için seçtiğiniz sonra [Visual Studio sunucu Gezgini'ndeki](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) ve [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).

Toplanacak seçebilirsiniz:

* İşletim sistemi performans sayaçları ve olay günlüklerini veya önceden tanımlanmış birtakım toplamak belirtebilirsiniz. 
* Tüm istekleri ve/veya bir IIS web sunucusuna başarısız istekler
* .NET uygulama izleme çıkış günlükleri
* Windows (ETW) olayları için olay izleme 
* Syslog günlüğü olaylarını Topla  
* Kilitlenme bilgi dökümleri 

Veri alternatif olarak iletilmesi için [Application Insights](../application-insights/app-insights-cloudservices.md), [Log Analytics](../log-analytics/log-analytics-queries.md), veya kullanan Azure dışı hizmetlerine [olay hub'ı](../event-hubs/event-hubs-about.md). 

Gelişmiş birden fazla toplama ölçüm ve günlükleri kümesini gerek duyduğunuz izleme için Windows ve Linux için Log Analytics aracısını gereklidir.  Bu aracı ile otomasyon ve Log Analytics, Azure vm'lerinizi yaşam döngüleri boyunca kapsamlı yönetim sunmak için sundukları özellik kümesini de dahil olmak üzere, gibi Azure hizmetlerini kullanmaya başlayabilirsiniz. Buna aşağıdakiler dahildir:

* [Azure Otomasyonu, güncelleştirme yönetimi](../automation/automation-update-management.md) işletim sistemi güncelleştirmeleri
* [Azure Otomasyonu Desired State Configuration](../automation/automation-dsc-overview.md) tutarlı yapılandırma durumunu korumak üzere
* İzleme yapılandırma değişiklikleriyle [Azure Otomasyon, değişiklik izleme ve stok](../automation/automation-change-tracking.md)
* İşletim sistemi ve barındırılan uygulamalar gibi özel günlükleri koleksiyonunu [FluentD](../log-analytics/log-analytics-data-sources-json.md), [özel günlükleri](../log-analytics/log-analytics-data-sources-custom-logs.md), [MySQL ve Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) Log Analytics ile
* Azure Hizmetleri gibi [Application Insights](https://docs.microsoft.com/azure/application-insights/) ve [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/) yerel olarak kendi verilerini doğrudan Log Analytics'te depolar.  

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Log Analytics ile ortamınızdaki bilgisayarlardan verileri toplama](../log-analytics/log-analytics-concept-hybrid.md) gereksinimleri ve kullanılabilir yöntemlerin aracı, veri merkezinizi veya diğer bulut ortamında dağıtmak için gözden geçirmek için.
- Azure VM’lerden veri toplamayı yapılandırmak için bkz. [Azure Sanal Makineler hakkında veri toplama](../log-analytics/log-analytics-quick-collect-azurevm.md). 
