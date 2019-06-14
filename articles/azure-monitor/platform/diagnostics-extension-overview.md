---
title: Azure tanılama uzantısını genel bakış
description: Hata ayıklama, performansı ölçmek, izleme, bulut Hizmetleri, sanal makineler ve service fabric trafik analizi için Azure Tanılama'kullanma
author: rboucher
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: 8a287f118c126967d2cf8cad77a434cfecc098eb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60236241"
---
# <a name="what-is-azure-diagnostics-extension"></a>Azure tanılama uzantısı nedir
Azure tanılama uzantısı, azure'da dağıtılan bir uygulamada tanılama verilerinin toplanmasını etkinleştiren aracısıdır. Bir dizi farklı kaynaktan tanılama uzantısını kullanabilirsiniz. Azure bulut hizmeti (Klasik) Web ve çalışan rolleri, desteklenmekte olan sanal makineler, sanal makine ölçek kümeleri ve Service Fabric. Diğer Azure Hizmetleri tanılama farklı yöntemleri vardır. Bkz: [Azure'da izlemeye genel bakış](../../azure-monitor/overview.md).

## <a name="linux-agent"></a>Linux Aracısı
A [uzantısının Linux sürümü](../../virtual-machines/extensions/diagnostics-linux.md) Linux çalıştıran sanal makineler için kullanılabilir. Toplanan istatistikleri ve davranışı Windows sürümünden farklılık gösterir.

## <a name="data-you-can-collect"></a>Toplayabileceğiniz veriler
Azure tanılama uzantısı, aşağıdaki veri türlerini toplayabilirsiniz:

