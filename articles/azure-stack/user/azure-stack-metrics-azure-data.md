---
title: Azure Stack'te Azure İzleyici | Microsoft Docs
description: Azure hakkında bilgi edinin Azure Stack'te İzleyici.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: mabrigg
ms.openlocfilehash: 5663d6fbf6272e46a1d9b75d84ee90a3f46c1d30
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42889879"
---
# <a name="azure-monitor-on-azure-stack"></a>Azure Stack'te Azure İzleyici

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack'te Azure İzleyici'hizmetine genel bakış sağlar. Azure İzleyici işlemi ve Azure Stack'te Azure İzleyicisi'ni kullanma hakkında ek bilgiler ele alınmaktadır. 

Giriş, Azure İzleyici ile çalışmaya başlama hakkında genel Azure makaleye göz atın ve genel bakış [Azure İzleyici ile çalışmaya başlama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started).

![Azure Stack İzleyici dikey penceresi](./media/azure-stack-metrics-azure-data/azs-monitor.png)

Azure İzleyici, Azure kaynaklarını izlemeye yönelik tek bir kaynak sağlayan platform hizmetidir. Azure İzleyici ile görselleştirin, sorgulama yapabilir, yönlendirme, arşivleme ve azure'daki kaynaklardan gelen günlükler ve ölçümler üzerinde işlem Aksi takdirde. Azure Stack Yönetici portalını İzleyici PowerShell cmdlet'leri, platformlar arası CLI veya Azure İzleyici REST API'leri kullanarak bu verilerle çalışabilirsiniz. Azure yığını tarafından desteklenen belirli bağlantı için bkz: [Azure Stack izleme verilerini kullanma](azure-stack-metrics-monitor.md)

> [!Note]  
Ölçümleri ve tanılama günlükleri, Azure Stack Geliştirme Seti için kullanılamaz.

## <a name="prerequisites"></a>Önkoşullar

Kayıt **Microsoft.insights** aboneliğinizin teklif kaynak sağlayıcı ayarları üzerinde kaynak sağlayıcısı. Kaynak sağlayıcısı aboneliğinizle ilişkili teklife kullanılabilir olduğunu doğrulayabilirsiniz:

1. Azure Stack Yönetici portalı'nı açın.
2. Seçin **sunar**.
3. Abonelikle ilişkili teklif seçin.
4. Seçin **kaynak sağlayıcıları** altında **ayarları.** 
5. Bulma **Microsoft.Insights** listesinde ve durum olduğundan emin olun **kayıtlı.**.

## <a name="overview"></a>Genel Bakış

Azure'da Azure İzleyici gibi Azure Stack'te Azure İzleyici, temel düzeyde altyapı ölçümlerini ve günlüklerini çoğu hizmetleri sağlar.

## <a name="azure-monitor-sources-compute-subset"></a>Azure İzleyici kaynakları: alt bilgi işlem

