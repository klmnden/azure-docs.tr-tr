---
title: Operations Management Suite'e (OMS) genel bakış | Microsoft Belgeleri
description: Microsoft Operations Management Suite (OMS), şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan, Microsoft'un bulut tabanlı BT yönetim çözümüdür.  Bu makalede OMS’nin değeri açıklanır, OMS'de yer alan farklı hizmetler ile teklifler tanımlanır ve bunların ayrıntılı içeriklerinin bağlantıları sağlanır.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: bwren
ms.openlocfilehash: 94dedebe48060441cd3167fea87f6b721eb14517
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
ms.locfileid: "26684023"
---
# <a name="what-is-operations-management-suite-oms"></a>Operations Management Suite (OMS) nedir?
Operations Management Suite (OMS) için bir giriş niteliğindeki bu makalede hizmetin sağladığı iş değerine, içerdiği hizmet ve yönetim çözümlerine ve farklı hizmet ve çözümleri bir paket altında toplayan tekliflere kısa bir genel bakış sunulmaktadır.  Her bir hizmet ve çözümün dağıtımı ve kullanımına yönelik ayrıntılı belgelerin bağlantıları da sağlanmıştır.

## <a name="from-on-premises-to-the-cloud"></a>Şirket içinden buluta
Microsoft, uzun bir süredir kurumsal ortamların yönetilmesine yönelik ürünler sağlıyor.  2007 yılında birden çok ürün System Center paketi olarak birleştirilmişti.  Bu paket, yazılım dağıtımı ve envanter gibi özellikler sunan Configuration Manager, sistemlerin ve uygulamaların proaktif olarak izlenmesine imkan tanıyan Operations Manager, el ile gerçekleştirilen işlemlerin otomatikleştirilmesini sağlayan runbook’ları içeren Orchestrator ve kritik önemdeki verilerin yedeklenip kurtarılması için Data Protection Manager’ı içeriyordu.

Buluta taşınan bilgi işlem kaynağı sayısı arttıkça System Center ürünlerine daha fazla bulut özelliği (Azure’da Operations Manager ve Orchestrator yönetim kaynakları gibi) eklendi.  Ne var ki bunlar hâlâ özünde şirket içi çözümler olarak tasarlanıyordu ve şirket içi yönetim ortamında dağıtılıp bakımlarının yapılabilmesi için ciddi bir yatırım gerekiyordu.  Bulutun tüm gücünden yararlanmak ve gelecekteki uygulamaları desteklemek için yeni bir yönetim yaklaşımı gerekiyordu.

## <a name="introducing-operations-management-suite"></a>Operations Management Suite’e Giriş
Operations Management Suite (OMS olarak da bilinir), en başından itibaren bulutta tasarlanan bir yönetim hizmetleri koleksiyonudur.  Şirket içinde dağıtılması ve yönetilmesi gereken kaynakların aksine, OMS bileşenleri tamamen Azure'da barındırılır.  Çok az yapılandırma gerektirir ve yalnızca birkaç dakikada kullanmaya başlayabilirsiniz.  

