---
title: Log Analytics nedir? | Microsoft Belgeleri
description: "Log Analytics, bulut ve şirket içi ortamınızdaki kaynaklar tarafından oluşturulan işletimsel verileri toplayıp analiz etmenize yardımcı olan bir Operations Management Suite (OMS) hizmetidir.  Log Analytics’in farklı bileşenleri hakkında kısa bir genel fikir veren bu makalede ayrıntılı içerik bağlantıları da vardır."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: bd90b460-bacf-4345-ae31-26e155beac0e
ms.service: log-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/06/2016
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 09a2b3ccd2048ab2638dd91d1905483f10d6d339


---
# <a name="what-is-log-analytics"></a>Log Analytics nedir?
Log Analytics, bulut ve şirket içi ortamlarınızdaki kaynaklar tarafından oluşturulan verileri toplayıp analiz etmenize yardımcı olan bir [Operations Management Suite \(OMS\)](../operations-management-suite/operations-management-suite-overview.md) hizmetidir. Fiziksel konumlarından bağımsız olarak, tüm sunucularınızda ve iş yüklerinizde yer alan milyonlarca kaydı, tümleşik arama ve özel panolar aracılığıyla analiz ederek, gerçek zamanlı öngörüler sağlar.

## <a name="log-analytics-components"></a>Log Analytics bileşenleri
Log Analytics’in merkezinde, Azure bulutunda barındırılan OMS deposu bulunur.  Veriler, veri kaynakları yapılandırılarak ve aboneliğinize çözümler eklenerek bağlı kaynaklardan depoya toplanır.  Veri kaynakları ve çözümler, kendi özellikleri olan, ancak depoya yapılan sorgularda yine de birlikte analiz edilebilen farklı kayıt türleri oluşturacaktır.  Böylece farklı kaynaklar tarafından toplanan farklı veri türleriyle çalışmak için aynı araçları ve yöntemleri kullanabilirsiniz.

![OMS deposu](media/log-analytics-overview/overview.png)

Bağlı kaynaklar, Log Analytics tarafından toplanan verileri oluşturan bilgisayarlar ve diğer kaynaklardır.  Bu, doğrudan bağlanan [Windows](log-analytics-windows-agents.md) ve [Linux](log-analytics-linux-agents.md) bilgisayarlarına yüklenmiş aracıları veya [bağlı System Center Operations Manager yönetim grubundaki](log-analytics-om-agents.md) aracıları içerebilir.  Log Analytics [Azure depolama alanından](log-analytics-azure-storage.md) da veri toplayabilir.

[Veri kaynakları](log-analytics-data-sources.md), bağlı her kaynaktan toplanan farklı veri türleridir.  Bunlar, [Windows](log-analytics-data-sources-windows-events.md) ve Linux aracılarından alınan olaylarla [performans verilerini](log-analytics-data-sources-performance-counters.md) ve [IIS günlükleri](log-analytics-data-sources-iis-logs.md) ile [özel metin günlükleri](log-analytics-data-sources-custom-logs.md) gibi kaynakları içerir.  Toplamak istediğiniz her veri kaynağını yapılandırabilirsiniz. Yapılandırma, otomatik olarak bağlı her kaynağa dağıtılır.

## <a name="analyzing-log-analytics-data"></a>Log Analytics verilerini çözümleme
Log Analytics ile çoğunlukla OMS portalı aracılığıyla etkileşim kurarsınız. Bu portal, her tarayıcıda çalışır ve toplanan verileri çözümlemenizi ve kullanmanızı sağlayacak yapılandırma ayarlarına ve birçok araca erişim sağlar.  Portalda, toplanan verileri analiz etmek için sorgular oluşturabileceğiniz [günlük aramalarını](log-analytics-log-searches.md), en değerli aramalarınızın grafik görünümleriyle özelleştirebileceğiniz [panoları](log-analytics-dashboards.md) ve ek işlevlerle analiz araçları sağlayan [çözümleri](log-analytics-add-solutions.md) kullanabilirsiniz.

![OMS portalı](media/log-analytics-overview/portal.png)

