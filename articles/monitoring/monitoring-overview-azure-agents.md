---
title: İzleme aracıları Azure genel bakış | Microsoft Docs
description: Bu makale Azure VM'ler izleme destekleyen Azure aracılarını kullanılabilir ayrıntılı bir genel bakış sağlar.
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
ms.openlocfilehash: a399c3968e5ee1e2d1f6d623a68dbb1e15cef212
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37088674"
---
# <a name="overview-of-the-azure-agents-to-monitor-azure-virtual-machines"></a>Azure sanal makinelerini izlemek için Azure aracıları genel bakış
Microsoft Azure, Azure veya Microsoft Windows ve Linux çalıştıran diğer bulut sağlayıcıları barındırılan sanal makineleri farklı veri türlerini toplamak için birden çok yol sağlar.  Bu makalede, sırayla, hangisinin service management destekleyecektir belirlemek her bir aracının bulunan veya genel gereksinimlerini izleme yetenekleri ve farkları açıklamaktadır yardımcı olur.  

## <a name="comparing-azure-diagnostic-and-log-analytics-agent"></a>Azure tanılama ve günlük analizi aracı karşılaştırma
Bugün Azure'da, aracıları bir Azure VM - izlemek kullanılabilir günlük analizi Aracısı Linux ve Windows için ve Azure tanılama uzantısını iki tür vardır.  Temelde, bu aracıları ölçümleri ve günlükleri toplamak ve bir depoya iletmek için tasarlanmıştır. Ancak, çıktıı olmasıdır.  

[Azure tanılama uzantısını](../monitoring-and-diagnostics/azure-diagnostics.md), 2010'da, genel olarak kullanılabilir hale geldi bu yana hangi Azure bulut Hizmetleri için sağlanmış olan bir VM gibi bir Azure Iaas kaynaktan basit tanılama veri koleksiyonunu sunar bir aracı ve Azure depolama alanına kalır.  Birkaç kullanılabilen araçlar biriyle gibi görüntülemek seçtiğiniz depolama biriminde, sonra [Visual Studio Server Explorer'da](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md) ve [Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).

Toplanacak seçebilirsiniz:

* İşletim sistemi performans sayaçları ve olay günlüklerini veya önceden tanımlanmış bir kümesini toplamak belirtebilirsiniz. 
* Tüm istekleri ve/veya bir IIS web sunucusu için başarısız istekleri
* .NET uygulama izleme Çıktı günlükleri
* Windows (ETW) olayları için olay izleme 
* Syslog günlüğü olaylarını Topla  
* Kilitlenme bilgi dökümleri 

Veri alternatif olarak iletilmesi için [Application Insights](../application-insights/app-insights-cloudservices.md), [günlük analizi](../log-analytics/log-analytics-overview.md), ya da kullanarak Azure olmayan hizmetlerine [olay hub'ı](../event-hubs/event-hubs-what-is-event-hubs.md). 

Gelişmiş birden fazla topluyorsunuz ölçümleri ve bir alt kümesini günlükleri ihtiyaç duyacağınız izleme için Windows ve Linux için günlük analizi Aracısı gerekli değildir.  Bu aracı ile otomasyon ve bunlar, Azure Vm'leriniz yaşam döngüsü boyunca kapsamlı yönetimini sağlamak üzere sunar özellikler kümesini de dahil olmak üzere günlük analizi gibi Azure hizmetleri kullanan imkanınız olur. Buna aşağıdakiler dahildir:

* [Azure Otomasyonu güncelleştirme yönetimi](../automation/automation-update-management.md) işletim sistemi güncelleştirmeleri
* [Azure Otomasyonu istenen durum Yapılandırması](../automation/automation-dsc-overview.md) tutarlı yapılandırma durumunu korumak için
* Yapılandırma değişiklikleri ile İzle [Azure Otomasyon değişiklik izleme ve stok](../automation/automation-change-tracking.md)
* İşletim sistemi ve barındırılan uygulamalar gibi özel günlüklerinden koleksiyonunu [FluentD](../log-analytics/log-analytics-data-sources-json.md), [özel günlükler](../log-analytics/log-analytics-data-sources-custom-logs.md), [MySQL ve Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) günlük analizi ile
* Azure Hizmetleri gibi [Application Insights](https://docs.microsoft.com/azure/application-insights/) ve [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/) verilerini doğrudan günlük analizi yerel olarak depolar.  

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [günlük analizi ile ortamınızdaki bilgisayarlardan verileri toplama](../log-analytics/log-analytics-concept-hybrid.md) gereksinimleri ve kullanılabilir yöntemlerin merkeziniz veya başka bir bulut ortamında bilgisayarlara aracı dağıtmak için gözden geçirmek için.
- Azure VM’lerden veri toplamayı yapılandırmak için bkz. [Azure Sanal Makineler hakkında veri toplama](../log-analytics/log-analytics-quick-collect-azurevm.md). 