- **En az maliyet ve dağıtım karmaşıklığı.**  OMS'nin tüm bileşenleri ve verileri Azure'da depolandığından, şirket içi bileşenlerin gerektirdiği karmaşıklık ve yatırım yükü olmaksızın kısa bir sürede hizmeti kullanmaya başlayabilirsiniz.
- **Ölçeğinizi bulut düzeyine çıkarın.**  Bulutta yalnızca gerçekten kullandığınız kaynaklar için ödeme yapma ve ihtiyaç duyduğunuz anda herhangi bir yüke uygun ölçeğe geçme olanağınız olduğundan, ihtiyacınız olmayan bilgi işlem kaynakları için ücret ödemeniz ya da depolama alanınızın bitmesi konusunda endişelenmeniz gerekmez.  Az sayıda kaynağı yöneterek kullanmaya başlayabilir, daha sonra ortamınızın tamamını kapsayacak şekilde ölçeği artırabilirsiniz.
- **En son özelliklerin sağladığı avantajlardan yararlanın.**  OMS hizmetlerine sürekli yeni özellikler eklenir ve mevcut özellikler güncelleştirilir.  Güncelleştirme dağıtımı gerekmeksizin her zaman en son özelliklere erişiminiz olur.
- **Tümleşik hizmetler.**  OMS hizmetlerinin her biri kendi başına önemli oranda değer sunarken, karmaşık yönetim senaryolarının çözülmesi için birlikte de çalışabilir.  Örneğin, Azure Otomasyonu’ndaki bir runbook, Azure Site Recovery ile bir yük devretme işlemini gerçekleştirebilir ve sonra bilgileri Log Analytics’te günlüğe kaydederek bir uyarı oluşturabilir.
- **Küresel bilgi.**  OMS’deki yönetim çözümlerinin her zaman en son bilgilere erişimi vardır.  Örneğin Güvenlik ve Denetim çözümü, dünyanın farklı yerlerinde algılanan en son tehditleri kullanarak bir tehdit analizi gerçekleştirebilir.
- **Her yerden erişim.**  Yönetim ortamınıza bir tarayıcı kullanabileceğiniz her yerden erişin.  İzleme verilerinize anında erişebilmek için akıllı telefonunuza OMS uygulamasını yükleyin.

### <a name="is-it-just-for-the-cloud"></a>Yalnızca bulut için mi?
OMS hizmetlerinin bulutta çalışıyor olması, şirket içi ortamınızı etkili bir şekilde yönetemeyecekleri anlamına gelmez.  Veri merkezinizdeki herhangi bir Windows ya da Linux bilgisayarına ekleyeceğiniz bir aracı, verileri Log Analytics'e göndererek bulut ya da şirket içindeki diğer tüm hizmetlerden toplanan verilerle birlikte analiz edilmesini sağlayabilir.  Azure Backup ve Azure Site Recovery ile buluttan yararlanarak şirket içi kaynaklar için yedekleme ve yüksek kullanılabilirlik sağlayabilirsiniz.  
Buluttaki runbook'lar genellikle şirket içi kaynaklarınıza erişemez ancak veri merkezinizde runbook'ları barındıracak bir veya daha fazla bilgisayara bir aracı yükleyebilirsiniz.  Bir runbook’u başlattığınızda yalnızca bulutta mı yoksa yerel bir çalışan olarak mı çalışacağını belirtirsiniz.

## <a name="hybrid-management-with-system-center"></a>System Center ile karma yönetim
Mevcut bir System Center yüklemeniz varsa bu bileşenleri OMS hizmetleriyle tümleştirerek hem şirket içi hem de bulut ortamlarınız için karma bir çözüm sağlayabilir ve her iki ürünün kendine özgü avantajlarından yararlanabilirsiniz.  Yönetilen aracıları bulutta analiz etmek için mevcut Operations Management yönetim grubunuzu Log Analytics’e bağlayın.  Verilerinizi bulutta yedeklemek için mevcut yedekleme işleminizi Data Protection Manager ile kullanın.  


## <a name="oms-services"></a>OMS hizmetleri
OMS’nin temel işlevleri Azure’da çalışan bir dizi hizmet tarafından sağlanır.  Her hizmet belirli bir yönetim işlevi sağlar ve farklı hizmetleri birleştirerek farklı yönetim senaryoları elde edebilirsiniz.