Log Analytics, depodaki verileri hızlı bir şekilde almak ve birleştirmek için bir sorgu söz dizimi sağlar.  Verileri doğrudan OMS portalında çözümlemek veya sorgunun sonuçları önemli bir koşulu gösterdiğinde günlük aramalarının otomatik olarak çalışmasını sağlamak için [Günlük Aramaları](log-analytics-log-searches.md) oluşturabilir ve kaydedebilirsiniz.

![Günlük araması](media/log-analytics-overview/log-search.png)

Genel ortamınızın durumunu kısa bir grafikte göstermek için [panonuza](log-analytics-dashboards.md) kayıtlı günlük aramalarıyla ilgili görselleştirmeler ekleyebilirsiniz.   

![Pano](media/log-analytics-overview/dashboard.png)

Log Analytics dışındaki verileri analiz etmek için OMS deposundaki verileri [Power BI](log-analytics-powerbi.md) veya Excel gibi araçlara aktarabilirsiniz.  Log Analytics verilerini kullanan özel çözümler oluşturmak veya başka sistemlerle tümleştirmek için [Günlük Arama API’sini](log-analytics-log-search-api.md) de kullanabilirsiniz.

## <a name="solutions"></a>Çözümler
Çözümler, Log Analytics’e işlevler ekler.  Genellikle bulutta çalışır ve OMS deposunda toplanan verilere ilişkin analiz sağlarlar. Günlük Aramaları veya OMS panosundaki çözüm tarafından sağlanan ek kullanıcı arabirimi kullanılarak çözümlenebilecek yeni kayıt türlerinin toplanmasını da belirtebilirler.  

![Değişiklik İzleme çözümü](media/log-analytics-overview/change-tracking.png)

Çözümler çeşitli işlevler için kullanılabilir. Çözüm Galeri’sinden mevcut çözümlere kolayca göz atabilir ve [OMS çalışma alanınıza ekleyebilirsiniz](log-analytics-add-solutions.md).  Çoğu çözüm, otomatik olarak dağıtılıp hemen çalışmaya başlayacaktır. Diğerleri ise yapılandırma gerektirebilir.

![Çözüm Galerisi](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Log Analytics mimarisi
Log Analytics’in, merkezi bileşenleri Azure bulutunda barındırıldığı için çok az dağıtım gereksinimi vardır.  Bu bileşenler, deponun yanı sıra toplanan verileri ilişkilendirmenizi ve çözümlemenizi sağlayan hizmetleri içerir.  Portala herhangi bir tarayıcıdan erişilebilir, bu nedenle herhangi bir istemci yazılımı gereksinimi yoktur.

[Windows](log-analytics-windows-agents.md) ve [Linux](log-analytics-linux-agents.md) bilgisayarlarındaki aracıları yüklemeniz gerekir, ancak zaten [bağlı SCOM yönetim grubunun](log-analytics-om-agents.md) üyesi olan bilgisayarlar için ek aracıya gerek yoktur.  SCOM aracıları, verilerini Log Analytics'e yönlendiren yönetim sunucuları ile iletişim kurmaya devam eder.  Ancak bazı çözümler, aracıların doğrudan Log Analytics ile iletişim kurmasını gerektirecektir.  Her çözüme yönelik belgeler, iletişim gereksinimlerini belirtecektir.

[Log Analytics için kaydolduğunuzda](log-analytics-get-started.md) bir OMS çalışma alanı oluşturursunuz.  Çalışma alanını kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir OMS ortamı olarak düşünebilirsiniz. Aboneliğinizde üretim ve test gibi birden fazla ortamı desteklemek için birden fazla çalışma alanı oluşturabilirsiniz.

![Log Analytics mimarisi](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kendi ortamınızda test etmek üzere [ücretsiz bir Log Analytics hesabı için kaydolun](log-analytics-get-started.md).
* OMS deposuna veri toplayabilen farklı [Veri Kaynaklarını](log-analytics-data-sources.md) görüntüleyin.
* Log Analytics’e işlev eklemek için [Çözüm Galerisi’ndeki mevcut çözümlere göz atın](log-analytics-add-solutions.md).




<!--HONumber=Nov16_HO2-->


