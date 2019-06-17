---
title: Azure İzleyici ile sürekli izleme | Microsoft Docs
description: Sürekli iş akışlarınızı izlemeyi etkinleştirmek için Azure İzleyicisi'ni kullanarak belirli adımlar açıklanmaktadır.
author: bwren
manager: carmonm
editor: ''
services: azure-monitor
documentationcenter: azure-monitor
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: bwren
ms.openlocfilehash: 1b86bc015b187fe75e79ba04df60a6bc5257a9b8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60497429"
---
# <a name="continuous-monitoring-with-azure-monitor"></a>Sürekli Azure İzleyici ile izleme

Sürekli izleme başvurduğu her aşaması, DevOps izleme eklemek için gerekli teknolojiyi ve işlem ve BT işlemleri yaşam döngüleri. Bu durum, performans ve güvenilirliğini uygulamanızın ve altyapınızın üretim geliştirme hareket ettikçe sürekli olarak sağlamaya yardımcı olur. Yardımcı kavramları sürekli tümleştirme ve sürekli dağıtım (CI/CD), sürekli izleme derlemelerinde geliştirin ve daha hızlı ve güvenilir bir kullanıcılarınıza sürekli değer sağlamak için yazılım sunun.

[Azure İzleyici](overview.md) birleşik izleme çözümü azure'daki uygulamalar arasında tam yığın observability sağlar ve bulutta ve şirket içi altyapı. İle sorunsuz bir şekilde çalışır [Visual Studio ve Visual Studio Code](https://visualstudio.microsoft.com/) geliştirme ve test sırasında ve tümleşik şekilde çalışarak [Azure DevOps](/azure/devops/user-guide/index) sürüm yönetimi ve dağıtım sırasında iş öğesi yönetimi ve işlemler. Sorunları izlemek ve var olan BT işlemlerinizi içindeki olaylar yardımcı olmak için tercih ettiğiniz ITSM ve SIEM araçlar arasında bile tümleştirir.

Bu makalede akışlarınızı sürekli izlemeyi etkinleştirmek için Azure İzleyicisi'ni kullanarak belirli adımlar açıklanmaktadır. Bu, farklı özelliklerini uygulama hakkında ayrıntılar sağlayan diğer belgelere yönelik bağlantılar içerir.


## <a name="enable-monitoring-for-all-your-applications"></a>Tüm uygulamalarınız için izlemeyi etkinleştir
Ortamınız genelinde observability kazanmak için tüm web uygulamaları ve hizmetleri üzerinde izlemeyi etkinleştirmek gerekir. Bu, tüm bileşenler genelinde uçtan uca işlemleri ve bağlantıları kolayca görselleştirmenizi olanak tanır.

- [Azure DevOps projeleri](../devops-project/overview.md) size mevcut kodunuzu ve Git deposu ile basitleştirilmiş bir deneyim sunmak ve azure'da bir sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak üzere örnek uygulamalardan birini seçin.
- [Sürekli izleme, DevOps yayın işlem hattında](../azure-monitor/app/continuous-monitoring.md) dağıtımınızı izleme verileri temel alan ağ geçidi ya da geri alma sağlar.
- [Durum İzleyicisi](../azure-monitor/app/monitor-performance-live-website-now.md) değiştirmeye veya kodunuzu yeniden dağıtmaya gerek kalmadan canlı bir .NET uygulamasını Azure Application Insights ile Windows üzerinde izleyin olanak tanır.
- Uygulamanız için kod erişimi varsa, ardından ile tam izlemeyi etkinleştir [Application Insights](../azure-monitor/app/app-insights-overview.md) için Azure İzleyici Application Insights SDK yükleyerek [.NET](../azure-monitor/learn/quick-monitor-portal.md), [Java ](../azure-monitor/learn/java-quick-start.md), [Node.js](../azure-monitor/learn/nodejs-quick-start.md), veya [herhangi bir programlama dili](../azure-monitor/app/platforms.md). Bu özel olayları, ölçümleri veya uygulamanızın ve işinizin ilgili olan sayfa görünümleri belirtmenize olanak sağlar.



## <a name="enable-monitoring-for-your-entire-infrastructure"></a>Altyapınız için izlemeyi etkinleştir
Uygulamalar yalnızca, temel alınan altyapı olarak güvenilirdir. İzleme altyapınız genelinde etkin olması, bir şeyler başarısız olduğunda bir olası kök nedeni bulmak daha kolay hale getirmek ve tam observability elde yardımcı olur. Azure İzleyici sistem durumunu ve sanal makineleri, kapsayıcıları, depolama ve ağ gibi kaynaklar dahil olmak üzere tüm hibrit altyapınızı performansını izlemenize yardımcı olur.