|| Hizmet | Açıklama |
|:--|:--|:--|
| ![Log Analytics](media/operations-management-suite-overview/icon-log-analytics.png) | Log Analytics | Fiziksel ve sanal makineler dahil olmak üzere çeşitli kaynakların kullanılabilirliğini ve performansını izleyin ve analiz edin. |
| ![Azure Otomasyonu](media/operations-management-suite-overview/icon-automation.png) | Automation | El ile gerçekleştirilen işlemleri otomatikleştirin; fiziksel ve sanal makinelere yapılandırma uygulayın. |
| ![Azure Backup](media/operations-management-suite-overview/icon-backup.png) | Backup | Kritik verileri yedekleyin ve geri yükleyin. |
| ![Azure Site Recovery](media/operations-management-suite-overview/icon-site-recovery.png) | Site Recovery | Kritik uygulamalar için yüksek kullanılabilirlik sağlayın. |

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics), yönetilen kaynaklardan toplanan verileri merkezi bir depoda birleştirerek OMS için izleme hizmetleri sağlar.  Bu verilere olaylar, performans verileri ya da API aracılığıyla sağlanan özel veriler dahil olabilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir.  Bu yöntem, çeşitli kaynaklardan gelen verileri birleştirerek Azure hizmetlerinizden topladığınız verileri mevcut şirket içi ortamınızdan toplananlarla birleştirmenize olanak tanır.  Ayrıca, veri toplama işlemini veriler üzerinde gerçekleştirilen eylemden ayırarak tüm eylemlerin her tür veri üzerinde kullanılabilmesini mümkün kılar.  

![Log Analytics’e genel bakış](media/operations-management-suite-overview/overview-log-analytics.png)

#### <a name="collecting-data"></a>Verileri toplama
Verileri analiz edilmek üzere Log Analytics deposuna taşımanın birçok farklı yolu vardır.

- **Windows veya Linux bilgisayarları ve sanal makineler.**  [Windows](../log-analytics/log-analytics-windows-agent.md) ve [Linux](../log-analytics/log-analytics-linux-agents.md) bilgisayarlarında ya da veri toplamak istediğiniz sanal makinelerde Microsoft Monitoring Agent’ı yüklersiniz.  Aracı, toplanması gereken olayları ve performans verilerini tanımlayan Log Analytics yapılandırmasından verileri otomatik olarak indirir.  Azure portalını kullanarak aracıyı Azure’da çalışan sanal makinelere kolayca yükleyebilirsiniz.  Mevcut bir Operations Manager ortamınız varsa yönetim grubunu Log Analytics’e bağlayabilir ve tüm mevcut aracılardan otomatik olarak veri toplamaya başlayabilirsiniz.
- **Azure hizmetleri.**  Log Analytics, Azure kaynaklarını izleyebilmeniz için [Azure Tanılama ve Azure İzleme](../log-analytics/log-analytics-azure-storage.md)’den alınan verileri depoda toplar.
- **Veri Toplayıcı API’si**  Log Analytics, [tüm istemcilerden toplanan verilerin doldurulması için bir REST API](../log-analytics/log-analytics-data-collector-api.md) içerir.  Bu sayede üçüncü taraf uygulamalardan veri toplayabilir ve özel yönetim senaryoları uygulayabilirsiniz.  Yaygın olarak kullanılan bir yöntem, Azure Otomasyonu’nda bir runbook kullanarak verileri toplamak ve sonra Veri Toplayıcı API’sini kullanarak verileri depoya yazmaktır.

#### <a name="reporting-and-analyzing-data"></a>Raporlama ve veri analizi
Log Analytics, depoda saklanan verilerin ayıklanması için güçlü bir sorgu dili içerir.  Tüm kaynaklardan toplanan veriler kayıt olarak depolandığından, birden çok kaynaktan gelen verileri tek bir sorguda analiz edebilirsiniz.
  
Log Analytics, geçici analize ek olarak bir sorgudan rapor oluşturmak ve verileri çözümlemek için birden çok yol sağlar.

