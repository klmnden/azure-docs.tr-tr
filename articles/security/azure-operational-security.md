---
title: Azure operasyonel güvenlik | Microsoft Docs
description: Microsoft Azure İzleyici günlüklerine, hizmetlerinin ve nasıl çalıştığı hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: ab5b50433b85416ff471546171998e992293b0ea
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60587009"
---
# <a name="azure-operational-security"></a>Azure çalışma güvenliği
## <a name="introduction"></a>Giriş

### <a name="overview"></a>Genel Bakış
Güvenlik bulutta iş ve nasıl önemli Azure güvenliği hakkında doğru ve güncel bilgi bulmak olduğunu olduğunu biliyoruz. Azure uygulamaları ve Hizmetleri için kullanılacak en iyi nedenlerden biri dolayısıyla çeşit güvenlik araçları ve özellikleri yararlanmak sağlamaktır. Güvenli Azure platformunda güvenli çözümleri kolayca oluşturmasına olanak sağlar, bu araçları ve özellikleri yardımcı olur. Windows Azure gizlilik, bütünlük ve kullanılabilirlik Müşteri verilerinin, saydam sorumluluk etkinleştirirken sağlamanız gerekir.

Güvenlik denetimleri dizisi Microsoft Azure hem müşterinin uygulanan ve Microsoft operasyonel Perspektifler, "Azure çalışma güvenliği", bu teknik incelemede, yazıldığı daha iyi anlamasına yardımcı olmak için kapsamlı bir sağlar. Windows Azure ile kullanılabilir işletimsel güvenlik bakın.

### <a name="azure-platform"></a>Azure platformu
Azure çok sayıda işletim sistemi, dilleri, çerçeveler, Araçlar, veritabanları ve cihazlar programlama destekleyen bir genel bulut hizmeti platformudur. Linux kapsayıcılarını Docker tümleştirmesiyle çalıştırabilirsiniz; JavaScript, Python, .NET, PHP, Java ve Node.js kullanarak uygulamalar oluşturun; arka iOS, Android ve Windows cihazları uçlar oluşturun. Azure bulut hizmeti, milyonlarca Geliştirici teknolojileri destekler ve BT profesyonellerinin zaten kullandığı ve güvendiği.

Oluşturmak ya da BT varlıklarınızı geçirme uygulamalar ve hizmetler ve denetimleri ile verileri korumak için bir kuruluşun yeteneklerine bağlı bir genel bulut hizmet sağlayıcısı, bulut tabanlı varlıklarınızı gizliliğini yönetmenizi sağlarlar.

Azure altyapısı tesisten uygulamalara milyonlarca müşteriye aynı anda barındırmak için tasarlanmıştır ve işletmelerin güvenlik gereksinimlerine bağlı karşılayabilecek güvenilir bir temel sunar. Ayrıca, Azure çeşit yapılandırılabilir güvenlik seçenekleri ve kuruluşunuzun dağıtımlarının benzersiz gereksinimleri karşılamak için güvenlik özelleştirebilmeniz için bunları denetleme olanağı ile sağlar. Bu belge Azure güvenlik anlamanıza yardımcı yetenekleri, bu gereksinimleri karşılayan yardımcı olabilir.

### <a name="abstract"></a>Özet
Azure operasyonel güvenlik hizmetleri, denetimleri ve kullanıcılara sunulan özellikleri verilerini, uygulamalarını ve diğer varlıklardan Microsoft azure'da korumak için ifade eder. Azure çalışma Güvenliği aracılığıyla edinilen Microsoft Security Development Lifecycle (SDL), Microsoft Security Response Center programı da dahil olmak üzere Microsoft'a özgü çeşitli özellikleri bilgi içeren bir çerçevesi üzerine inşa edilmiştir, ve siber güvenlik tehditleri hakkındaki ayrıntılı tanıma.