- Otomatik olarak aldığınız [platform ölçümler, etkinlik günlükleri ve tanılama günlükleri](platform/data-sources.md) hiçbir yapılandırma, Azure kaynaklarınızın en fazla.
- Daha ayrıntılı sahip sanal makineler için izlemeyi etkinleştirin [VM'ler için Azure İzleyici](insights/vminsights-overview.md).
-  AKS ile kümeleri için daha ayrıntılı olarak izlemeyi etkinleştir [kapsayıcılar için Azure İzleyici](insights/container-insights-overview.md).
- Ekleme [izleme çözümleri](insights/solutions-inventory.md) farklı uygulama ve hizmetler, ortamınızdaki.


[Kod olarak altyapı](/azure/devops/learn/what-is-infrastructure-as-code) kullanım için kaynak kodu DevOps ekipleri gibi aynı sürüm oluşturma kullanarak, açıklayıcı bir modelde altyapı yönetimi. Bu, güvenilirlik ve ölçeklenebilirlik ortamınıza ekler ve uygulamalarınızı yönetmek için kullanılan benzer süreçlerden yararlanmanızı sağlar.

-  Kullanım [Resource Manager şablonları](platform/template-workspace-configuration.md) izlemeyi etkinleştir ve uyarılar üzerinde büyük bir kaynak kümesini yapılandırmak için.
- Kullanım [Azure İlkesi](../governance/policy/overview.md) , kaynaklarınız üzerinden farklı kuralları uygulamak için. Bu, bu kaynakları, Kurumsal standartlarınız ve hizmet düzeyi sözleşmeleri ile uyumlu kalmasını sağlar. 


##  <a name="combine-resources-in-azure-resource-groups"></a>Azure kaynak gruplarındaki kaynaklarla birleştirin
Azure'da bugün tipik bir uygulaması, Vm'leri ve uygulama hizmetleri veya Cloud Services, AKS kümeleri veya Service Fabric üzerinde barındırılan mikro hizmetler gibi birden fazla kaynak içerir. Bu uygulamalar, Event Hubs, depolama, SQL ve Service Bus gibi bağımlılıklar sık kullanın.

- Uygulamalarınızı farklı kılan tüm kaynaklarınız genelinde tam görünürlük elde etmek için kaynakları inAzure kaynak grupları birleştirin. [Azure İzleyici kaynak grupları için](../azure-monitor/insights/resource-group-insights.md) durumunu ve performansını tam yığın uygulamasının tamamını ve tüm araştırmalar için karşılık gelen bileşenleri araştırıp bulma veya hata ayıklama sağlayan izlemek için basit bir yol sağlar.

## <a name="ensure-quality-through-continuous-deployment"></a>Sürekli dağıtım elde edebileceğiniz kaliteden emin olun
Sürekli Tümleştirme / sürekli dağıtım sayesinde otomatik olarak tümleştirin ve kod değişiklikleri otomatik test sonuçlarına göre uygulamanızın dağıtabilirsiniz. Bu, dağıtım sürecini kolaylaştırır ve bunlar üretime geçmeden önce herhangi bir değişiklik kalitesini sağlar.


- Kullanım [Azure işlem hatları](/azure/devops/pipelines) sürekli dağıtımı uygulamak ve, kod tamamlama kadar tüm süreci, CI/CD testleri temel üretime otomatikleştirmek için.
- Kullanım [kalite kapıları](/azure/devops/pipelines/release/approvals/gates) , dağıtım öncesi veya dağıtım sonrası izleme tümleştirmek için. Bu, temel sistem durumu/performans ölçümlerini (KPI'ler) uygulamalarınızı, üretim ve farkları altyapı ortamında geliştirme taşıdığınızda veya ölçek olumsuz Kpı'lerinizi etkileyen değil ulaşmanızı sağlar.
- [Ayrı izleme örnekleri korumak](../azure-monitor/app/separate-resources.md) , geliştirme, Test, Kanarya ve üretim gibi farklı dağıtım ortamları arasında. Bu, toplanan veriler ilişkili uygulama ve altyapı arasında uygun olmasını sağlar. Ortamlar arasında verilerin bağıntısını gerekiyorsa, kullanabileceğiniz [ölçüm Gezgini'nde birden çok kaynak grafikleri](../azure-monitor/platform/metrics-charts.md) veya oluşturma [Azure İzleyici'de kaynaklar arası sorgular](log-query/cross-workspace-query.md).


## <a name="create-actionable-alerts-with-actions"></a>Eyleme dönüştürülebilir uyarı eylemleri ile oluşturma
İzleme önemli bir özelliği, geçerli ve tahmin edilen sorunları yöneticileri proaktif olarak bildirme. 

- Oluşturma [Azure İzleyici'de uyarılar](../azure-monitor/platform/alerts-overview.md) günlükleri ve tahmin edilebilir hata durumlarını tanımlamak için ölçümleri temel alan. Tüm uyarılar eyleme dönüştürülebilir gerçek kritik koşulları temsil eder ve hatalı pozitif sonuçları azaltmak arama yapma bir hedefi olmalıdır. Kullanım [dinamik eşikler](platform/alerts-dynamic-thresholds.md) temelleri kendi statik eşikler tanımlamak yerine ölçüm verileri otomatik olarak hesaplamak için. 
- Yöneticilerinize bildiren en etkili yolu kullanmak uyarılar için eylemleri tanımlayın. Kullanılabilir [bildirim eylemlerinde](platform/action-groups.md#create-an-action-group-by-using-the-azure-portal) SMS, e-postalar, anında iletme bildirimleri veya sesli çağrı.
- Eylemler için daha gelişmiş [, ITSM aracına bağlanma](platform/itsmc-overview.md) veya diğer uyarı yönetim sistemleri ile [Web kancaları](platform/activity-log-alerts-webhook.md).
- Uyarılarla de tanımlanan durumlar düzeltme [Azure Otomasyonu runbook'ları](../automation/automation-webhooks.md) veya [Logic Apps](/connectors/custom-connectors/create-webhook-trigger) Web kancalarını kullanan bir uyarıdan başlatılabilir. 
- Kullanım [otomatik ölçeklendirme](../azure-monitor/learn/tutorial-autoscale-performance-schedule.md) dinamik olarak artırmak ve toplanan ölçümlere göre işlem kaynaklarınızı azaltın.

## <a name="prepare-dashboards-and-workbooks"></a>Panolar ve çalışma kitapları hazırlama
Geliştirme ve operasyon araçları ve aynı telemetriyi erişimi olmasını sağlayarak ortamınız genelinde desenlerini görüntülemek ve algılama saati (MTTD) ve geri yükleme ortalama süresi (MTTR) en aza indirmek sağlar.

- Hazırlama [özel panolar](../azure-monitor/learn/tutorial-app-dashboards.md) ortak ölçüm ve günlükleri için kuruluşunuzdaki farklı rollere göre. Panolar, tüm Azure kaynakları verilerinden birleştirebilirsiniz.
- Hazırlama [çalışma kitapları](../azure-monitor/app/usage-workbooks.md) geliştirme ve operasyon arasında paylaşımı bilgi sağlamak için. Bu ölçüm grafikleri ve günlük sorguları veya müşteri desteği veya işlemleri yardımcı geliştiriciler tarafından temel sorunların işlemeye hazır bile olarak sorun giderme kılavuzları ile dinamik raporlar olarak hazırlanmış.

## <a name="continuously-optimize"></a>Sürekli olarak iyileştirin
 İzleme sürekli olarak KPI'ler ve kullanıcı davranış ölçümlerini izleme ve planlama yineleme ile en iyi duruma getirme uygulamak istemediğiniz öneren popüler geliştirme-ölçme-öğrenme felsefemiz'ın temel özelliklerinden biridir. Azure İzleyici ölçüm ve günlükleri işinize ve sonraki dağıtımın gerektiği gibi yeni veri noktaları eklemek için ilgili toplamanıza yardımcı olur.

- Application ınsights araçlarını kullanma [son kullanıcı davranışı ve katılım izlemek](../azure-monitor/learn/tutorial-users.md).
- Kullanım [etki analizi](../azure-monitor/app/usage-impact.md) alanlarına odaklanmak önemli KPI'ları sürücüye açın önceliğini belirlemeye yardımcı olmak için.


## <a name="next-steps"></a>Sonraki adımlar

- Fark bileşenleri hakkında bilgi edinin [Azure İzleyici](overview.md).
- [Sürekli izleme ekleme](../azure-monitor/app/continuous-monitoring.md) yayın işlem hattınızı için.