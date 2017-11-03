---
title: "Azure tanılama genel bakış | Microsoft Docs"
description: "Hata ayıklama, izleme, bulut Hizmetleri, sanal makineler ve hizmet doku trafiği çözümleme performansını ölçmek için Azure Tanılama'yı kullanın"
services: multiple
documentationcenter: .net
author: rboucher
manager: 
editor: 
ms.assetid: baad40d8-c915-4f93-b486-8b160bf33463
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2017
ms.author: robb
ms.openlocfilehash: 0c6e4d9d2a3744f607b72364f3944c700acd070c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-diagnostics"></a>Azure tanılama nedir
Azure tanılama dağıtılan bir uygulama tanılama verilerini toplama sağlar. Azure içinde bir özelliktir. Bir dizi farklı kaynaktan tanılama uzantısını kullanabilirsiniz. Şu anda Azure bulut hizmeti Web ve çalışan rolleri, Microsoft Windows ve Service Fabric çalışan Azure sanal makineler desteklenir. Diğer Azure hizmetleriyle kendi ayrı tanılama vardır.

## <a name="data-you-can-collect"></a>Verileri toplamak
Azure tanılama aşağıdaki veri türlerini toplayabilirsiniz:

| Veri kaynağı | Açıklama |
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

Azure tanılama uzantısını bir Azure depolama hesabı bu veri aktarma veya gibi hizmetler için Gönder [Application Insights](../application-insights/app-insights-cloudservices.md). Veri, hata ayıklama ve sorun giderme performansını ölçmek, kaynak kullanımı, trafik analizi ve kapasite planlaması izleme ve denetim için kullanabilirsiniz.

## <a name="versioning"></a>Sürüm oluşturma
Bkz: [Azure tanılama sürüm geçmişi](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Sonraki adımlar
Tanılama toplamak ve başlamak için aşağıdaki makalelere kullanmak için çalıştığınız hangi hizmet seçin. Belirli görevleri başvurusu için genel Azure tanılama bağlantıları kullanın.

## <a name="web-apps"></a>Web Apps
Web uygulamaları Azure tanılama kullanmayın unutmayın. Eşdeğer bilgileri bulmak [Web uygulamaları](../app-service/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Azure Tanılama'yı kullanarak bulut Hizmetleri
* Visual Studio kullanıyorsanız, bkz: [bir bulut Hizmetleri uygulaması izleme için Visual Studio](../vs-azure-tools-debug-cloud-services-virtual-machines.md) başlamak için. Aksi takdirde bkz:
* [Bulut Hizmetleri Azure Tanılama'yı kullanarak izleme](../cloud-services/cloud-services-how-to-monitor.md)
* [Bulut Hizmetleri uygulamada Azure tanılama ayarlama](../cloud-services/cloud-services-dotnet-diagnostics.md)

Daha gelişmiş konular için bkz:

* [Bulut Hizmetleri için Application Insights'a Azure tanılama kullanma](../application-insights/app-insights-cloudservices.md)
* [Bulut Hizmetleri uygulaması Azure Tanılama ile akışı izleme](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Bulut hizmetleri üzerinde tanılamayı ayarlamak için PowerShell kullanın](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines-using-azure-diagnostics"></a>Azure tanılama kullanarak sanal makineler
* Visual Studio kullanıyorsanız, bkz: [izleme Azure sanal makineler için Visual Studio'yu kullanın](../vs-azure-tools-debug-cloud-services-virtual-machines.md) başlamak için. Aksi takdirde bkz:
* [Azure tanılama üzerinde bir Azure sanal makine ayarlama](../virtual-machines-dotnet-diagnostics.md)

Daha gelişmiş konular için bkz:

* [Tanılama Azure sanal makineler üzerinde ayarlamak için PowerShell kullanın](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [İzleme ve tanılama Azure Resource Manager şablonu kullanarak bir Windows sanal makine oluşturma](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-using-azure-diagnostics"></a>Service Fabric Azure Tanılama'yı kullanarak
Konumundaki başlamak [Service Fabric uygulaması izleme](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Bu makalede aldıktan sonra diğer birçok Service Fabric tanılama makalelerin sol taraftaki gezinti ağacında kullanılabilir.

## <a name="general-azure-diagnostics-articles"></a>Genel Azure tanılama makaleleri
* [Azure tanılama şema yapılandırma](https://msdn.microsoft.com/library/azure/mt634524.aspx) -toplamak ve Tanılama verileri yönlendirmek için şema dosyası değiştirmeyi öğrenin. Ayrıca Visual Studio şema dosyasını değiştirmek için kullanabileceğiniz olduğunu unutmayın.
* [Azure Tanılama verileri Azure depolama alanında nasıl depolandığını](../cloud-services/cloud-services-dotnet-diagnostics-storage.md) -tanılama veri yazıldığı BLOB'ları ve tabloları adlarını bilme.
* Öğrenme [performans sayaçları Azure Tanılama'kullanma](../cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
* Öğrenme [Application Insights rota Azure tanılama bilgileri](azure-diagnostics-configure-application-insights.md)
* Tanılama başlatılıyor ile sorun varsa veya verileriniz Azure Storage tablolarda bkz [Azure tanılama sorunlarını giderme](azure-diagnostics-troubleshooting.md)
