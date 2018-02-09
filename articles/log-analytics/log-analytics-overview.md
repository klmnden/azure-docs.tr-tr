---
title: "Azure’da Log Analytics nedir? | Microsoft Docs"
description: "Log Analytics, bulut ve şirket içi ortamınızdaki kaynaklar tarafından oluşturulan işletimsel verileri toplayıp analiz etmenize yardımcı olan bir Azure hizmetidir.  Log Analytics’in farklı bileşenleri hakkında kısa bir genel fikir veren bu makalede ayrıntılı içerik bağlantıları da vardır."
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
ms.date: 01/24/2018
ms.author: bwren
ms.openlocfilehash: a95528f5bd259a36ea96c7bc0660ca082c09d6e6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="what-is-log-analytics"></a>Log Analytics nedir?
Log Analytics, Azure’daki bulut ve şirket içi ortamlarını, kullanılabilirlik ve performansı sürdürmek amacıyla izleyen bir hizmettir.  Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar.  Bu makale, Log Analytics’in sağladığı değere ilişkin kısa bir açıklama, nasıl çalıştığına genel bakış ve daya ayrıntılı içeriklerin bağlantılarını içerir.

## <a name="is-log-analytics-for-you"></a>Log Analytics sizin için uygun mu?
Azure ortamınız için kullandığınız geçerli bir izleme aracı yoksa, Azure kaynaklarınıza ilişkin izleme verilerini toplayıp çözümleyen [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md) ile çalışmaya başlayabilirsiniz.  Log Analytics, diğer verilerle ilişkilendirmek ve ek analiz sağlamak üzere [Azure İzleyici'deki verileri toplayabilir](log-analytics-azure-storage.md).

Şirket içi ortamınızı izlemek istiyorsanız veya Azure İzleyici ya da System Center Operations Manager gibi hizmetleri kullanarak zaten izleme yapıyorsanız, Log Analytics önemli ölçüde katkıda bulunabilir.  Verileri doğrudan aracılarınızdan ve bu diğer araçlardan tek bir depoya toplayabilir.  Log Analytics’in günlük aramaları, görünümler ve çözümler gibi analiz araçları, tüm ortamınıza ilişkin merkezi bir analiz sağlamak üzere toplanan tüm verilerle çalışır.


## <a name="using-log-analytics"></a>Log Analytics kullanma
Log Analytics’e, herhangi bir tarayıcıda çalışan ve yapılandırma ayarlarının yanı sıra toplanan verileri analiz edip üzerinde işlem yapmaya yönelik çeşitli araçlara erişmenizi sağlayan Azure portalı üzerinden erişebilirsiniz.  Portalda, toplanan verileri analiz etmek için sorgular oluşturabileceğiniz [günlük aramalarını](log-analytics-log-searches.md), en değerli aramalarınızın grafik görünümleriyle özelleştirebileceğiniz [panoları](log-analytics-dashboards.md) ve ek işlevlerle analiz araçları sağlayan [çözümleri](log-analytics-add-solutions.md) kullanabilirsiniz.

Aşağıdaki görüntü, çalışma alanına yüklenmiş [çözümlere](#add-functionality-with-management-solutions) ilişkin özet bilgileri gösteren Genel bakış ekranından alınmıştır.  İlgili çözümün daha ayrıntılı verilerine ulaşmak için herhangi bir kutucuğa tıklayabilirsiniz.

![OMS portalı](media/log-analytics-overview/portal.png)

Log Analytics, depodaki verileri hızlı bir şekilde almak ve birleştirmek için bir sorgu dili içerir.  Verileri doğrudan portalda çözümlemek veya sorgunun sonuçları önemli bir koşulu gösterdiğinde günlük aramalarının otomatik olarak çalışmasını sağlamak için [Günlük Aramaları](log-analytics-log-searches.md) oluşturabilir ve kaydedebilirsiniz.

![Günlük araması](media/log-analytics-overview/log-search.png)

Log Analytics dışındaki verileri analiz etmek için [Power BI](log-analytics-powerbi.md) veya Excel gibi araçlara aktarabilirsiniz.  Log Analytics verilerini kullanan özel çözümler oluşturmak veya başka sistemlerle tümleştirmek için [Günlük Arama API’sini](log-analytics-log-search-api.md) de kullanabilirsiniz.

## <a name="add-functionality-with-management-solutions"></a>Yönetim çözümleriyle işlevsellik ekleme
[Yönetim çözümleri](log-analytics-add-solutions.md), Log Analytics’e ek veri ve analiz araçları sağlayarak Log Analytics’e işlevsellik ekler.  Günlük Aramaları veya panodaki çözüm tarafından sağlanan ek kullanıcı arabirimi kullanılarak çözümlenebilecek yeni kayıt türlerinin toplanmasını da belirtebilirler.  Aşağıdaki örnek görüntüde [Değişiklik İzleme çözümü](log-analytics-change-tracking.md) gösterilmektedir

![Değişiklik İzleme çözümü](media/log-analytics-overview/change-tracking.png)

Çeşitli işlevlere yönelik çözümler mevcuttur ve sürekli olarak başka çözümler eklenmektedir.  Azure Market’ten mevcut çözümlere kolayca göz atabilir ve bu çözümleri [çalışma alanınıza ekleyebilirsiniz](log-analytics-add-solutions.md).  Çoğu çözüm otomatik olarak dağıtılıp hemen çalışmaya başlarken, bazıları biraz yapılandırma gerektirebilir.

![Çözüm Galerisi](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-components"></a>Log Analytics bileşenleri
Log Analytics’in merkezi, Azure bulutunda barındırılan verilerin toplandığı bir depodur.  Veriler, veri kaynakları yapılandırılarak ve aboneliğinize çözümler eklenerek bağlı kaynaklardan toplanır.  Veri kaynakları ve çözümler, kendi özellikleri olan, ancak depoya yapılan sorgularda yine de birlikte analiz edilebilen farklı kayıt türleri oluşturacaktır.  Böylece farklı kaynaklar tarafından toplanan farklı veri türleriyle çalışmak için aynı araçları ve yöntemleri kullanabilirsiniz.

![Log Analytics bileşenleri](media/log-analytics-overview/overview.png)

Bağlı kaynaklar, Log Analytics tarafından toplanan verileri oluşturan bilgisayarlar ve diğer kaynaklardır.  Bu, doğrudan bağlanan [Windows](log-analytics-windows-agent.md) ve [Linux](log-analytics-linux-agents.md) bilgisayarlarına yüklenmiş aracıları veya [bağlı System Center Operations Manager yönetim grubundaki](log-analytics-om-agents.md) aracıları içerebilir.  Azure kaynakları için Log Analytics, [Azure İzleyici ve Azure Tanılama](log-analytics-azure-storage.md)’dan veri toplar.

[Veri kaynakları](log-analytics-data-sources.md), bağlı her kaynaktan toplanan farklı veri türleridir.  Bunlar, [Windows](log-analytics-data-sources-windows-events.md) ve Linux aracılarından alınan [olaylar](log-analytics-data-sources-windows-events.md) ile [performans verilerini](log-analytics-data-sources-performance-counters.md) ve [IIS günlükleri](log-analytics-data-sources-iis-logs.md) ile [özel metin günlükleri](log-analytics-data-sources-custom-logs.md) gibi kaynakları içerir.  Toplamak istediğiniz her veri kaynağını yapılandırabilirsiniz. Yapılandırma, otomatik olarak bağlı her kaynağa dağıtılır.

Özel gereksinimleriniz varsa, bir REST API istemcisinden depoya veri yazmak üzere [HTTP Veri Toplayıcı API’sini](log-analytics-data-collector-api.md) kullanabilirsiniz.

## <a name="log-analytics-architecture"></a>Log Analytics mimarisi
Log Analytics’in, merkezi bileşenleri Azure bulutunda barındırıldığı için çok az dağıtım gereksinimi vardır.  Bu bileşenler, deponun yanı sıra toplanan verileri ilişkilendirmenizi ve çözümlemenizi sağlayan hizmetleri içerir.  Portala herhangi bir tarayıcıdan erişilebilir, bu nedenle herhangi bir istemci yazılımı gereksinimi yoktur.

[Windows](log-analytics-windows-agent.md) ve [Linux](log-analytics-linux-agents.md) bilgisayarlarındaki aracıları yüklemeniz gerekir, ancak zaten [bağlı System Center Operations Manager yönetim grubunun](log-analytics-om-agents.md) üyesi olan bilgisayarlar için ek aracıya gerek yoktur.  Operations Manager aracıları, verilerini Log Analytics'e yönlendiren yönetim sunucuları ile iletişim kurmaya devam eder.  Ancak bazı çözümler, aracıların doğrudan Log Analytics ile iletişim kurmasını gerektirecektir.  Her çözüme yönelik belgeler, iletişim gereksinimlerini belirtecektir.

[Log Analytics için kaydolduğunuzda](log-analytics-get-started.md) bir çalışma alanı oluşturursunuz.  Çalışma alanını kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir Log Analytics ortamı olarak düşünebilirsiniz. Aboneliğinizde üretim ve test gibi birden fazla ortamı desteklemek için birden fazla çalışma alanı oluşturabilirsiniz.

![Log Analytics mimarisi](media/log-analytics-overview/architecture.png)

## <a name="next-steps"></a>Sonraki adımlar
* Kendi ortamınızda test etmek üzere [ücretsiz bir Log Analytics hesabı için kaydolun](log-analytics-get-started.md).
* Log Analytics’e veri toplayabilen farklı [Veri Kaynaklarını](log-analytics-data-sources.md) görüntüleyin.
* Log Analytics’e işlev eklemek için [Çözüm Galerisi’ndeki mevcut çözümlere göz atın](log-analytics-add-solutions.md).

