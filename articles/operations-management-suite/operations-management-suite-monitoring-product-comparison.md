---
title: Microsoft Ürün Karşılaştırma izleme | Microsoft Docs
description: Microsoft Operations Management Suite (OMS) Microsoft'un bulut yönetmek ve şirket içi korumak ve altyapı bulut yardımcı olan BT yönetim çözümü dayalıdır.  Bu makalede, OMS'de yer alan farklı hizmetler tanımlanmakta ve bunların ayrıntılı içeriklerinin bağlantıları sağlanmaktadır.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: a63ca0ad-61f8-425d-a48c-d87ba518c104
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren
ms.openlocfilehash: b4201f105a87b0a41059c061eb37fb35d4514e02
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23866320"
---
# <a name="microsoft-monitoring-product-comparison"></a>Microsoft izleme ürün karşılaştırma
Bu makalede, System Center Operations Manager (SCOM) ve günlük analizi Operations Management Suite (OMS) arasında bir karşılaştırma mimarilerini, nasıl bunlar kaynakları izlemesini ve bunlar toplamak veri analizi nasıl gerçekleştirdikleri mantığı bakımından sağlanmaktadır. .  Bu, kendi farklar ve göreli gücü temel bir anlayış vermektir.  

## <a name="basic-architecture"></a>Temel mimari
### <a name="system-center-operations-manager"></a>System Center Operations Manager
Tüm SCOM bileşenleri veri merkezinizde yüklenir.  [Aracıları yüklü](http://technet.microsoft.com/library/hh551142.aspx) Windows ve Linux makinelerde SCOM tarafından yönetilir.  Aracıları bağlanmak için [yönetim sunucuları](https://technet.microsoft.com/library/hh301922.aspx) SCOM veritabanı ve veri ambarı ile iletişim kurar.  Aracılar yönetim sunucularına bağlanmak için etki alanı kimlik doğrulaması kullanır.  Bu güvenilen bir etki alanının dışında sertifika kimlik doğrulaması gerçekleştirmek veya bağlanılan bir [ağ geçidi sunucusu](https://technet.microsoft.com/library/hh212823.aspx).

SCOM iki SQL veritabanları, işlemsel veri için bir ve Raporlama ile veri analizi desteklemek için başka bir veri ambarı gerektirir.  A [raporlama sunucusu](https://technet.microsoft.com/library/hh298611.aspx) SQL Reporting Services'ı veri ambarından veri raporlamak için çalışır. 

SCOM yönetim paketleri gibi ürünler için kullanarak bulut kaynaklarını izleyebilirsiniz [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708), ve [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html).  Bu yönetim paketleri, performans ve kullanılabilirlik ölçmek için bulan bulut kaynakları ve çalışan iş akışları için proxy olarak bir veya daha fazla yerel aracı kullanın.  Proxy aracıları için kullanılan aynı zamanda [izleme ağ aygıtları](https://technet.microsoft.com/library/hh212935.aspx) ve diğer dış kaynaklara.

İşletim Konsolu yönetim sunucuları birine bağlayan ve görüntülemek ve toplanan verileri çözümlemek ve SCOM ortamını yapılandırmak için yönetici sağlayan bir Windows uygulamasıdır.  Web tabanlı bir konsol, herhangi bir IIS sunucusu üzerinde barındırılan ve tarayıcı üzerinden veri çözümleme sağlar.

![SCOM mimarisi](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Log Analytics
Dağıtma ve en düşük maliyeti ve yönetim çabası ile yönetmek için çoğu OMS Azure bulutunda bileşenleridir.  Günlük analizi tarafından toplanan tüm verileri OMS deposunda saklanır.

Günlük analizi veri üç kaynaktan birinde toplayabilirsiniz:

* Fiziksel ve sanal makineleri Windows çalıştıran ve [Microsoft İzleme Aracısı'nı (MMA)](https://technet.microsoft.com/library/mt484108.aspx) ya da Linux ve [Linux için Operations Management Suite Aracısı](https://technet.microsoft.com/library/mt622052.aspx).  Bu makinelere şirket içi veya Azure veya başka sanal makineler olabilir bulut.
* Bir Azure depolama hesabıyla [Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md) Azure çalışan rolü, web rolü ya da sanal makine tarafından toplanan veriler.
* [SCOM yönetim grubuna bağlantı](https://technet.microsoft.com/library/mt484104.aspx).  Bu yapılandırmada, aracılar verileri nerede sonra OMS veri deposuna teslim SCOM veritabanına teslim SCOM yönetim sunucuları ile iletişim kurar.
  Yöneticiler, toplanan verileri çözümlemek ve Azure üzerinde barındırılan ve herhangi bir tarayıcıdan erişilebilir OMS portalı ile günlük analizi yapılandırın.  Bu verilere erişmek için mobil uygulamalar standart platformlar için kullanılabilir.

![Günlük analizi mimarisi](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>SCOM ve günlük analizi tümleştirme
SCOM için günlük analizi veri kaynağı olarak kullanıldığında, her iki ürün ortamında izleme karma özelliklerini yararlanabilirsiniz.  Varolan SCOM aracıları SCOM'den yönetim paketlerini çalıştırmaya devam yanı sıra OMS tarafından yönetilecek işlemler Konsolu aracılığıyla yapılandırabilirsiniz.  
Bir bağlı SCOM yönetim grubundan veriler için günlük analizi dört yöntemlerden birini kullanarak teslim edilir:

* Olayları ve performans verilerini aracısı tarafından toplanan ve SCOM'ye teslim.  SCOM yönetim sunucuları için günlük analizi sonra verileri teslim eder.
* IIS günlükleri gibi bazı olaylar ve güvenlik olaylarını doğrudan günlük analizi Aracısı'ndan teslim edilecek devam eder.
* Bazı çözümleri aracıya ek yazılımlara veya ek verileri toplamak için yazılım yüklü olmasını gerektirir.  Bu veriler genellikle doğrudan günlük analizi için gönderilir.
* Bazı çözümleri doğrudan Aracıdan kaynaklanmayan SCOM yönetim sunucularından veri toplar.  Örneğin, [uyarı yönetimi çözümü](https://technet.microsoft.com/library/mt484092.aspx) oluşturulduktan sonra SCOM'den uyarılarını toplar.

## <a name="monitoring-logic"></a>İzleme mantığını
SCOM ve günlük analizi aracılardan toplanan benzer verilerle çalışır ancak nasıl tanımlamak ve veri toplama için kendi mantığını uygulamak ve nasıl bunlar bunlar toplamak verileri çözümlemek temel farklılıklar vardır.

### <a name="operations-manager"></a>Operations Manager
SCOM uygulanan için izleme mantığı [yönetim paketleri](https://technet.microsoft.com/library/hh457558.aspx) bu bileşenlerin ve analiz etmek için veri toplamak için sistem durumu ölçme izleyiciye bileşenleri bulma mantığını içerir.  İzleme verilerini bir olay veya performans sayacı toplama olarak kadar basit olabilir veya bir betikte uygulanan karmaşık mantık kullanabilirsiniz.  Tam izleme içeren yönetim paketleri çeşitli için kullanılabilir [Microsoft ve üçüncü taraf uygulamaları](http://go.microsoft.com/fwlink/?LinkId=82105) yanı sıra donanım ve ağ cihazları.  Yapabilecekleriniz [kendi yönetim paketlerinizi Yazar](http://aka.ms/mpauthor) özel uygulamalar için.

Yönetim paketleri, her bir performans sayacı örnekleme, bir hizmetin durumunu denetleme veya betik çalıştırma gibi bazı farklı izleme işlevi yerine getiren birden çok iş akışı içerir.  Her bir iş akışı bağımsız olarak çalışır ve hangi veritabanı gibi için yazılacak kendi sonuçları ve olup bir uyarı üretir tanımlar. 

İş akışı çalıştırdıkları, burada bunlar hata göz önünde bulundurun eşik sıklığı gibi ayrıntılarını ve oluşturdukları uyarının önem derecesini geçersiz kılabilirsiniz.  Ayrıca, kendi iş akışları ekleyerek ek işlevsellik sağlayabilir.

![Geçersiz kılmaları](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Yönetim paketleri Operations Manager veritabanına yüklenir ve aracılara yönetim sunucuları tarafından otomatik olarak dağıtılmış.  Her bir aracının otomatik olarak yönetim paketlerini karşıdan yüklemek ve yüklü uygulamalar için ilgili iş akışları yükleyin.  Aracı tarafından toplanan veriler içine eklenmek SCOM veritabanı ve veri ambarı yönetim sunucusuna geri teslim edilir.  İşlemler konsolunda görüntülemek ve özel görünümler, panolar ve raporlar yönetim paketinde aracılığıyla bu verileri çözümlemek sağlar.

Yönetim paketlerinin dağıtım aşağıdaki çizimde gösterilmiştir.

![Yönetim Paketi akış](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Log Analytics
#### <a name="event-and-performance-collection"></a>Olay ve performans toplama
Günlük analizi, Windows olay günlüğü, IIS günlüklerini ve Syslog gibi kaynakları kullanarak aracı sistemlerden olaylar ve performans sayaçlarını toplar.  Kendisi için veri günlük analizi portalından toplanır ve günlük toplanan verileri çözümlemek için sorgular oluşturun ölçütlerini tanımlayabilirsiniz.  Standart ölçüt OMS çalışma alanınızı oluştururken ve belirli uygulamalar için ek veri tanımlayabilirsiniz tanımlanır. 

![Günlük analizi tanımlayan olay günlükleri](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

SCOM genellikle verileri ve yanıt olarak gerçekleştirilen eylem için belirli ölçütleri tanımlayın birçok ayrıntılı iş akışı sahipken, günlük analizi veri toplama için daha fazla genel ölçüt içeriyor.  Günlük sorgular ve çözümleri analiz etme ve toplanan sonra bulut belirli veriler üzerinde çalışıyor. daha fazla hedeflenen ölçütlerini belirtin.

#### <a name="solutions"></a>Çözümler
Çözümleri, veri toplama ve analiz için ilave bir mantık sağlar.  Çözüm Galeriden OMS aboneliğinize eklemek için çözümler seçebilirsiniz.

![Çözümleri Galerisi](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Olayları ve OMS depoya toplanan performans sayaçlarının analizini sağlayan bulut çözümleri öncelikle çalıştırın.  Bunlar, ayrıca günlük sorgularla veya OMS panosunda çözümü tarafından sağlanan ek kullanıcı arabirimi tarafından analiz ek verilerin toplanmasını tanımlayabilir. 

Örneğin, [değişiklik izleme çözümü](https://technet.microsoft.com/library/mt484099.aspx) yapılandırma aracı sistemlerinde dönüşür ve özetleyen birkaç grafik görünümlerle çözümlenebilir OMS deponuza olayların algılandığı değişiklikleri Yazar algılar.  Çözümü tarafından toplanan ayrıntılı verileri görüntülemek günlük sorguları içine özetlenen görünümünden ayrıntıya girebilirsiniz.

Aboneliğinize eklemek hangi çözümü seçebilirsiniz, ancak şu anda kendi çözümleri oluşturma yeteneği yoktur.  Olaylar ve performans sayaçları toplamak ve kendi günlük sorguları temel alan özel görünüm oluşturmak için seçebilirsiniz.

Günlük analizi için izleme mantığını aşağıdaki şemada özetlenir.

![Günlük analizi çözüm akışı](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Sistem Durumunu İzleme
### <a name="operations-manager"></a>Operations Manager
SCOM, bir uygulama'nin farklı bileşenleri model ve her biri için gerçek zamanlı bir sistem durumu sağlar.  Bu, yalnızca hataları algılandı görüntüleme ve zaman içinde aynı zamanda herhangi bir zamanda gerçek bir uygulama veya sistem durumunu ve her bir bileşen doğrulamak için performans sağlar.  Bir uygulama kullanılabilir dönemleri anladığı olduğundan, SCOM sistem durumu altyapısında hizmet düzeyi sözleşmeleri (hangi çözümleme ve zaman içinde bir uygulamanın kullanılabilirliğini raporlama SLA) da destekler.

Örneğin, aşağıdaki SQL veritabanı motoru tarafından SCOM izlenen gerçek zamanlı durumunu görüntüler.  Her bir veritabanı için bir veritabanı motoru durumunu alta yarısı görünümü gösterilir.

![Durum görünümü](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Veritabanı motoru biri için sistem durumu Gezgini genel sistem durumu belirlemek için kullanılan monitörlerle aşağıda gösterilmiştir.  Bu İzleyici SQL Yönetim paketinde tanımlanan ve SCOM tarafından bulunan tüm SQL veritabanı motoru karşı çalıştırın.

![Sistem durumu Gezgini](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Birden çok sistem üzerinde bileşenleri, dağıtılmış bir uygulamanın durumunu ölçmek için birleştirilebilir.  Bu, birden çok Dağıtılmış Bileşen içeren iş kolu uygulamaları için özellikle yararlı olabilir.  Uygulama için kullanılabilirlik içine, her bileşenin sistem durumu ölçer bir model, toplaması oluşturabilirsiniz.

Active Directory, dağıtılmış bileşenlerinden çözümlemek için bir model sağlayan bir Yönetim Paketi örneğidir.  Aşağıdaki örnek diyagram durumunu genel ortamında ve ormanlar, etki alanları ve etki alanı denetleyicileri arasındaki ilişkiyi gösterir.  Bu bileşenlerin her birini, bileşenleri ve birden çok monitör yukarıdaki SQL örnektekine benzer içerir.

![SCOM diyagram görünümü](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Log Analytics
OMS modeli uygulamaları için ortak bir alt içermez ve gerçek zamanlı durumlarını ölçün.  Tek tek çözümlerinin toplanan verilere dayalı belirli hizmetleri genel durumunu değerlendirmek ve gerçek zamanlı analiz gerçekleştirmek için aracı üzerinde özel mantık yükleyebilir.  Çözümleri erişim ile bulutta OMS depoya çalıştığından, genellikle genellikle yönetim paketleri tarafından gerçekleştirilen daha derin çözümleme sağlayabilir. 

Örneğin, [AD değerlendirmesi ve SQL değerlendirmesi çözümleri](https://technet.microsoft.com/library/mt484102.aspx) toplanan verileri çözümlemek ve farklı yönlerini ortam için bir derecelendirme sağlayın.  Ortamının performansını ve kullanılabilirliğini artırmak için yapılan iyileştirmeler için öneriler içerir.

![AD değerlendirme çözümü](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Veri analizi
SCOM ve günlük analizi toplanan verileri analiz etmek için farklı özellikleri sağlar.  SCOM biçimleri ve Tablo formunda veri ambarından veri sunmak için raporlar çeşitli son veri analizi için işletim konsolunda görünümleri ve panoları sahiptir.  Günlük analizi, OMS deposundaki verileri çözümlemek için bir tam günlük sorgu dili ve arabirim sağlar.  SCOM için günlük analizi veri kaynağı olarak kullanıldığında, depo hem sistemlerden verileri çözümlemek için günlük analizi araçları kullanılabilmesi için SCOM tarafından toplanan verileri içerir.

### <a name="operations-manager"></a>Operations Manager
#### <a name="views"></a>Görünümler
İşlemler konsolunda görünümler, farklı veri türleri SCOM tarafından toplanan olaylar, uyarılar ve durumu verileri, genellikle tablo farklı biçimlerde görüntülemek ve performans verileri için grafikleri satır izin verir.  Görünümler en az çözümlemesi ya da veri birleştirilmesi ancak belirli ölçütlere göre filtre uygulamak izin verir. 

![Görünümler](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Yönetim paketleri genellikle uygulama ya da izlediği sistem destekleyen birden çok görünüm sağlar.  Bu Yönetim Paketi bulur, farklı nesneler algılanan sorunları için uyarı görünümleri ve sayaçları için performans görünümleri için durum görünümleri içerebilir.

Görünümler, özellikle, açık uyarılar dahil olmak üzere ortamı geçerli durumunu ve izlenen sistemleri ve nesnelerin sistem durumunu çözümlemek için uygundur.  Ayrıntılı olay veya performans verilerini kök nedenini tanılamak için belirli bir uyarı destekleme aşağıya doğru inebilir. Benzer şekilde, performans ve geçerli durumunu değerlendirmek için bir uygulama'nin farklı bileşenleri durumunu görüntüleyebilirsiniz.

#### <a name="dashboards"></a>Panolar
Daha özelleştirilebilir ve daha zengin Görselleştirmelerini içerebilir ancak görünümler gibi işlemler konsolunda panolar öncelikle aynı verilerle çalışır.  Bir dizi standart panolar kullanılabilir kendi amaçlarınız doğrultusunda kolayca özelleştirebilirsiniz.  Bir PowerShell sorgudan döndürülen verileri görüntüleyen bir PowerShell pencere öğesini de kullanabilirsiniz.

![Pano](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Geliştiriciler kendi yönetim paketlerine dahil panolar özel bileşenleri eklemek için sahipsiniz.  Bu yüksek oranda aşağıda gösterilen SQL Yönetim Paketi panosunda gibi belirli bir uygulama için özelleştirilmiş.  Bu panoyu özel sürümleri için de bir şablon olarak kullanılabilir.

![SQL Panosu](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Reports
SCOM raporlarda Tablo formunda veri ambarında verileri analiz edin.  Bunlar yazdırılabilir ve PDF, CSV ve Word gibi farklı dosya biçimlerinde otomatik teslim için zamanlanan.  Özellikle uzun vadeli eğilimlerini analiz için uygun şekilde raporlar veri ambarından verilerle çalışır.

Yönetim paketleri genellikle özel raporlar için belirli bir uygulama sağlar.  Kendi uygulamalar için veya geçici analiz gerçekleştirmek için özelleştirebileceğiniz genel rapor kitaplığı seçebilirsiniz.

Active Directory Yönetim Paketi tarafından toplanan verileri gösteren bir örnek performans raporu aşağıdadır.

![Rapor](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Log Analytics
Günlük analizi sahip bir [sorgu dili](https://technet.microsoft.com/library/mt484120.aspx) verileri bir özel görünüm veya rapor oluşturmak zorunda kalmadan birden çok uygulama arasında analiz gerçekleştirmek için kullanabilirsiniz.  OMS bulutta uygulandığı için sorgular ve veri analizi performansını donanım sınırlamalar tabi değildir ve milyonlarca kayıt da dahil, sorguların kolayca çözümleyebilirsiniz. 

Günlük analizi sorgularında ayrıca diğer işlevleri temelidir.  Bir sorgu kaydetmek, sonuçları Excel'e veya sahip otomatik olarak düzenli aralıklarla çalıştırmak ve sonuçları belirli ölçütlere uyan varsa uyarı oluştur.  

![Günlük sorgu akışı](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Günlük analizi sorgu örneği aşağıdadır.  Bu örnekte "adı kullanmaya" ile tüm olayları döndürülen ve olay tarafından gruplandırılmış kimliği  Kullanıcı yalnızca sorgu sağlar ve günlük analizi analiz gerçekleştirmek için kullanıcı arabirimi dinamik olarak oluşturur.  Herhangi bir öğeyi listeden seçim ayrıntılı olay verileri döndürür.

![Günlük sorgu](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Geçici analiz sağlamaya ek olarak, günlük analizi sorgularda gelecekte kullanılmak üzere kaydedilebilir ve aynı zamanda eklenen, [OMS Pano](http://technet.microsoft.com/library/mt484090.aspx) aşağıdaki örnekte gösterildiği gibi.

![OMS Panosu](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Sonraki Adımlar
* Dağıtma [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx).
* Kaydolun [günlük analizi](https://azure.microsoft.com/documentation/services/log-analytics).  

