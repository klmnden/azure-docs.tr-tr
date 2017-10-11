---
title: "Operations Management Suite (OMS) tümleştirme | Microsoft Docs"
description: "OMS standart özelliklerini kullanarak ek olarak, diğer yönetim uygulamaları ve Hizmetleri ile bir karma yönetim ortamı sağlamak için özel yönetim senaryoları ortamınız için benzersiz sağlar ya da özel yönetim sağlamayı tümleştirebilirsiniz Müşterileriniz için deneyimi.  Bu makalede farklı seçeneklerinizi OMS ve ayrıntılı teknik bilgi sağlama makalelerinin bağlantıları ile tümleştirme için genel bir bakış sağlar."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fc5f3a8a-77f7-4103-bd7e-744c15ffcca7
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 7a24df6f2c3b2c091d1b66b8b9c0a61035ffde11
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="integrating-with-operations-management-suite-oms"></a>Operations Management Suite (OMS) ile tümleştirme
Operations Management Suite, yönetmek ve şirket içi korumak ve altyapı bulut yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür.  OMS standart özelliklerini kullanarak ek olarak, diğer yönetim uygulamaları ve Hizmetleri ile bir karma yönetim ortamı sağlamak için özel yönetim senaryoları ortamınız için benzersiz sağlar ya da özel yönetim sağlamayı tümleştirebilirsiniz Müşterileriniz için deneyimi.  Bu makalede farklı seçeneklerinizi OMS Hizmetleri ve ayrıntılı teknik bilgi sağlama makalelerinin bağlantıları ile tümleştirmek için genel bir bakış sağlar. 

## <a name="log-analytics"></a>Log Analytics
Günlük analizi tarafından toplanan yönetim verilerini Azure üzerinde barındırılan bir havuzda depolanır.  Depo içinde depolanan tüm verileri son derece büyük miktarlarda verinin hızlı analiz sağlayan günlük aramaları mevcut değil.  Tümleştirme gereksinimlerinizi depo çözümleme için kullanılabilir hale getirme yeni verilerle doldurmak için veya yeni bir görsel öğe sağlamak ya da başka bir yönetim aracı ile tümleştirmek için deposundaki verileri ayıklamak üzere olabilir.

Her veri deposunda parçasının bir kayıt olarak depolanır.  Depo doldurmak, kullanıcıların çözümünüzü kullanan kayıt türünü ve bir açıklama ve özelliklerini sağlamalıdır.  Verileri almak için bu bilgileri çalıştığınız veriler hakkında gerekir.

![OMS depo doldurma](media/operations-management-suite-integration/repository.png)

### <a name="populate-the-log-analytics-repository"></a>Günlük analizi depo doldurma
OMS depo doldurmak için birden çok yöntem bulunmaktadır.  Kullandığınız yöntem, kaynak verilerin bulunduğu, verileri ve hangi biçimi gibi etkenlere bağlıdır istemcileri desteklemek için ihtiyacınız.  Veri deposunda depolandıktan sonra nasıl toplanan fark etmez.

Aşağıdaki bölümlerde OMS depo doldurmak için farklı seçenekler açıklanmaktadır.

#### <a name="connected-sources-and-data-sources"></a>Bağlı kaynakları ve veri kaynakları
Bağlı kaynakları burada veri için OMS depo alınabilecek konumlardır.  Veri kaynakları ve çözümleri bağlı kaynakları çalıştırın ve toplanan belirli verileri tanımlayın.  Uygulamanız bu veri kaynaklarından biri için veri yazıyorsa sonra bu veri kaynağı yapılandırarak toplayabilirsiniz.  Syslog olayları, uygulamanızın oluşturduğu, örneğin, ardından bunlar Syslog veri kaynağı tarafından Linux Aracısı'nı toplanabilir.

