---
title: Azure tanılama uzantısını genel bakış | Microsoft Docs
description: Hata ayıklama, izleme, bulut Hizmetleri, sanal makineler ve hizmet doku trafiği çözümleme performansını ölçmek için Azure Tanılama'yı kullanın
services: multiple
documentationcenter: .net
author: rboucher
manager: ''
editor: ''
ms.assetid: baad40d8-c915-4f93-b486-8b160bf33463
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/01/2018
ms.author: robb
ms.openlocfilehash: daeaddefa461e71fcc62af4efc4fb7084b237cf9
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="what-is-azure-diagnostics-extension"></a>Azure tanılama uzantısını nedir
Azure tanılama uzantısını dağıtılan bir uygulama tanılama verilerini toplama sağlar. Azure içinde bir aracıdır. Bir dizi farklı kaynaktan tanılama uzantısını kullanabilirsiniz. Azure bulut hizmeti (Klasik) Web ve çalışan rolleri, sanal makineler, desteklenmekte olan sanal makine ölçek kümeleri ve Service Fabric. Diğer Azure hizmetleriyle farklı tanılama yöntemi vardır. Bkz: [Azure'da izleme genel bakış](monitoring-overview.md). 

## <a name="linux-agent"></a>Linux Aracısı
A [uzantısı'nın Linux sürümü](../virtual-machines/linux/diagnostic-extension.md) Linux çalıştıran sanal makineler için kullanılabilir. Toplanan istatistikleri ve davranış Windows sürümünden farklılık gösterir. 

## <a name="data-you-can-collect"></a>Verileri toplamak
Azure tanılama uzantısını aşağıdaki veri türlerini toplayabilirsiniz:

| Veri Kaynağı | Açıklama |
| --- | --- |
| Performans sayaçları |İşletim sistemi ve özel performans sayaçları |
| Uygulama günlükleri |Uygulamanız tarafından yazılan iletilerin izleme |
| Windows olay günlükleri |Windows olay günlüğü sisteme gönderilen bilgiler |
| .NET olay kaynağı |.NET kullanarak olayları yazma kodu [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) sınıfı |
| IIS günlükleri |IIS web siteleri hakkında bilgi |
| Temel ETW bildirimi |Herhangi bir işlem tarafından oluşturulan olay Windows için izleme olayları |
| Kilitlenme bilgi dökümleri |Bir uygulama çökmesi durumunda işleminin durumu hakkında bilgi |
| Özel hata günlükleri |Uygulama veya hizmet tarafından oluşturulan günlükleri |
| Azure tanılama altyapı günlükleri |Tanılama kendisi hakkında bilgi |

## <a name="data-storage"></a>Veri depolama
Uzantı, verilerini depolayan bir [Azure depolama hesabı](azure-diagnostics-storage.md) belirttiğiniz. 

İçin de gönderebilirsiniz [Application Insights](../application-insights/app-insights-cloudservices.md). Kendisine akışını sağlamak için başka bir seçenektir [olay hub'ı](../event-hubs/event-hubs-what-is-event-hubs.md), o Azure olmayan izleme hizmetleri göndermenize izin verir. 


## <a name="versioning-and-configuration-schema"></a>Sürüm oluşturma ve yapılandırma şeması
Bkz: [Azure tanılama sürüm geçmişi ve şema](azure-diagnostics-versioning-history.md).


## <a name="next-steps"></a>Sonraki adımlar
Tanılama toplamak ve başlamak için aşağıdaki makalelere kullanmak için çalıştığınız hangi hizmet seçin. Belirli görevleri başvurusu için genel Azure tanılama bağlantıları kullanın.

## <a name="cloud-services-using-azure-diagnostics"></a>Azure Tanılama'yı kullanarak bulut Hizmetleri
* Visual Studio kullanıyorsanız, bkz: [bir bulut Hizmetleri uygulaması izleme için Visual Studio](../vs-azure-tools-debug-cloud-services-virtual-machines.md) başlamak için. Aksi takdirde bkz:
* [Bulut Hizmetleri Azure Tanılama'yı kullanarak izleme](../cloud-services/cloud-services-how-to-monitor.md)
* [Bulut Hizmetleri uygulamada Azure tanılama ayarlama](../cloud-services/cloud-services-dotnet-diagnostics.md)

Daha gelişmiş konular için bkz:

* [Bulut Hizmetleri için Application Insights'a Azure tanılama kullanma](../application-insights/app-insights-cloudservices.md)
* [Bulut Hizmetleri uygulaması Azure Tanılama ile akışı izleme](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Bulut hizmetleri üzerinde tanılamayı ayarlamak için PowerShell kullanın](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines"></a>Virtual Machines
* Visual Studio kullanıyorsanız, bkz: [izleme Azure sanal makineler için Visual Studio'yu kullanın](../vs-azure-tools-debug-cloud-services-virtual-machines.md) başlamak için. Aksi takdirde bkz:
* [Azure tanılama üzerinde bir Azure sanal makine ayarlama](../virtual-machines-dotnet-diagnostics.md)

Daha gelişmiş konular için bkz:

* [Tanılama Azure sanal makineler üzerinde ayarlamak için PowerShell kullanın](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [İzleme ve tanılama Azure Resource Manager şablonu kullanarak bir Windows sanal makine oluşturma](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric"></a>Service Fabric
Konumundaki başlamak [Service Fabric uygulaması izleme](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Bu makalede aldıktan sonra diğer birçok Service Fabric tanılama makalelerin sol taraftaki gezinti ağacında kullanılabilir.

## <a name="general-articles"></a>Genel makaleleri
* Öğrenme [performans sayaçları Azure Tanılama'kullanma](../cloud-services/diagnostics-performance-counters.md).
* Tanılama başlatılıyor ile sorun varsa veya bkz: Azure depolama tablolarda, verilerinizi bulma [Azure tanılama sorunlarını giderme](azure-diagnostics-troubleshooting.md)