| Veri Kaynağı | Açıklama |
| --- | --- |
| Performans sayacı ölçümleri |İşletim sistemi ve özel performans sayaçları |
| Uygulama günlükleri |Uygulamanız tarafından yazılan izleme iletileri |
| Windows Olay günlükleri |Windows olay günlüğü sisteme gönderilen bilgiler |
| .NET EventSource günlükleri |.NET kullanarak olayları yazma kod [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) sınıfı |
| IIS Günlükleri |IIS web siteleri hakkında bilgi |
| [Bildirim tabanlı ETW günlükleri](https://docs.microsoft.com/windows/desktop/etw/about-event-tracing) |Herhangi bir işlem tarafından oluşturulan olay izleme için Windows olayları. (1) |
| Kilitlenme bilgi dökümleri (günlük) |Bir uygulama çökerse işlemin durumu hakkındaki bilgileri |
| Özel hata günlükleri |Uygulamanız veya hizmetiniz tarafından oluşturulan günlükleri |
| Azure Tanılama Altyapısı günlükleri |Azure tanılama kendisi hakkında bilgi |

(1) ETW sağlayıcıları listesini almak için şunu çalıştırın `c:\Windows\System32\logman.exe query providers` bilgileri toplamak istediğiniz makine üzerinde bir konsol penceresinde.

## <a name="data-storage"></a>Veri depolama
Uzantı verilerini depolayan bir [Azure depolama hesabı](diagnostics-extension-to-storage.md) belirttiğiniz.

Buna da gönderebilirsiniz [Application Insights](../../azure-monitor/app/cloudservices.md). 

Kendisine akış için başka bir seçenektir [olay hub'ı](../../event-hubs/event-hubs-about.md), daha sonra Azure dışı izleme hizmetlerine göndermek sağlar.

Ayrıca, verilerinizi Azure İzleyici ölçümleri zaman serisi veritabanına gönderme seçeneğiniz de vardır. Şu anda bu havuz yalnızca performans sayaçları için geçerlidir. Performans sayaçları özel ölçümler olarak göndermenize olanak sağlar. Bu özellik Önizleme aşamasındadır. Azure Monitor havuzu destekler:
* Azure İzleyici aracılığıyla gönderilen tüm performans sayaçlarını alınırken [Azure İzleyici ölçümleri API'leri.](https://docs.microsoft.com/rest/api/monitor/)
* Azure İzleyici aracılığıyla gönderilen tüm performans sayaçlarını uyarı [ölçüm uyarıları](../../azure-monitor/platform/alerts-overview.md) Azure İzleyici'de
* Joker karakter işleci, performans sayaçları "Örnek" boyutu, ölçüm olarak ele alınıyor.  Örneğin, toplanan "LogicalDisk (\*) / DiskWrites/sn" filtre olması ve her Mantıksal Disk (örneğin, C:) sanal makine için bölme çizim veya Disk Yazma/sn uyarısında "Örnek" boyutta sayacı

Bu havuz yapılandırma hakkında daha fazla bilgi edinmek için bkz [Azure tanılama şeması belgeleri.](diagnostics-extension-schema-1dot3.md)

## <a name="costs"></a>Maliyetler
Yukarıdaki seçeneklerin her birinin ücrete neden olabilir. Beklenmeyen faturaları önlemek için araştırma emin olun.  Application Insights, olay hub'ı ve Azure depolama ile alımı ilişkili ayrı maliyetleri ve depolanan zaman. Özellikle, belirli bir zaman dönemi, maliyetleri düşük tutmak amacıyla sonra eski verileri temizlemeye isteyebilirsiniz Azure depolama tüm veri sonsuza kadar tutar.    

## <a name="versioning-and-configuration-schema"></a>Sürüm oluşturma ve yapılandırma şeması
Bkz: [Azure tanılama sürüm geçmişi ve şema](diagnostics-extension-schema.md).


## <a name="next-steps"></a>Sonraki adımlar
Hangi hizmeti üzerinde tanı toplama ve kullanmaya başlamak için aşağıdaki makaleleri kullanın çalıştığınız seçin. Belirli görevleri için başvuru için genel Azure tanılama bağlantıları kullanın.

## <a name="cloud-services-using-azure-diagnostics"></a>Azure Tanılama'yı kullanarak bulut Hizmetleri
* Visual Studio kullanıyorsanız, bkz. [Cloud Services uygulamasına izlemek için Visual Studio](/visualstudio/azure/vs-azure-tools-debug-cloud-services-virtual-machines) kullanmaya başlamak için. Aksi takdirde bkz:
* [Azure Tanılama'yı kullanarak bulut hizmetleri nasıl izlenir?](../../cloud-services/cloud-services-how-to-monitor.md)
* [Azure Tanılama'da bulut Hizmetleri uygulamasını ayarlama](../../cloud-services/cloud-services-dotnet-diagnostics.md)

Daha gelişmiş konular için bkz.

* [Azure Tanılama, Cloud Services için Application Insights ile kullanma](../../azure-monitor/app/cloudservices.md)
* [Azure Tanılama ile bulut Hizmetleri uygulamasının akışı izleme](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Bulut hizmetleri üzerinde tanılamayı ayarlamak için PowerShell kullanma](../../virtual-machines/extensions/diagnostics-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines"></a>Virtual Machines
* Visual Studio kullanıyorsanız, bkz. [izleme Azure sanal makineler için Visual Studio kullanımı](/visualstudio/azure/vs-azure-tools-debug-cloud-services-virtual-machines) kullanmaya başlamak için. Aksi takdirde bkz:
* [Bir Azure sanal makinesi üzerinde Azure tanılama ayarlama](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines)

Daha gelişmiş konular için bkz.

* [Azure sanal Makineler'de tanılamayı ayarlamak için PowerShell kullanma](../../virtual-machines/extensions/diagnostics-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [İzleme ve tanılama Azure Resource Manager şablonu kullanarak bir Windows sanal makinesi oluşturma](../../virtual-machines/extensions/diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric"></a>Service Fabric
Kullanmaya başlayın [bir Service Fabric uygulamasını izleme](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Bu makalede aldıktan sonra birçok diğer Service Fabric tanılama makalelerin sol taraftaki gezinti ağacında kullanılabilir.

## <a name="general-articles"></a>Genel makaleleri
* Öğrenme [Azure Tanılama'da performans sayaçları kullanma](../../cloud-services/diagnostics-performance-counters.md).
* Verilerinizi Azure depolama tabloları ' bkz veya tanılama başlatılıyor ile sorun varsa [Azure tanılama sorunlarını giderme](diagnostics-extension-troubleshooting.md)