- **Görünümler ve panolar.**  [Görünümler](../log-analytics/log-analytics-view-designer.md) ve [panolar](../log-analytics/log-analytics-dashboards.md), portaldaki bir sorgunun sonuçlarını görselleştirir.  Yönetim çözümleri genellikle çözümden toplanan verileri çözümleyen görünümler içerir.  Veri analizi için kendi özel görünümlerinizi de oluşturabilir ve bunları özel portalınızdan kolayca erişilebilecek şekilde paylaşabilirsiniz.
- **Dışarı aktarın.**  Herhangi bir sorgunun sonuçlarını Log Analytics dışında analiz etmek üzere dışarı aktarma seçeneğiniz vardır.  Hatta, çok kullanışlı görselleştirme ve analiz özellikleri sunan [Power BI](../log-analytics/log-analytics-powerbi.md)’ya yönelik düzenli bir dışarı aktarma işlemi zamanlayabilirsiniz.
- **Günlük Arama API’si.**  Log Analytics, [tüm istemcilerden veri toplanması için bir REST API](../log-analytics/log-analytics-log-search-api.md) içerir.  Bu sayede, program aracılığıyla depoda toplanan verilerle çalışabilir veya başka bir izleme aracından bu verilere erişebilirsiniz.

#### <a name="alerting"></a>Uyarı
Log Analytics bir sorun algıladığında sizi [proaktif olarak uyarabilir](../log-analytics/log-analytics-alerts.md) veya düzeltici bir eylem uygulayabilir.  Log Analytics’teki diğer tüm analizler gibi bu işlem de bir günlük aramasıyla gerçekleştirilir.  Bu arama düzenli bir zamanlamaya göre çalışır ve sonuçların belirli ölçütlerle eşleşmesi durumunda bir uyarı oluşturulur.

![Log Analytics uyarıları](media/operations-management-suite-overview/overview-alerts.png)

Uyarılar, Log Analytics deposunda bir uyarı kaydı oluşturmaya ek olarak aşağıdaki eylemleri de gerçekleştirebilir.

- **E-posta gönderme.**  Algılanan bir sorunla ilgili e-posta göndererek sizi proaktif olarak bilgilendirme.
- **Runbook.**  Log Analytics’teki bir uyarı, Azure Otomasyonu’ndaki bir runbook’u başlatabilir.  Bu işlem genellikle algılanan sorunu düzeltme girişiminde bulunmak için gerçekleştirilir.  Runbook, karşılaşılan sorunun Azure’da veya başka bir bulutta olması durumunda bulutta; fiziksel veya sanal bir makinede olması durumundaysa yerel bir aracıda başlatılabilir.
- **Web kancası.**  Bir uyarı tarafından bir web kancası başlatılıp günlük araması sonuçlarından toplanan veriler kancaya geçirilebilir.  Bu özellik, alternatif bir uyarı sistemi gibi dış hizmetlerle tümleştirme olanağı sağlar veya bir dış web sitesi için düzeltici eylem gerçekleştirme girişiminde bulunabilir.