![Azure İzleyici kaynakları - işlem alt](media//azure-stack-metrics-azure-data/azs-monitor-computersubset.png)

**Microsoft.Compute** Azure Stack'te kaynak sağlayıcısı içerir:
 - Virtual Machines 
 - Sanal makine ölçek kümeleri

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Uygulama - tanılama günlükleri, uygulama günlükleri ve ölçümler

Uygulamaları, çalışan bir sanal makinenin işletim sisteminde çalıştırabilirsiniz **Microsoft.Compute** kaynak sağlayıcısı. Bu uygulamalar ve sanal makineleri kendi günlükleri ve ölçümleri kümesini gösterin. Azure İzleyici çoğu uygulama düzeyi ölçümleri ve günlükleri toplamak için Azure tanılama uzantısını (Windows veya Linux) kullanır. 

Ölçüler türleri şunlardır:
 - Performans sayaçları
 - Uygulama günlükleri
 - Windows olay günlükleri
 - .NET olay kaynağı
 - IIS günlükleri
 - Bildirim tabanlı ETW
 - Kilitlenme bilgi dökümleri
 - Müşteri hata günlükleri

> [!Note]  
> Linux tanılama uzantısı Azure Stack üzerinde desteklenmiyor.

### <a name="host-and-guest-vm-metrics"></a>Konak ve Konuk VM ölçümleri

Daha önce listelenen işlem kaynakları, bir VM'nin ayrılmış konak ve konuk işletim sistemi vardır. VM konak ve konuk işletim sistemi kök VM'yi Konuk VM içindeki Hyper-V hiper yönetici ve eşdeğerdir. VM konak ve konuk işletim sistemi için ölçüm toplayabilirsiniz. Ayrıca, konuk işletim sistemi için tanılama günlüklerini toplayabilir. Azure Stack'te konak ve Konuk sanal makine ölçümleri için toplanabilir ölçümlerin bir listesini kullanılabilir [desteklenen ölçümler Azure İzleyici ile Azure Stack üzerinde](azure-stack-metrics-supported.md). 

### <a name="activity-log"></a>Etkinlik günlüğü

Azure Stack altyapısı tarafından görülen işlem kaynaklarınızı hakkında bilgi için etkinlik günlüklerini arama yapabilirsiniz. Günlük, kaynakların oluşturulduğu veya yok edildiği zamanlar gibi bilgiler içerir. Etkinlik günlükleri, Azure Stack Azure ile tutarlı olur. Daha fazla bilgi için açıklamasına bakın [etkinlik günlüğüne genel bakış azure'da](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). 


## <a name="azure-monitor-sources-everything-else"></a>Azure İzleyici kaynakları: diğer her şey

![Azure İzleyici kaynakları - diğer her şey](media//azure-stack-metrics-azure-data/azs-monitor-othersubset.png)

### <a name="resources---metrics-and-diagnostics-logs"></a>Kaynaklar - ölçümleri ve tanılama günlükleri

Toplanabilir ölçümleri ve tanılama günlükleri, kaynak türüne göre değişir. Azure Stack'te her bir kaynak için toplanabilir ölçümlerin bir listesini desteklenen ölçümler kullanılabilir. Daha fazla bilgi için [desteklenen ölçümler Azure İzleyici ile Azure Stack üzerinde](azure-stack-metrics-supported.md).

### <a name="activity-log"></a>Etkinlik günlüğü

Etkinlik günlüğü için aynı işlem kaynakları var. 

### <a name="uses-for-monitoring-data"></a>Veri izleme kullanır

**Store ve arşivleme**  

Bazı izleme verileri Azure İzleyici'de belirli bir süre boyunca depolanır ve kullanılabilir. 
 - Ölçümler 90 gün süreyle depolanır. 
 - Etkinlik günlüğü girişleri 90 gün süreyle depolanır. 
 - Tanılama günlükleri depolanmaz.
 - Verileri daha uzun bekletme süresi için bir depolama hesabına arşivleme.

**Sorgu**  

Sistem ya da Azure depolama verilere erişmek için Azure İzleyici REST API, platformlar arası komut satırı arabirimi (CLI) komutlarını, PowerShell cmdlet'leri veya .NET SDK'sını kullanabilirsiniz. 

**Görselleştirme**

İzleme verilerinizi grafik ve tablo olarak görselleştirmek eğilimleri doğrudan veriye bakmaya göre daha hızlı bulmanıza yardımcı olur. 

Görselleştirme yöntemlerinden bazıları şunlardır:
 - Azure Stack kullanıcı ve Yönetici portalını kullanma
 - Microsoft Power BI için rota verilerini
 - Canlı akış kullanarak veya aracın Azure depolama alanındaki bir arşivden okumasını sağlayarak verileri bir üçüncü taraf görselleştirme aracına yönlendirme

## <a name="methods-of-accessing-azure-monitor-on-azure-stack"></a>Azure Stack'te Azure erişim yöntemleri izleme

Genel olarak, aşağıdaki yöntemlerden birini kullanarak veri izleme, yönlendirme ve alma yöntemlerini yönetebilirsiniz. Tüm eylemler veya veri türleri için tüm yöntemler kullanılabilir olmayabilir.

 - [Azure Stack portalı](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-use-portal)
 - [PowerShell](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples)
 - [Platformlar arası komut satırı arabirimi](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples)
 - [REST API](https://docs.microsoft.com/rest/api/monitor)
 - [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack'te izleme veri tüketimi makaledeki seçenekleri hakkında daha fazla bilgi [Azure Stack verilerini izleme Tüket](azure-stack-metrics-monitor.md).