* [Günlük analizi veri kaynaklarında](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Çözümler
Çözümleri OMS yeteneklerini genişletir.  Bir çözüm bağlı kaynaktan veri toplayabilir veya depoya toplanmış kayıtlar üzerinde analiz gerçekleştirebilir.  Microsoft tarafından sağlanan her bir çözüm topladığı veriler hakkında ayrıntılar sağlayan tek bir makale vardır.

* [Günlük analizi çözümleri](../log-analytics/log-analytics-add-solutions.md)

#### <a name="http-data-collector-api"></a>HTTP veri toplayıcı API
Günlük analizi HTTP Veri Toplayıcı günlük analizi depoya JSON verilerini eklemenizi sağlayan bir REST API API'dir.  Diğer veri kaynakları veya çözümleri biri verilerine sağlamaz bir uygulamanız varsa, bu API'yı kullanabilir.  API çağırabilir ve herhangi bir veri kaynağı veya çözüm koleksiyonu zamanlamada kalmaz herhangi bir istemciyi depodan doldurmak için kullanılabilir.

* [Günlük analizi HTTP veri toplayıcı API](../log-analytics/log-analytics-data-collector-api.md)

### <a name="retrieve-data-from-the-log-analytics-repository"></a>Günlük analizi depodan veri alma
OMS depodan veri almak için birden çok yöntem bulunmaktadır.  Kullanıcıların OMS konsolunu kullanarak verileri almak istediğiniz ve farklı türdeki görselleştirmeleri ve analiz verin.  Başka bir yönetim çözümü gibi bir dış işlem gelen verileri de alabilirsiniz.

#### <a name="log-searches"></a>Günlük aramalar
OMS deposunda depolanan tüm verileri günlük arar mevcut değil.  Kullanıcılar kendi geçici analiz OMS konsolundaki gerçekleştirme veya belirli günlük arama için bir görsel öğe içeren bir pano oluşturun.  Çözümleri, önceden tanımlanmış aramaları dayalı görselleştirmelerle özel görünümler içerebilir.  Bir dış uygulama veya yönetim aracından OMS deposundaki verilere erişme günlük arama API kullanabilirsiniz.  

* [Günlük analizi günlük aramalarda](../log-analytics/log-analytics-log-searches.md)
* [Günlük analizi günlük arama REST API'si](../log-analytics/log-analytics-log-search-api.md)
* [Günlük analizi cmdlet'leri](https://msdn.microsoft.com/library/mt188224.aspx)

#### <a name="custom-views"></a>Özel görünümler
Görünüm Tasarımcısı kullanıcılara Görselleştirme ve verilerin çözümünüzdeki çözümlemesini sağlamak OMS konsolunda özel görünümler oluşturmanıza olanak sağlar.  Her görünüm konsol ve tanımladığınız günlük aramaları temel görselleştirme bölümleri herhangi bir sayıda ana sayfada görüntülenen bir kutucuğu içerir.

* [Günlük analizi Görünüm Tasarımcısı](../log-analytics/log-analytics-view-designer.md)

#### <a name="power-bi"></a>Power BI
Görselleştirmeleri ve çözümleme araçları yararlanabilirsiniz şekilde günlük analizi otomatik olarak veri OMS depodan Power BI'a aktarabilirsiniz.  Verilerin güncel korunur, böylece bir zamanlamaya göre bu verme işlemi gerçekleştirir. 

* [Power BI için günlük analizi veri dışarı aktarma](../log-analytics/log-analytics-powerbi.md)

## <a name="automation"></a>Otomasyon
OMS işlemleri için toplanan verileri tepki vermek için veya diğer yönetim işlevleri gerçekleştirmek için otomatik hale getirebilirsiniz.  Uygulamanızdan veri toplamak ve OMS depoya ekleyin ya da deposunda bulunan verilere yanıt olarak bilinen bir sorun düzeltilmesi otomatikleştirebilir. 

![Otomasyon](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbook'lar
Azure Otomasyonu runbook'ları Azure bulutunda PowerShell komut dosyaları ve iş akışları çalıştırın.  Azure'daki kaynakları veya buluttan erişilebilen diğer kaynakları yönetmek için kullanabilirsiniz.  Runbook'ları karma Runbook çalışanı kullanarak bir yerel veri merkezinde da çalıştırılabilir.  Azure portalından veya dış işlemlere çok sayıda PowerShell ya da Otomasyon API gibi yöntemleri kullanarak bir runbook'u başlatabilirsiniz.

* [Azure Otomasyonu runbook başlatma](../automation/automation-starting-a-runbook.md)
* [Azure Automation cmdlet'leri](https://msdn.microsoft.com/library/dn690262.aspx)
* [Otomasyon REST API'si](https://msdn.microsoft.com/library/mt662285.aspx)
* [Otomasyon .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Uyarılar
Uyarı kuralları otomatik olarak bir zamanlamaya göre günlük aramaları çalıştırın.  Sonuçları belirli ölçütlere uyan varsa ortaya çıkan uyarı Azure Otomasyonu'nda bir runbook başlatın veya bir dış işlem başlatmak için bir Web kancası çağırın.  Bu yanıtları her ikisi de günlük aramada döndürülen verileri de dahil olmak üzere Uyarı ayrıntılarını içerebilir.

* [Günlük analizi uyarıları](../log-analytics/log-analytics-alerts.md)
* [Günlük analizi uyarı API](../log-analytics/log-analytics-api-alerts.md)

## <a name="backup-and-site-recovery"></a>Yedekleme ve Site kurtarma
Azure yedekleme ve Site Recovery, kurumsal veri koruma ve sunucuları ve uygulamaları kullanılabilir olmasını sağlamaya yönelik hizmetler sağlar.  Uygulamanız için yedekleme hizmetleri sağlamak veya bir sanal makinenin yük devretmeyi başlatmadan olarak bu senaryolara gerçekleştirmek için bu hizmetleri yararlanabilirsiniz.

* [Azure yedekleme cmdlet'lerini](https://msdn.microsoft.com/library/mt619253.aspx)
* [Azure Site Recovery REST API'si](https://msdn.microsoft.com/library/azure/mt750497.aspx)
* [Azure Site kurtarma cmdlet'leri](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Özel çözümler
Çalışma alanınızı veya Müşteri'nin çalışma çalıştırmak için özel bir çözüm içine tümleştirme mantığı kapsüller.  Çözümünüzü tam Yönetimi senaryosu sağlamak için diğer kaynaklara ek olarak bu makaledeki tümleştirme yöntemlerden herhangi birini içerebilir.  Çözüm kaldırıldığında, tüm oluşturulduğu kaynakları OMS çalışma ve Azure abonelik kaldırılır, çözüm kaynaklarında paketlenir.

Örneğin, çözümünüzü toplamak ve verileri işlemek ve HTTP veri toplayıcı API kullanarak günlük analizi depoyu doldurmak için bir Otomasyon runbook'u içerebilir.  Gösterir ve toplanan verileri çözümler özel bir görünümü de içerebilir.  

* (Yakında) özel çözümler oluşturma    

## <a name="next-steps"></a>Sonraki adımlar
* Başvuru [OMS SDK](operations-management-suite-sdk.md) OMS Hizmetleri otomatikleştirme hakkında teknik bilgi için.  