### <a name="azure-automation"></a>Azure Otomasyonu
[Azure Otomasyonu](http://azure.microsoft.com/documentation/services/automation), OMS için işlem otomasyonu ve yapılandırma yönetimi sağlar.  El ile gerçekleştirilen işlemleri otomatikleştirir; fiziksel ve sanal makinelere yapılandırma uygulanmasına yardımcı olur.  

#### <a name="process-automation"></a>Süreç Otomasyonu
Azure Otomasyonu, PowerShell betiğini ya da PowerShell iş akışını temel alan [runbook](../automation/automation-runbook-types.md)'ları kullanarak el ile gerçekleştirilen işlemleri otomatikleştirir.  Azure Otomasyonu, birden çok runbook için ortak olabilecek değişkenler ve bir runbook’un kimlik doğrulama gerçekleştirmesi için gerekli olabilecek şifrelenmiş bilgilerinizi depolamanıza imkan sağlayan kimlik bilgileri ve bağlantılar gibi Runbook’ları destekleyen varlıklar da içerir.
Runbook’lar paketteki diğer hizmetler için de süreç otomasyonu sağlar.  Diğer hizmetlerin her birine PowerShell ile veya bir REST API aracılığıyla erişilebildiğinden, Log Analytics’te yönetim verileri toplama ya da Azure Backup ile bir yedekleme işlemi başlatma gibi işlevleri gerçekleştirecek runbook’lar oluşturabilirsiniz.

##### <a name="accessing-resources"></a>Kaynaklara erişme
Runbook’lar PowerShell’i temel aldığından, PowerShell cmdlet’leriyle erişilebilen tüm kaynakları yönetebilir.  Otomasyon hesabınıza [bir modül yüklediğinizde](../automation/automation-integration-modules.md), modül o hesaptaki tüm runbook’lar tarafından kullanılabilir hale gelir. 
 
Runbook’lar bulutta çalışırken buluttan erişilebilen tüm kaynaklara erişebilir.  Bu kaynaklara örnek olarak Azure aboneliğinizdeki kaynaklar, Amazon Web Services (AWS) gibi başka bir buluttaki kaynaklar ya da bir REST API aracılığıyla erişilebilen bir hizmet verilebilir.  Buluttaki runbook'lar herhangi bir kimlik bilgisi altında çalışmaz ancak eriştikleri kaynaklarda kimlik doğrulaması gerçekleştirmek için kimlik bilgileri, bağlantılar ve sertifikalar gibi Otomasyon varlıklarından yararlanabilir.

Veri merkezinizdeki kaynaklara bulutta çalışan bir runbook’tan büyük bir olasılıkla erişilemez.  Bununla birlikte, yerel kaynaklara erişim gerektiren runbook’ları çalıştırmak için veri merkezinizde bir veya daha fazla [Karma Runbook Çalışanı](../automation/automation-hybrid-runbook-worker.md) yükleyebilirsiniz.  Bir runbook’u başlattığınızda yalnızca bulutta mı yoksa belirli bir çalışan üzerinde mi çalışacağını belirtirsiniz.

![Azure Otomasyonu runbook’ları](media/operations-management-suite-overview/overview-runbooks.png)

##### <a name="starting-a-runbook"></a>Runbook başlatma
Runbook’lar, çeşitli yönetim senaryolarına dahil edilebilmeleri için [farklı metotlarla başlatılabilir](../automation/automation-starting-a-runbook.md).  

- **Azure Portal.**  Diğer Azure hizmetlerinde olduğu gibi Azure Otomasyonu Azure Portal’dan yönetilebilir.  Runbook’ları başlatmanın yanı sıra içeri aktarabilir veya kendi runbook’larınızı yazabilirsiniz.
- **Zamanlananlar.**  Runbook’ları düzenli aralıklarla başlayacak şekilde zamanlayabilirsiniz.  Bu sayede düzenli bir yönetim işlemini otomatik olarak yineleyebilir veya Log Analytics’e veri toplayabilirsiniz.
- **PowerShell ve API.**  Bir PowerShell cmdlet’inden ya da Azure Otomasyonu REST API’sinden runbook başlatabilir ve gerekli parametre bilgilerini runbook’a geçirebilirsiniz.  
- **Web kancası.**  Herhangi bir runbook’un dış uygulamalardan ya da web sitelerinden başlatılmasına imkan sağlayan web kancaları oluşturulabilir.
- **Log Analytics Uyarısı.**  Log Analytics’teki bir uyarı, uyarı tarafından tanımlanan sorunu düzeltme girişiminde bulunmak üzere otomatik olarak bir runbook başlatabilir.

#### <a name="configuration-management"></a>Yapılandırma Yönetimi
Windows PowerShell’deki [PowerShell İstenen Durum Yapılandırması (DSC)](../automation/automation-dsc-overview.md), fiziksel ve sanal makinelerin yapılandırmalarını dağıtmanıza ve uygulamanıza imkan tanıyan bir yönetim platformudur.  Azure Otomasyonu, DSC yapılandırmalarını yönetir ve bulutta aracıların gerekli yapılandırmaları almak için erişebileceği bir çekme sunucusu sağlar.

![Azure Automation DSC](media/operations-management-suite-overview/overview-dsc.png)

### <a name="azure-backup-and-azure-site-recovery"></a>Azure Backup ve Azure Site Recovery
Azure Backup ve Azure Site Recovery, iş devamlılığı ve olağanüstü durum kurtarmaya katkı sağlar.  İki hizmet de uygulamalarınızın kesintiler sırasında kullanılabilir kalmasını ve sistem yeniden çevrimiçi olduğunda normal işlemlere dönmesini sağlamanıza yardımcı olan özellikler içerir.  Her iki hizmet de kuruluşunuz için tanımlanan kurtarma noktası hedeflerine (RPO) ve kurtarma zamanı hedeflerine (RTO) katkıda bulunur. RPO'nuz, verilerin bir kesinti sırasında ne kadar süre kullanım dışı kalabileceğine ilişkin kabul edilebilir sınırı tanımlarken RTO, bir kesinti sırasında bir hizmet veya uygulamanın kullanım dışı kalabileceği kabul edilebilir süre miktarını kısıtlar.

#### <a name="azure-backup"></a>Azure Backup
[Azure Backup](http://azure.microsoft.com/documentation/services/backup), OMS için veri yedekleme ve geri yükleme hizmetleri sağlar.  Uygulama verilerinizi korur ve herhangi bir sermaye yatırımı olmadan en düşük işletim giderleriyle yıllar boyunca saklar.  SQL Server ve SharePoint gibi uygulama iş yüklerinin yanı sıra fiziksel ve sanal Windows sunucularındaki verileri yedekleyebilir.  Bu ayrıca yedeklilik ve uzun vadeli depolama açısından, korumalı verilerin Azure'a yönelik olarak çoğaltılması için System Center Data Protection Manager (DPM) tarafından da kullanılabilir.

Azure Backup'ta korunan veriler belirli bir coğrafi bölgede yer alan bir yedekleme kasasında depolanır. Veriler aynı bölge içinde çoğaltılır ve kasa türüne bağlı olarak, daha fazla dayanıklılık için başka bir bölgede de çoğaltılabilir.

Azure Backup için üç temel senaryo söz konusudur.

- **Azure Backup aracısının yüklediği Windows makinesi.** Herhangi bir Windows sunucusundaki veya istemcisindeki dosyaları ve klasörleri doğrudan Azure Backup Vault’a yedekleyin.<br><br>![Azure Backup aracısının yüklediği Windows makinesi](media/operations-management-suite-overview/overview-backup-01.png)
- **System Center Data Protection Manager (DPM) veya Microsoft Azure Backup Sunucusu.** DPM veya Microsoft Azure Backup Sunucusu’ndan yararlanarak SQL ve SharePoint gibi uygulama iş yüklerinin yanı sıra dosyaları ve klasörleri yerel depolama alanında yedekleyin ve sonra Azure Backup Vault’a çoğaltın. Hyper-V veya VMware’de Windows ve Linux sanal makinelerini destekler.<br><br>![System Center Data Protection Manager (DPM) veya Microsoft Azure Backup Sunucusu](media/operations-management-suite-overview/overview-backup-02.png)
- **Azure Sanal Makine Uzantıları.** Azure’daki Windows veya Linux sanal makinelerinizi Azure Backup Vault’a yedekleyin.<br><br>![Azure Sanal Makine Uzantıları](media/operations-management-suite-overview/overview-backup-03.png)



#### <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery), şirket içi sanal ve fiziksel makinelerin Azure’a veya ikincil bir konuma çoğaltılmasını düzenleyerek iş sürekliliği sağlar. Birincil konumunuzun kullanılamaması durumunda kullanıcılarınızın çalışmaya devam edebilmesi için ikincil konuma yük devreder; sistemler çalışır hale geldiğindeyse yeniden başlatma gerçekleştirirsiniz. 

Azure Site Recovery, sunucular ve uygulamalar için yüksek kullanılabilirlik sağlar.  Çoğaltma, yük devretme ve şirket içi Hyper-V sanal makinelerini, VMware sanal makinelerini, fiziksel Windows/Linux sunucularını kurtarma işlemlerini düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı sağlar. Makineleri ikincil bir veri merkezine yönelik olarak çoğaltabilir veya Azure'a yönelik çoğaltarak veri merkezinizi genişletebilirsiniz. Site Recovery aynı zamanda iş yükleri için basit bir yük devretme ve kurtarma işlevi sunar. SQL Server AlwaysOn gibi olağanüstü durum kurtarma mekanizmaları ile tümleştirilir ve birden çok makinede katmanlı halde bulunan iş yüklerine yönelik kolay bir yük devretme işlemi için kurtarma planları sağlar.

Azure Site Recovery için üç temel çoğaltma senaryosu söz konusudur.

- **Hyper-V sanal makinelerini çoğaltma.**  Hyper-V sanal makineleri VMM bulutlarında yönetiliyorsa, ikincil veri merkezine veya Azure depolama hizmetine yönelik olarak çoğaltma gerçekleştirebilirsiniz. Azure'a yönelik çoğaltma, güvenli bir İnternet bağlantısı üzerinden yapılır. İkincil veri merkezine yönelik çoğaltma ise LAN üzerinden yapılır.  Hyper-V sanal makineleri VMM tarafından yönetilmiyorsa yalnızca Azure depolama hizmetine yönelik olarak çoğaltma gerçekleştirebilirsiniz. Azure'a yönelik çoğaltma, güvenli bir İnternet bağlantısı üzerinden yapılır.<br><br>![Hyper-V sanal makinelerini çoğaltma](media/operations-management-suite-overview/overview-siterecovery-hyperv.png)
- **VMware sanal makinelerini çoğaltma.**  VMware sanal makinelerini VMware veya Azure depolama hizmetini çalıştıran ikincil bir veri merkezine yönelik olarak çoğaltabilirsiniz. Azure'a yönelik çoğaltma, siteler arası bir VPN veya Azure ExpressRoute üzerinden ya da güvenli bir İnternet bağlantısı yoluyla gerçekleşebilir. İkincil bir veri merkezine yönelik çoğaltma, InMage Scout veri kanalı üzerinden gerçekleşir.<br><br>![VMware sanal makinelerini çoğaltma](media/operations-management-suite-overview/overview-siterecovery-vmware.png)
- **Fiziksel Windows ve Linux sunucularını çoğaltma.**  Fiziksel sunucuları ikincil bir veri merkezine veya Azure depolama hizmetine yönelik olarak çoğaltabilirsiniz. Azure'a yönelik çoğaltma, siteler arası bir VPN veya Azure ExpressRoute üzerinden ya da güvenli bir İnternet bağlantısı yoluyla gerçekleşebilir. İkincil bir veri merkezine yönelik çoğaltma, InMage Scout veri kanalı üzerinden gerçekleşir. Azure Site Recovery, bazı istatistikler görüntüleyen bir OMS çözümüne sahiptir ancak işlemler için Azure portalını kullanmanız gerekir.<br><br>![Fiziksel Windows ve Linux sunucularını çoğaltma](media/operations-management-suite-overview/overview-siterecovery-physical.png)


Site Recovery, belirli bir coğrafi Azure bölgesinde yer alan kasalardaki meta verileri depolar. Site Recovery hizmeti tarafından çoğaltılmış veri depolanmaz.

## <a name="management-solutions"></a>Yönetim Çözümleri
[Yönetim Çözümleri](operations-management-suite-solutions.md), bir veya daha fazla OMS hizmetinden yararlanarak belirli bir yönetim senaryosunu uygulayan önceden paketlenmiş mantık kümeleridir.  Microsoft ve iş ortakları tarafından sağlanan ve OMS yatırımınızın değerini artırmak için Azure aboneliğinize kolayca ekleyebileceğiniz farklı çözümler vardır.  Bir iş ortağı olarak uygulama ve hizmetlerinizi desteklemeye yönelik kendi çözümlerinizi oluşturabilir ve Azure Market ya da Hızlı Başlangıç Şablonları aracılığıyla diğer kullanıcılara sağlayabilirsiniz.

[Güncelleştirme Yönetimi çözümü](oms-solution-update-management.md), birden çok hizmetten yararlanarak ek işlevsellik sağlayan çözümlere iyi bir örnektir.  Bu çözüm, Windows ve Linux için Log Analytics aracısını kullanarak her bir aracıdaki gerekli güncelleştirmeler hakkında bilgi toplar.  Bu veriler, dahil olan bir panoyla analiz edilebilecekleri Log Analytics deposuna yazılır.  Bir dağıtım oluşturduğunuzda, gerekli güncelleştirmelerin yüklenmesi için Azure Otomasyonu’ndaki runbook’lar kullanılır.  Bu işlemi baştan sona portalda yönetirsiniz ve altyapısal ayrıntılar konusunda endişelenmeniz gerekmez.

![Çözüm](media/operations-management-suite-overview/overview-solution.png)

Çoğu çözüm aşağıdaki işlevlerin birini veya daha fazlasını gerçekleştirebilir.

- Ek bilgi toplama.  Log Analytics, istemcilerden ve hizmetlerden olaylar ve performans verileri dahil çeşitli veriler toplar.  Bir yönetim çözümü, çoğunlukla Azure Otomasyonu runbook’larını kullanarak diğer veri kaynakları tarafından sağlanmayan ek bilgiler toplayabilir.
- Toplanan bilgilerin ek analizini gerçekleştirme.  Yönetim çözümleri, verilerin analizi edilmesini ve görselleştirilmesini sağlayan panolar ve görünümler içerir.  Bunlar, ayrıntılı verilerin detayına gitmenize olanak sağlayan önceden tanımlanmış günlük aramalarının geri bağlantılarını içerir.  Ayrıca, zaten depoda toplanmış veriler üzerinde analiz (örneğin, güvenlik olaylarında tehdit olarak görülebilecek desenler arama) gerçekleştirebilir.
- İşlev ekleme.  Microsoft tarafından sağlanan bazı çözümler, çekirdek hizmetlerin özelliklerini genişleterek ek işlevler sağlayabilir.  Örneğin, Hizmet Eşlemesi kendi keşif konsoluna sahiptir ve sunucu ile işlem bağımlılıklarını gerçek zamanlı olarak eşler.
OMS’ye Microsoft iş ortakları tarafından düzenli olarak yeni çözümler eklendiğinden, yatırımınızın değeri sürekli artar.  OMS portalındaki Çözüm Kataloğu aracılığıyla Microsoft çözümlerine göz atabilir ve istediğiniz çözümleri yükleyebilir ya da Azure Portal’daki Azure Market aracılığıyla hem Microsoft hem de iş ortağı çözümlerini görüp yükleyebilirsiniz.  

![Çözüm Galerisi](media/operations-management-suite-overview/solution-gallery.png)


## <a name="next-steps"></a>Sonraki adımlar
* [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) hakkında bilgi edinme.
* [Azure Otomasyonu](../automation/automation-intro.md) hakkında bilgi edinme.
* [Azure Backup](http://azure.microsoft.com/documentation/services/backup) hakkında bilgi edinme.
* [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) hakkında bilgi edinme.
* Farklı OMS tekliflerinde [sağlanan çözümleri](../log-analytics/log-analytics-add-solutions.md) keşfedin. 