Bu teknik incelemeyi Microsoft'un Azure işletimsel güvenlik için Microsoft Azure bulut platformu içerisindeki yaklaşımlar ve Hizmetleri kapsar:
1.  [Azure İzleyici](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

2.  [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)

3.  [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

4.  [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

5.  [Azure depolama analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Azure Active directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)


## <a name="microsoft-azure-monitor-logs"></a>Microsoft Azure izleme günlükleri

Hibrit bulut BT yönetim çözümü olan Microsoft Azure İzleyici günlüklerine. Tek başına kullanılan veya mevcut System Center dağıtım, Azure İzleyici günlüklerine genişletmek için size maksimum esneklik ve denetim için bulut tabanlı yönetim altyapınızın sağlar.

![Azure İzleyici günlükleri](./media/azure-operational-security/azure-operational-security-fig1.png)

Azure İzleyici günlüklerine ile rekabet çözümlerine göre daha düşük bir maliyetle, şirket içi, Azure, AWS, Windows Server, Linux, VMware ve OpenStack, dahil, herhangi bir buluttaki herhangi bir örneğine yönetebilirsiniz. Bulut öncelikli dünyada için tasarlanan Azure İzleyici günlüklerine yeni iş sorunlarını karşılamak ve uygulamaları ve bulut ortamları yeni iş yüklerine uyum sağlamak için en hızlı ve en ekonomik yolu olan Kurumsal yönetmek için yeni bir yaklaşım sunar.

### <a name="azure-monitor-services"></a>Azure izleme Hizmetleri

Azure İzleyici günlüklerine temel işlevlerini, Azure'da çalışan hizmetleri kümesi tarafından sağlanır. Her hizmet belirli bir yönetim işlevi sağlar ve farklı hizmetleri birleştirerek farklı yönetim senaryoları elde edebilirsiniz.

| Hizmet  | Açıklama|
| :------------- | :-------------|
| Azure İzleyici günlükleri | Fiziksel ve sanal makineler dahil olmak üzere çeşitli kaynakların kullanılabilirliğini ve performansını izleyin ve analiz edin. |
|Otomasyon | El ile gerçekleştirilen işlemleri otomatikleştirin; fiziksel ve sanal makinelere yapılandırma uygulayın. |
| Backup | Yedekleme ve kritik veri geri yükleme. |
| Site Recovery | Kritik uygulamalar için yüksek kullanılabilirlik sağlayın. |

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure İzleyici günlüklerine](https://azure.microsoft.com/documentation/services/log-analytics) yönetilen kaynaklardan toplanan verileri merkezi bir depoya toplayarak izleme hizmetleri sağlar. Bu verilere olaylar, performans verileri ya da API aracılığıyla sağlanan özel veriler dahil olabilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir.


Bu yöntem, çeşitli kaynaklardan gelen verileri birleştirmenize olanak tanır, birleştirebildiğiniz şekilde Azure hizmetlerinizi mevcut olan verileri şirket içi ortamınızdaki. Ayrıca, veri toplama işlemini veriler üzerinde gerçekleştirilen eylemden ayırarak tüm eylemlerin her tür veri üzerinde kullanılabilmesini mümkün kılar.


![Azure İzleyici günlükleri](./media/azure-operational-security/azure-operational-security-fig2.png)

Azure İzleyici hizmeti aşağıdaki yöntemleri kullanarak buluttaki verilerinizi güvenli bir şekilde yönetir:
-   veriler arasında ayrım yapma
-   veri saklama
-   fiziksel güvenlik
-   Olay yönetimi
-   Uyumluluk
-   güvenlik standartları sertifikaları

### <a name="azure-backup"></a>Azure Backup

[Azure yedekleme](https://azure.microsoft.com/documentation/services/backup) veri yedekleme ve Hizmetleri geri yükleme ve ürün ve hizmetlerini Azure İzleyici Suite'in bir parçası olan sağlar.
Uygulama verilerinizi korur ve herhangi bir sermaye yatırımı olmadan en düşük işletim giderleriyle yıllar boyunca saklar. Bu SQL Server ve SharePoint gibi uygulama iş yüklerinin yanı sıra fiziksel ve sanal Windows sunucularındaki verileri yedekleyebilirsiniz. Tarafından da kullanılabilir [System Center Data Protection Manager (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) yedeklilik ve uzun vadeli depolama için korumalı verilerin Azure'a çoğaltmak için.


Azure Backup'ta korunan veriler belirli bir coğrafi bölgede yer alan bir yedekleme kasasında depolanır. Veriler aynı bölge içinde çoğaltılır ve kasa türüne bağlı olarak, daha fazla dayanıklılık için başka bir bölgede de çoğaltılabilir.

### <a name="management-solutions"></a>Yönetim Çözümleri
[Azure İzleyici](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan Microsoft'un bulut tabanlı BT yönetim çözümüdür.


[Yönetim çözümleri](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) logics bir veya daha fazla Azure izleme hizmetlerini kullanarak belirli bir yönetim senaryosunu uygulayan önceden paketlenmiş kümeleridir. Microsoft ve iş ortakları, kolayca Azure İzleyicisi'nde yatırımınızın değerini artırmak için Azure aboneliğinize ekleyebileceğinizi farklı çözümler vardır. Bir iş ortağı olarak, uygulamalarınızı ve hizmetlerinizi desteklemek ve bunları Azure Market veya hızlı başlangıç şablonları aracılığıyla kullanıcılara sağlamak için kendi çözümlerinizi oluşturabilir.


![Yönetim Çözümleri](./media/azure-operational-security/azure-operational-security-fig4.png)

Ek işlevsellik sağlamak için birden fazla hizmet kullanan bir çözüm için iyi bir örnek [güncelleştirme yönetimi çözümünü](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management). Bu çözümü kullanan [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) hakkında bilgi toplamak, Windows ve Linux için aracı her bir aracı güncelleştirmeleri gerekiyor. Bu veriler, burada dahil olan bir panoyla çözümleyebilirsiniz Azure İzleyici günlükleri deposuna yazar.

Runbook'ları bir dağıtım oluşturduğunuzda [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) gerekli güncelleştirmeleri yüklemek için kullanılır. Bu işlemi baştan sona portalda yönetirsiniz ve altyapısal ayrıntılar konusunda endişelenmeniz gerekmez.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, Azure kaynaklarınızı korumanıza yardımcı olur. Bu, Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Belirleyebilir hizmet içinde yalnızca Azure aboneliklerinize karşı aynı zamanda karşı ilkeleri [kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups), daha ayrıntılı olabilir.

### <a name="security-policies-and-recommendations"></a>Güvenlik ilkeleri ve öneriler

Güvenlik ilkeleri, belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetim kümesini tanımlar.

Güvenlik Merkezi'nde, şirketinizin güvenlik gereksinimleri ve uygulamaların türü veya verilerin duyarlılığına göre ilkeleri tanımlarsınız.

![Güvenlik ilkeleri ve öneriler](./media/azure-operational-security/azure-operational-security-fig5.png)


Abonelik düzeyinde otomatik olarak etkinleştirilen ilkeler, sağ tarafındaki diyagramda gösterildiği gibi Abonelikteki tüm kaynak gruplarına yayılır:


### <a name="data-collection"></a>Veri toplama

Güvenlik Merkezi, verilerinizin güvenlik durumunu değerlendirmek, güvenlik önerileri sağlamak ve sizi tehditlere karşı uyarmak için sanal makinelerinizden (VM) veri toplar. Ne zaman, aboneliğinizdeki tüm sanal makineler ilk erişiminizi Güvenlik Merkezi, veri toplama etkinleştirilir. Veri toplama önerilir, ancak Güvenlik Merkezi ilkesinde veri toplamayı kapatarak bu seçimi kaldırabilirsiniz.

### <a name="data-sources"></a>Veri kaynakları

- Azure Güvenlik Merkezi, güvenlik durumunuzu görüntüleme, güvenlik açıklarını tanımlayıp çözümler önerme ve etkin tehditleri saptama amacıyla aşağıdaki kaynaklarda bulunan verileri analiz eder:

-   Azure Hizmetleri: Azure hizmetlerinin yapılandırmasına ilişkin ilgili hizmetin kaynak sağlayıcısıyla iletişim kurarak dağıttığınız bilgileri kullanır.

- Ağ trafiği: Microsoft'un altyapısından kaynak/hedef IP/bağlantı noktası gibi ağ trafiği meta verilerini, paket boyutu ve ağ protokolü kullanan örnek.

-   İş ortağı çözümleri: Güvenlik duvarları ve kötü amaçlı yazılımdan koruma çözümleri gibi tümleşik iş ortağı çözümlerinden güvenlik uyarılarını kullanır.

-   Sanal makineleriniz: Yapılandırma bilgilerini ve Windows olay ve Denetim günlükleri, IIS günlükleri, syslog iletileri ve sanal makinelerinizdeki kilitlenme dökümü dosyaları gibi güvenlik olaylarına ilişkin bilgileri kullanır.

### <a name="data-protection"></a>Veri koruma

Müşterilerin tehditleri önlemesine, algılamasına ve yanıt vermesine yardımcı olmak amacıyla Azure Güvenlik Merkezi, güvenlikle ilgili veriler, meta veriler, olay günlükleri, kilitlenme dökümü dosyaları ve diğer verileri toplar ve işler. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır.

-   **Veriler arasında ayrım yapma**: Veriler hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler kuruluşa göre etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır.

-   **Veri erişimi**: Güvenlik önerileri sağlamak ve olası güvenlik tehditlerini araştırmak üzere Microsoft personeli erişebileceği toplanan bilgiler veya göre Azure Hizmetleri, analiz kilitlenme dökümü dosyaları gibi oluşturma olayları, VM diski anlık görüntüleri ve yapıtları, işlem, yanlışlıkla müşteri verilerini ya da sanal makinelerinizdeki kişisel verileri içerebilir. Türetmeyeceğini [Microsoft çevrimiçi hizmet koşulları ve gizlilik bildirimini](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), Microsoft olmayan hangi durum müşteri verilerini veya reklam ya da benzeri ticari amaçlarla bundan bilgi.

-   **Veri kullanımı**: Microsoft desenleri ve birden fazla kiracıda görülen tehdit önleme ve algılama özelliklerimizi geliştirmek için kullanır; Bunu açıklanan gizlilik taahhütlerine uygun şekilde yaptığımız bizim [gizlilik bildirimi](https://www.microsoft.com/en-us/privacystatement/OnlineServices/).

### <a name="data-location"></a>Veri konumu

Azure Güvenlik Merkezi, kilitlenme döküm dosyalarının kısa ömürlü kopyalarını toplar ve açıktan yararlanma girişimlerinin ve başarılı uzlaşmaların kanıtı olarak analiz eder. Azure Güvenlik Merkezi bu analizi çalışma alanıyla aynı coğrafi bölgede gerçekleştirir ve analiz tamamlandığında kısa ömürlü kopyaları siler. Makine yapıları VM ile aynı bölgede merkezi olarak depolanır.

-   **Depolama hesaplarınızı**: Sanal makinelerinin çalıştığı her bölge için bir depolama hesabı belirtilir. Bunun yapılması verileri, verilerin toplandığı sanal makine ile aynı bölgede depolamanızı sağlar.

-   **Azure Güvenlik Merkezi deposunda**: İş ortağı uyarıları, öneriler ve güvenlik sistem durumu gibi güvenlik uyarıları hakkında bilgi merkezi, şu anda Amerika Birleşik Devletleri'nde depolanır. Bu bilgiler size güvenlik uyarısı, öneri veya sistem durumu sağlamak üzere gerektiğinde sanal makinelerinizden toplanan ilgili yapılandırma bilgilerini ve güvenlik olaylarını içerebilir.


## <a name="azure-monitor"></a>Azure İzleyici

[Azure İzleyici güvenlik günlükleri](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) ve denetim çözümü, BT'nin güvenlik olayların etkisini en aza yardımcı olabilecek tüm kaynaklar, etkin bir şekilde izlemek için olanak tanır. Azure İzleyici günlüklerine güvenlik ve denetim kaynakları izlemek için kullanılan güvenlik etki alanları vardır. Güvenlik etki alanı seçenekleri hızlı erişim sağlar, güvenlik izleme için aşağıdaki etki alanları ayrıntılı ele alınmıştır:

-   Kötü amaçlı yazılım değerlendirmesi
-   Güncelleştirme değerlendirmesi
-   Kimlik ve erişim.

Azure İzleyici, belirli tür kaynak işaretçileri bilgi sağlar. Görselleştirme, sorgu, yönlendirme, uyarı, Otomatik ölçek ve Otomasyon veriler hem Azure altyapısı (Etkinlik günlüğü) ve tek tek her Azure kaynağı (tanılama günlükleri) sunar.

![Azure İzleyici](./media/azure-operational-security/azure-operational-security-fig6.png)


Bulut uygulamaları ile birçok hareketli parçadan karmaşıktır. İzleme, uygulama güncel kalıp emin olmak için veri ve sağlam bir durumda çalışmasını sağlar. Ayrıca olası sorunları stave veya olanları sorun gidermeye yardımcı olur.

Ayrıca, uygulamanızı daha ayrıntılı Öngörüler elde etmek için izleme verilerini kullanabilirsiniz. Hakkında bilgi sahibi uygulama performans ve sürdürülebilirliği geliştirmek için yardımcı veya aksi halde el ile müdahale gerektiren eylemleri otomatikleştirme.

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü


Aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlayan bir günlüktür. Abonelikleriniz için Denetim düzlemi olayları raporları olduğundan Etkinlik günlüğünü daha önce "Denetim günlüklerini" veya "Operasyonel günlükler," olarak bilinir.

![Azure etkinlik günlüğü](./media/azure-operational-security/azure-operational-security-fig7.png)

Etkinlik günlüğü'nü kullanarak belirleyebilirsiniz ' ne, kim ve ne zaman ' işlemlerini (PUT, POST, DELETE), aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen herhangi bir yazma için. Ayrıca, işlemi ve ilgili diğer özellikleri durumunu anlayabilirsiniz. Etkinlik günlüğünü okuma (GET) işlemlerini ya da Klasik modeli kullandığınız kaynaklar için işlemleri içermez.

### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

Bu günlükler bir kaynak tarafından gönderilir ve bu kaynağın işlemiyle ilgili zengin, sık kullanılan veriler sağlar. Bu günlüklerin içeriği kaynak türüne göre değişir.

Örneğin, Windows olayı sistem günlükleri VM'ler ve blob, tablo için tanılama günlüğünü bir kategorisi olan ve kuyruk depolama hesapları için tanılama günlüklerini kategorilerini günlüklerdir.

Tanılama günlükleri tümünden [etkinlik günlüğü (eski denetim günlüğü veya işlem günlüğü olarak bilinir)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Etkinlik günlüğü, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri, kaynağınızın kendisi tarafından gerçekleştirilen işlemler hakkında bilgi sağlar.

### <a name="metrics"></a>Ölçümler

Azure İzleyici, performans ve sistem durumunu azure'da iş yüklerinizi görünürlük elde etmek için telemetri kullanmasına olanak tanır. Azure telemetri verilerinin en önemli ölçümleri (performans sayaçları olarak da bilinir) çoğu Azure kaynakları tarafından gösterilen türüdür. Azure İzleyicisi'ni yapılandırma ve bunları kullanmak için çeşitli yollar sağlar [ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) izleme ve sorun giderme. Ölçüm değerli bir telemetri kaynağı olan ve aşağıdaki görevleri gerçekleştirmek olanak sağlar:

-   **Performans İzleme** çizim portal grafiğinde, ölçümleri ve söz konusu panoya grafik sabitleme kaynağınızın (örneğin, bir sanal makine, Web sitesi veya mantıksal uygulama).

-   **Bir sorun bildirim alma** , etkiler kaynağınızın performansını bir ölçüm belirli bir eşiği geçtiğinde.

-   **Otomatik eylemler yapılandırma**, otomatik ölçeklendirme bir kaynağa veya bir ölçüm belirli bir eşiği geçtiğinde bir runbook tetikleme gibi.

-   **Gelişmiş analiz gerçekleştirmesine** veya kaynağınızın performansını ya da kullanım eğilimlerini raporlama.

-   **Arşiv** kaynağınızın uyumluluk veya denetim amacıyla performans veya sistem durumu geçmişi.

### <a name="azure-diagnostics"></a>Azure Tanılama

Azure'da dağıtılan bir uygulamada tanılama verilerinin toplanmasını sağlayan bir özelliktir. Tanılama uzantısını çeşitli farklı kaynaklardan kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti Web ve çalışan rolleri](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure sanal makineler](https://docs.microsoft.com/azure/virtual-machines/windows/overview) Microsoft Windows çalıştıran ve [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics). Diğer Azure Hizmetleri, kendi ayrı tanılama vardır.

## <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi

Ağ güvenlik denetimi, ağ güvenlik açıklarını algılama ve BT güvenlik ve Mevzuat idare modeli ile uyumluluk sağlamak için önemlidir. Güvenlik grubu görünümü ile yapılandırılan ağ güvenlik grubu ve güvenlik kuralları ve geçerli güvenlik kuralları alabilirsiniz. Uygulanan kuralları listesiyle açık olduğundan ve ağ güvenlik açığı değerlendirme bağlantı noktaları belirleyebilirsiniz.

[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) koşulları içinde azure'a veya azure'dan ağ düzeyinde izlemenizi ve tanılamanızı sağlayan bölgesel bir hizmettir. Ağ Tanılama ve görselleştirme araçları Ağ İzleyicisi ile kullanılabilen anlamanıza, tanılamanıza ve ağınıza azure'da Öngörüler elde etmeye yardımcı olur. Bu hizmet içeren paket yakalama, sonraki atlama IP akışı doğrulama, güvenlik grubu görünümü, NSG akış günlükleri. Senaryo düzeyi izleme ağ kaynaklarını tek tek ağ kaynak izleme aksine bir uçtan uca görünümünü sağlar.

![Azure Ağ İzleyicisi](./media/azure-operational-security/azure-operational-security-fig8.png)

Ağ İzleyicisi şu anda aşağıdaki özellikleri içerir:

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">Denetim günlükleri</a>**-ağ yapılandırmasının bir parçası gerçekleştirilen işlemleri günlüğe kaydedilir. Bu günlükler, Azure portalında görüntülenebilir veya veya üçüncü taraf araçları gibi Power BI Microsoft araçlarını kullanarak alınır. Denetim günlükleri, portal, PowerShell, CLI ve Rest API kullanılabilir. Denetim günlükleri hakkında daha fazla bilgi için bkz: Resource Manager ile işlemleri denetleme. Denetim günlükleri, tüm ağ kaynaklarında yapılan işlemler için kullanılabilir.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">IP akışı doğrular </a>**  -tabanlı akış bilgileri 5-tuple paket parametrelere (hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol) bir paket izin verilip verilmediğini denetler. Paket bir ağ güvenlik grubu tarafından reddedilirse, kural ve paketi reddeden ağ güvenlik grubu döndürülür.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Sonraki atlama</a>**  -kullanıcı tanımlı yollar herhangi tanılamanıza olanak sağlayarak Azure ağ Dokusunda yönlendirilen paketleri yanlış sonraki atlama belirler.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Güvenlik grubu görünümü</a>**  -bir VM üzerinde uygulanan etkili ve uygulanan güvenlik kuralları alır.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">NSG akış günlüğü</a>**  -akış günlüklerini ağ güvenlik grupları için izin verilen veya grubu güvenlik kuralları tarafından engellenen trafik ilgili günlükleri tutmak etkinleştirin. Akış, bir 5 demet bilgi tarafından – kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol tanımlanır.

## <a name="azure-storage-analytics"></a>Azure Depolama Analizi

[Depolama analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) toplu işlem istatistiklerini ve kapasite istekleriyle ilgili verileri bir depolama hizmetine dahil ölçümleri depolayabilir. İşlemler her iki API düzeyinde işlem ve depolama hizmet düzeyinde bildirilir ve kapasitesi depolama hizmet düzeyinde bildirilir. Ölçüm verileri, depolama hizmeti kullanımını çözümleme, depolama hizmeti karşı yapılan istekleri ile ilgili sorunları tanılayın ve hizmet kullanan uygulamaların performansını artırmak için kullanılabilir.

[Azure depolama analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) günlüğe kaydetme işlemlerini gerçekleştiren ve bir depolama hesabı için ölçüm verileri sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz. Depolama analizi günlük kaydı, kullanılabilir [Blob, kuyruk ve tablo hizmetlerine](https://docs.microsoft.com/azure/storage/storage-introduction). Depolama analizi, başarılı ve başarısız istekler hakkında ayrıntılı bilgi için bir depolama hizmetine kaydeder.

Bu bilgiler, tek tek istekleri izlemek için ve bir depolama hizmeti ile ilgili sorunları tanılamak için kullanılabilir. Bir en iyi çaba ilkesine göre istekleri günlüğe kaydedilir. Hizmet uç noktasına karşı yapılan istekler varsa, günlük girişi oluşturulur. Örnek bir depolama hesabı, kendi Blob uç noktası ancak kendi tablo veya kuyruk uç etkinliği varsa yalnızca günlükleri için Blob hizmetine ilişkin oluşturulur.

Depolama analizi kullanmak için onu ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirmeniz gerekir. İçinde etkinleştirebilirsiniz [Azure portalında](https://portal.azure.com/); Ayrıntılar için bkz: [Azure portalında depolama hesabı izleme](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Depolama analizi REST API veya istemci kitaplığı aracılığıyla programlı olarak da etkinleştirebilirsiniz. Depolama analizi, her hizmet için ayrı ayrı etkinleştirmek için hizmeti özelliklerini ayarla işlemine kullanın.

Toplanan veriler (günlük için) iyi bilinen bir blob ve tablo hizmeti API'leri ve Blob hizmeti kullanılarak erişilebilecek (ölçüm) için iyi bilinen tablolara depolanır.

Depolama analizi, depolama hesabınız için toplam limiti bağımsızdır depolanan veri miktarına 20 TB limiti vardır. Tüm günlükler depolanan [blok blobları](https://docs.microsoft.com/azure/storage/storage-analytics) $logs adlı bir kapsayıcıda, otomatik olarak oluşturulan depolama analizi için bir depolama hesabı etkin olduğunda.

Depolama analizi tarafından gerçekleştirilen aşağıdaki eylemler Faturalanabilir şunlardır:

-   Günlüğe kaydetme için BLOB'ları oluşturmak için istekler
-   Ölçümler için tablo varlıkları oluşturmak için istek sayısı.

> [!Note]
> Faturalandırma ve veri saklama ilkeleri hakkında daha fazla bilgi için bkz. [depolama analizi ve faturalandırma](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> En iyi performans için olası azalmayı önlemek için sanal makineye bağlı, yüksek oranda kullanılan disk sayısını sınırlamak istiyorsunuz. Tüm diskler yüksek oranda aynı anda kullanılan değil, daha büyük bir sayı disk depolama hesabını destekler.

> [!Note]
> Depolama hesabı sınırları hakkında daha fazla bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/storage-scalability-targets).


Aşağıdaki türde kimliği doğrulanmış ve anonim istekler kaydedilir.

| Kimliği Doğrulandı  | Anonim|
| :------------- | :-------------|
| Başarılı istekler | Başarılı istekler |
|Başarısız istek, zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere | Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istekleri |
| Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istekleri |İstemci ve sunucu zaman aşımı hataları |
|   Analiz veri istekleri |    304 (değiştirilmedi) hata koduyla başarısız GET istekleri |
| Depolama analizi kendisini günlük oluşturma veya silme gibi tarafından yapılan istekleri günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir listesi belgelenen [depolama analizi günlüğe yazılan işlemler ve durum iletileri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama analizi günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) konuları. | Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir listesi belgelenen [depolama analizi günlüğe yazılan işlemler ve durum iletileri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama analizi günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD eksiksiz çok faktörlü kimlik doğrulaması, cihaz kaydı, Self Servis parola yönetimi, Self Servis Grup Yönetimi, ayrıcalıklı hesap yönetimi, rol tabanlı erişim gibi kimlik yönetimi özelliklerini de içerir. Denetim, uygulama kullanımını izleme, zengin denetim ve güvenlik izleme ve uyarı verme.

-   Azure AD ile çok faktörlü kimlik doğrulama ve koşullu erişim ile uygulama güvenliğini artırın.

-   Uygulama kullanımını izlemek ve işinizi raporlama ve izleme güvenliğiyle Gelişmiş tehditlere karşı koruyun.

Azure Active Directory (Azure AD), dizininize yönelik güvenlik, etkinlik ve denetim raporlarını içerir. [Azure Active Directory denetim raporu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) müşterilerin kendi Azure Active Directory'de oluştu ayrıcalıklı Eylemler tanımlamak için yardımcı olur. Ayrıcalıklı Eylemler yükseltme değişiklikler (örneğin, rol oluşturma veya parola sıfırlama), değişen İlkesi yapılandırmalarını (örneğin, parola ilkelerini) veya dizin yapılandırması (örneğin, etki alanında Federasyon ayarlarını değişiklikler) değişiklikleri içerir.

Raporları, olay adı, değişiklik ve tarih ve saat (UTC) tarafından etkilenen hedef kaynak eylemi gerçekleştiren aktör için denetim kaydı sağlar. Müşteriler, Azure Active Directory denetim olaylarının listesi alamadı [Azure portalında](https://portal.azure.com/)anlatılan şekilde [denetim günlüklerinizi görüntülemek](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Kapsama dahil olan raporların listesi şu şekildedir:

| Güvenlik raporları  | Etkinlik raporları| Denetim raporları |
| :------------- | :-------------| :-------------|
|Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri | Uygulama kullanımı: özet | Dizin denetimi raporu |
|Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri | Uygulama kullanımı: ayrıntılı |   |
|Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri | Uygulama panosu |  |
|Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri |Hesap hazırlama hataları |  |
|Düzensiz oturum açma etkinliği |Bireysel kullanıcı cihazları |  |
|Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri |Bireysel kullanıcı Etkinliği |   |
|Anormal oturum açma etkinliği gösteren kullanıcılar |Grup etkinlik raporu |   |
| |Parola Sıfırlama Kayıt Etkinlik Raporu |   |
| |Parola sıfırlama etkinliği |   |



Bu raporların verileri SIEM sistemleri, Denetim ve iş zekası araçları gibi uygulamalarınız için yararlı olabilir. Azure AD raporlama [API'leri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve Araçlar menüsünden bu API'leri çağırabilirsiniz.

Azure AD Denetim Raporu olayları 180 gün boyunca saklanır.

> [!Note]
> Raporlarda saklama hakkında daha fazla bilgi için bkz. [Azure Active Directory rapor saklama ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Depolanırken ilgilenen kullanıcılar kendi [olaylarını denetleme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) raporlama API'si uzun saklama süreleri için denetim olayları ayrı veri deposuna düzenli olarak çekmek için kullanılabilir.

## <a name="summary"></a>Özet

Bu makalede özetleri gizliliğinizi ve yazılım ve yardımcı hizmetler sunarken, verilerinizin güvenliğini sağlama, kuruluşunuzun BT altyapısını yönetin. Microsoft, bunlar başkaları verilerini entrust bu güven katı güvenlik gerektiren tanır. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır. Güvenliğini sağlama ve veri koruma birincil Microsoft'ta bir önceliğe sahiptir.

Bu makalede açıklanır

-   Nasıl veri toplanan, işlenen ve Azure İzleyici paketindeki güvenli.

-   Birden çok veri kaynağında olayları hızla çözümleyin. Güvenlik ihlalinden doğabilecek zararları azaltmak için güvenlik risklerini tanımlayın, tehdit ve saldırıların kapsamını ve etkisini anlayın.

-   Giden kötü amaçlı IP trafiğini ve kötü amaçlı tehdit türlerini görselleştirerek saldırı desenlerini tanımlayın. Platformdan bağımsız olarak ortamınızın tamamının güvenlik açısından duruşunu anlayın.

-   Güvenlik ve uyumluluk denetimi için gerekli tüm günlük ve olay verilerini yakalayın. Saat ve eksiksiz, arama yapılabilen ve dışarı aktarılabilen günlük ve olay veri kümesi ile bir güvenlik denetim sağlamak için gereken kaynakları eğik çizgi.

<ul>
<li>Güvenlikle ilgili olaylar, Denetim ve varlıklarınızı gözünüzü tutmak İhlale yol açmak üzere analiz toplama:</li>
<ul>
<li>Güvenlik duruşunu</li>
<li>Önemli sorun</li>
<li>Özetleri tehditler</li>
</ul>
</ul>

## <a name="next-steps"></a>Sonraki Adımlar

- [Tasarım ve çalışma güvenliği](https://www.microsoft.com/trustcenter/security/designopsecurity)

Microsoft, Microsoft'un bulut altyapısının dayanıklı ve saldırılara karşı korunuyor olduğundan emin olun yardımcı olmak için güvenlikten ödün yazılım ve Hizmetleri tasarlar.

- [Azure İzleyici günlüklerine | Güvenlik ve uyumluluk](https://www.microsoft.com/cloud-platform/security-and-compliance)

Microsoft güvenlik veri ve analiz daha akıllı ve etkili tehdit algılama gerçekleştirmek için kullanın.

- [Azure Güvenlik Merkezi planlama ve işlemler](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide) adımları ve kuruluşunuzun güvenlik gereksinimlerine ve bulut Yönetimi modeline dayanarak Güvenlik Merkezi kullanımınızı iyileştirmek için izleyebileceğiniz bir dizi.

