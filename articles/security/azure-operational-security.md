---
title: "Azure işlem güvenliği | Microsoft Docs"
description: "Microsoft Operations Management Suite (OMS), hizmetlerinin ve nasıl çalıştığı hakkında bilgi edinin."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: ec9e0fc7d67537a47d5c0d3bb376b60dc6ccffcd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="azure-operational-security"></a>Azure işlem güvenliği
## <a name="introduction"></a>Giriş

### <a name="overview"></a>Genel Bakış
Güvenlik bulutta iş ve nasıl önemli Azure güvenliği hakkında doğru ve güncel bilgi bulma olduğunu olduğunu biliyoruz. Çeşitli güvenlik araçları ve yetenekler yararlanmak için uygulamalar ve hizmetler için Azure kullanmak için en iyi nedenlerinden biridir. Bu araçları ve yetenekleri güvenli Azure platformunda güvenli çözümler oluşturmak mümkün kılar yardımcı olur. Windows Azure gizlilik, bütünlük ve müşteri verilerini, kullanılabilirliğini, ayrıca saydam sorumluluk etkinleştirirken sağlamanız gerekir.

Güvenlik denetimleri dizisi Microsoft Azure içinde her iki müşterinin uygulanan ve Microsoft işlemsel bakış açılarını, "Azure işletimsel güvenlik", bu teknik incelemede yazılmış, daha iyi anlamak müşterilere yardımcı olmak için kapsamlı bir sağlar. Windows Azure ile kullanılabilir işletimsel güvenlik bakın.

### <a name="azure-platform"></a>Azure platformu
Azure işletim sistemleri, programlama dilleri, çerçeveleri, Araçlar, veritabanları ve aygıtları geniş çapta destekleyen bir genel bulut hizmeti platformudur. Docker Tümleştirmesi ile Linux kapsayıcıları çalıştırabilirsiniz; JavaScript, Python, .NET, PHP, Java ve Node.js ile uygulamalar oluşturma; Yapı geri-iOS, Android ve Windows cihazları sona erer. Azure bulut hizmeti aynı teknolojileri geliştiriciler milyonlarca destekler ve BT uzmanları zaten kullanır ve güven.

Oluşturmanıza veya BT varlıklarına geçirmek, uygulamalar ve hizmetler ve denetimleri ile verileri korumak için bir kuruluşun yeteneklerini güvenmek bir genel bulut hizmeti sağlayıcısı bulut tabanlı varlıklarınızı güvenliği yönetmek için sağlarlar.

Azure'nın altyapı tesis aynı anda milyonlarca müşteri barındırmak için uygulamalar için tasarlanmıştır ve güvenlik gereksinimlerine bağlı işletmeler karşılayabilecek güvenilir bir temel sağlar. Ayrıca, Azure ile geniş bir yelpazesini yapılandırılabilir güvenlik seçenekleri ve kuruluşunuzun dağıtımları benzersiz gereksinimlerini karşılamak için güvenlik özelleştirebilirsiniz böylece bunları denetleme olanağı sağlar. Bu belge, nasıl Azure güvenliği anlamanıza yardımcı özellikler, bu gereksinimleri karşılamak yardımcı.

### <a name="abstract"></a>Özet
Azure işletimsel güvenlik hizmetleri, denetimleri ve kullanıcılar için kullanılabilir özellikler verilerini, uygulamaları ve diğer Microsoft Azure varlıkları korumak için ifade eder. Azure işlem güvenliği Microsoft Security Development Lifecycle (SDL), Microsoft Security Response Center programı da dahil olmak üzere Microsoft'a özgü çeşitli özellikleri aracılığıyla elde edilen bilgilerden içerir çerçevesi üzerine inşa edilmiştir, ve siber güvenlik tehdit derin farkındalığınızı.

Bu teknik incelemede Azure işletimsel güvenlik Microsoft'un yaklaşımı Microsoft Azure bulut platformu içinde özetler ve hizmetleri içerir:
1.  [Azure Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

2.  [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)

3.  [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

4.  [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

5.  [Azure depolama çözümlemeleri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Azure Active directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)


## <a name="microsoft-operations-management-suite"></a>Microsoft Operations Management Suite

Microsoft Operations Management Suite (OMS) karma bulut BT yönetimi çözümüdür. Tek başına kullanıldığında veya var olan System Center dağıtımınızı genişletmek için OMS, en fazla esneklik ve denetim için bulut tabanlı yönetim altyapınızın sunar.

![Microsoft Operations Management Suite](./media/azure-operational-security/azure-operational-security-fig1.png)

OMS ile şirket içi, Azure, AWS, Windows Server, Linux, VMware ve OpenStack, rekabet çözümleri daha düşük bir maliyetle de dahil olmak üzere tüm bulut örnek yönetebilirsiniz. Bulut ilk dünya için yerleşik, OMS kuruluşunuzu yeni iş zorlayan ve yeni iş yükleri, uygulamaları ve bulut ortamlarını uyum sağlamak için diğer bir deyişle hızlı ve en düşük maliyetli şekilde yönetme için yeni bir yaklaşım sunmaktadır.

### <a name="oms-services"></a>OMS hizmetleri

OMS’nin temel işlevleri Azure’da çalışan bir dizi hizmet tarafından sağlanır. Her hizmet belirli bir yönetim işlevi sağlar ve farklı hizmetleri birleştirerek farklı yönetim senaryoları elde edebilirsiniz.

| Hizmet  | Açıklama|
| :------------- | :-------------|
| Log Analytics | Fiziksel ve sanal makineler dahil olmak üzere çeşitli kaynakların kullanılabilirliğini ve performansını izleyin ve analiz edin. |
|Otomasyon | El ile gerçekleştirilen işlemleri otomatikleştirin; fiziksel ve sanal makinelere yapılandırma uygulayın. |
| Backup | Yedekleme ve geri yükleme kritik verileri. |
| Site Recovery | Kritik uygulamalar için yüksek kullanılabilirlik sağlayın. |

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics), yönetilen kaynaklardan toplanan verileri merkezi bir depoda birleştirerek OMS için izleme hizmetleri sağlar. Bu verilere olaylar, performans verileri ya da API aracılığıyla sağlanan özel veriler dahil olabilir. Toplanan veriler uyarı, analiz ve dışarı aktarma için kullanılabilir hale gelir.


Bu yöntem, çeşitli kaynaklardan veri birleştirmesine olanak tanır, araya getirebilirsiniz şekilde varolan ile Azure hizmetlerinizi verilerden ortamı şirket. Ayrıca, veri toplama işlemini veriler üzerinde gerçekleştirilen eylemden ayırarak tüm eylemlerin her tür veri üzerinde kullanılabilmesini mümkün kılar.


![Log Analytics](./media/azure-operational-security/azure-operational-security-fig2.png)

Günlük analizi hizmeti aşağıdaki yöntemleri kullanarak bulut tabanlı verilerinizi güvenli bir şekilde yönetir:
-   veriler arasında ayrım yapma
-   veri saklama
-   Fiziksel güvenlik
-   Olay yönetimi
-   Uyumluluk
-   güvenlik standartları sertifikaları

### <a name="azure-backup"></a>Azure Backup

[Azure yedekleme](http://azure.microsoft.com/documentation/services/backup) veri yedekleme ve Hizmetleri geri yükleme ve ürün ve hizmetlerini OMS paketinin bir parçası olan sağlar.
Uygulama verilerinizi korur ve herhangi bir sermaye yatırımı olmadan en düşük işletim giderleriyle yıllar boyunca saklar. SQL Server ve SharePoint gibi uygulama iş yüklerinin yanı sıra fiziksel ve sanal Windows Server verilerini yedekleyebilirsiniz. Tarafından da kullanılabilir [System Center Data Protection Manager (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) artıklık ve uzun vadeli depolama için Azure için korunan veri çoğaltmak için.


Azure Backup'ta korunan veriler belirli bir coğrafi bölgede yer alan bir yedekleme kasasında depolanır. Veriler aynı bölge içinde çoğaltılır ve kasa türüne bağlı olarak, aynı zamanda daha fazla esneklik için başka bir bölgeye çoğaltılmış.

### <a name="management-solutions"></a>Yönetim Çözümleri
[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) , yönetmek ve şirket içi korumak ve bulut altyapısı yardımcı olan Microsoft'un bulut tabanlı BT yönetimi çözümüdür.


[Yönetim çözümleri](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) bir veya daha fazla OMS hizmetlerini kullanarak belirli bir yönetim senaryoları uygulayacak logics paketlenmiş kümeleridir. Microsoft ve iş ortakları tarafından sağlanan ve OMS yatırımınızın değerini artırmak için Azure aboneliğinize kolayca ekleyebileceğiniz farklı çözümler vardır. Bir iş ortağı olarak, uygulama ve hizmetlerinize desteklemek ve bunları Azure Marketi veya hızlı başlangıç şablonlarından aracılığıyla kullanıcılara sağlamak üzere kendi çözümleri oluşturabilirsiniz.


![Yönetim Çözümleri](./media/azure-operational-security/azure-operational-security-fig4.png)

Ek işlevsellik sağlamak için birden çok hizmet kullanan bir çözüm, iyi bir örnektir [güncelleştirme yönetimi çözümü](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management). Bu çözümü kullanan [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) Aracısı hakkında bilgi toplamak, Windows ve Linux için gerekli güncelleştirmeleri her aracı. Bu veriler, dahil olan bir panoyla analiz edilebilecekleri Log Analytics deposuna yazılır.

Runbook'ları bir dağıtım oluşturduğunuzda [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) gerekli güncelleştirmeleri yüklemek için kullanılır. Bu işlemi baştan sona portalda yönetirsiniz ve altyapısal ayrıntılar konusunda endişelenmeniz gerekmez.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

Azure Güvenlik Merkezi, Azure kaynaklarınızı korumanıza yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi, Azure sağlar. Tanımlayabilir hizmet içinde yalnızca, Azure aboneliklerinize karşı aynı zamanda karşı ilkeleri [kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups), daha ayrıntılı olabilir.

### <a name="security-policies-and-recommendations"></a>Güvenlik ilkeleri ve öneriler

Güvenlik ilkeleri, belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetim kümesini tanımlar.

Güvenlik Merkezi'nde, şirketinizin güvenlik gereksinimleri ve uygulamaların türü veya verilerin duyarlılığına göre ilkeleri tanımlarsınız.

![Güvenlik ilkeleri ve öneriler](./media/azure-operational-security/azure-operational-security-fig5.png)


Abonelik düzeyinde otomatik olarak etkinleştirilen ilkeler sağ tarafındaki diyagramda gösterildiği gibi abonelik içindeki tüm kaynak gruplarına yayılması:


### <a name="data-collection"></a>Veri toplama

Güvenlik Merkezi, verilerinizin güvenlik durumunu değerlendirmek, güvenlik önerileri sağlamak ve sizi tehditlere karşı uyarmak için sanal makinelerinizden (VM) veri toplar. Tüm vm'lerde aboneliğinizde ilk erişiminizi Güvenlik Merkezi, veri koleksiyonu etkin olmadığında. Veri toplama önerilir, ancak Güvenlik Merkezi ilkesinde veri toplamayı kapatarak bu seçimi kaldırabilirsiniz.

### <a name="data-sources"></a>Veri kaynakları

- Azure Güvenlik Merkezi, güvenlik durumunuzu görüntüleme, güvenlik açıklarını tanımlayıp çözümler önerme ve etkin tehditleri saptama amacıyla aşağıdaki kaynaklarda bulunan verileri analiz eder:

-   Azure Hizmetleri: Dağıttığınız Azure hizmetlerinin yapılandırmasına ilişkin bilgileri, ilgili hizmetin kaynak sağlayıcısıyla iletişim kurarak kullanır.

- Ağ Trafiği: Microsoft’un altyapısından kaynak/hedef IP/bağlantı noktası, paket boyutu ve ağ protokolü gibi örneği alınmış ağ trafiği meta verilerini kullanır.

-   İş Ortağı Çözümleri: Güvenlik duvarları ve kötü amaçlı yazılımdan koruma çözümleri gibi tümleşik iş ortağı çözümlerinden güvenlik uyarılarını kullanır.

-   Sanal Makineleriniz: Sanal makinelerinizdeki yapılandırma bilgilerini ve Windows olay ve denetim günlükleri, IIS günlükleri, syslog iletileri ve kilitlenme dökümü dosyaları gibi güvenlik olaylarına ilişkin bilgileri kullanır.

### <a name="data-protection"></a>Veri koruma

Müşterilerin tehditleri önlemesine, algılamasına ve yanıt vermesine yardımcı olmak amacıyla Azure Güvenlik Merkezi, güvenlikle ilgili veriler, meta veriler, olay günlükleri, kilitlenme dökümü dosyaları ve diğer verileri toplar ve işler. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır.

-   **Veri ayırma**: Veriler hizmet boyunca her bir bileşende mantıksal olarak ayrı tutulur. Tüm veriler kuruluşa göre etiketlenir. Bu etiketleme, veri yaşam döngüsü boyunca devam eder ve her bir hizmet katmanında uygulanır.

-   **Veri erişimi**: güvenlik önerileri sağlamak ve olası güvenlik tehditlerini araştırmak üzere Microsoft personeli erişebilir toplanan bilgiler ve işlem oluşturma olayları, VM disk kilitlenme döküm dosyaları da dahil olmak üzere Azure Hizmetleri tarafından çözümlenen Anlık görüntüler ve yapıları kasıtsız olarak müşteri verilerini veya sanal makinelerinizi kişisel verileri içerebilir. Size uygun [Microsoft çevrimiçi hizmet koşulları ve gizlilik bildirimini](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), Microsoft olmayan hangi durum müşteri verilerini kullanır veya reklam ya da benzeri ticari amaçlarla için bu bilgileri türetilir.

-   **Veri kullanımı**: Microsoft önleme ve algılama özelliklerimizi geliştirmek amacıyla birden fazla kiracıda görülen modelleri ve tehdit bilgilerini kullanır; bunu [Gizlilik Bildirimi](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx) belgemizde açıklanan gizlilik taahhütlerine uygun şekilde yaparız.

### <a name="data-location"></a>Veri konumu

Azure Güvenlik Merkezi, kilitlenme döküm dosyalarının kısa ömürlü kopyalarını toplar ve açıktan yararlanma girişimlerinin ve başarılı uzlaşmaların kanıtı olarak analiz eder. Azure Güvenlik Merkezi bu analizi çalışma alanıyla aynı coğrafi bölgede gerçekleştirir ve analiz tamamlandığında kısa ömürlü kopyaları siler. Makine yapıları VM ile aynı bölgede merkezi olarak depolanır.

-   **Depolama hesaplarınızı**: sanal makineleri çalıştırdığı her bölge için bir depolama hesabı belirtilir. Bunun yapılması verileri, verilerin toplandığı sanal makine ile aynı bölgede depolamanızı sağlar.

-   **Azure Güvenlik Merkezi Depolama**: İş ortağı uyarıları, öneriler ve güvenlik durumu gibi güvenlik uyarıları hakkında bilgiler, şu anda Birleşik Devletler’de bulunan merkezde depolanmaktadır. Bu bilgiler size güvenlik uyarısı, öneri veya sistem durumu sağlamak üzere gerektiğinde sanal makinelerinizden toplanan ilgili yapılandırma bilgilerini ve güvenlik olaylarını içerebilir.


## <a name="azure-monitor"></a>Azure İzleyici

[OMS güvenlik](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) ve etkin olarak güvenlik olayların etkisini en aza yardımcı olabilecek tüm kaynakları izlemek için BT'nin çözümü sağlar. OMS güvenlik ve denetim kaynakları izlemek için kullanılan güvenlik etki alanları sahiptir. Güvenlik etki alanı seçenekleri hızlı erişim sağlar, güvenlik izleme için şu etki alanlarına daha ayrıntılı olarak ele alınmıştır:

-   kötü amaçlı yazılım değerlendirmesi
-   Güncelleştirme değerlendirmesi
-   Kimlik ve erişim.

Azure İzleyicisi kaynakları belirli türde bilgileri işaretçiler sağlar. Görselleştirme, sorgu, yönlendirme, uyarı, otomatik ölçeklendirme ve Otomasyon verileri hem Azure altyapısı (Etkinlik günlüğü) ve tek tek her Azure kaynak (tanılama günlükleri) sunar.

![Azure İzleyici](./media/azure-operational-security/azure-operational-security-fig6.png)


Bulut uygulamalarını birçok taşıma bölümleriyle karmaşıktır. İzleme, uygulamanızı kurma kalmasını sağlamak için veri ve sağlıklı bir durumda çalışmasını sağlar. Ayrıca olası sorunları stave veya olanları sorun gidermeye yardımcı olur.

Ayrıca, uygulamanız hakkında ayrıntılı Öngörüler elde etmek için izleme verilerini kullanabilirsiniz. Bu bilgi, uygulama performansı veya devamlılığını iyileştirmek için yardımcı veya aksi halde el ile müdahale gerektiren Eylemler otomatikleştirmek.

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü


Aboneliğinizi kaynaklarında gerçekleştirilen işlemler hakkında bilgi sağlayan bir günlüktür. Denetim düzlemi olayları aboneliklerinizi için raporları beri etkinlik günlüğü daha önce "Denetim günlüklerini" veya "İşlem günlükleri," olarak bilinir.

![Azure etkinlik günlüğü](./media/azure-operational-security/azure-operational-security-fig7.png)

Etkinlik günlüğü kullanarak, belirleyebilirsiniz ' ne, kimin, ne zaman ve ' herhangi yazma işlemleri (PUT, POST, DELETE) aboneliğinizi kaynaklarında alınan için. İşleminin durumunu ve ilgili diğer özellikleri de anlayabilirsiniz. Etkinlik günlüğü okuma (GET) işlemleri veya Klasik modeli kullanan kaynakları işlemlerinde içermez.

### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

Bu günlükler bir kaynak tarafından gösterilen ve bu kaynağın işlemi hakkında zengin, sık veriler sağlar. Bu günlükler içeriğini kaynak türüne göre değişir.

Örneğin, Windows olayı sistem günlükleri tanılama günlüğünün VM'ler ve blob, tablo için bir kategori, ve sıra günlükleri tanılama günlüklerini kategorileri depolama hesapları için.

Tanılama günlüklerini farklı [etkinlik günlüğü (önceki adıyla denetim günlüğü veya işlem günlüğü olarak bilinir)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Etkinlik günlüğü aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri işlemleri kaynağınız kendisini gerçekleştirilen bir anlayış sağlar.

### <a name="metrics"></a>Ölçümler

Azure İzleyicisi, performans ve sistem durumu, iş yüklerinin Azure üzerinde görünürlük elde etmek için telemetri kullanmasına olanak sağlar. En önemli Azure telemetri verileri çoğu Azure kaynaklar tarafından gösterilen (performans sayaçlarını olarak da bilinir) ölçümleri türüdür. Azure İzleyicisi, yapılandırmak ve bunlar kullanmak için çeşitli yollar sağlar [ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) izleme ve sorun giderme için. Ölçümleri telemetri değerli bir kaynaktır ve aşağıdaki görevleri yapmanıza olanak sağlar:

-   **Performans İzleme** kendi ölçümleri portal grafik Çizdirmek ve bu grafik bir Pano için sabitleme kaynağın (örneğin, bir VM, Web sitesi ya da mantıksal uygulama).

-   **Bir sorun bildirim alma** , etkiler, kaynak performans ölçüm belirli bir eşiği kestiği olduğunda.

-   **Otomatik eylemler yapılandırma**, bir kaynak ölçekleme veya belirli bir eşiği ölçüm kestiği olduğunda bir runbook tetikleme otomatik gibi.

-   **Gelişmiş analizler gerçekleştirmek** veya kaynağınız performans ya da kullanım eğilimlerini üzerinde raporlama.

-   **Arşiv** uyumluluk veya denetim amacıyla bir kaynak performans veya sistem durumu geçmişini.

### <a name="azure-diagnostics"></a>Azure Tanılama

Dağıtılan bir uygulama tanılama verilerini toplama sağlar. Azure içinde bir özelliktir. Çeşitli farklı kaynaklardan tanılama uzantısını kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti Web ve çalışan rolleri](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/windows/overview) Microsoft Windows çalıştıran ve [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics). Diğer Azure hizmetleriyle kendi ayrı tanılama vardır.

## <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi

Ağınızda güvenlik denetimi, ağ güvenlik açıklarını algılama ve BT güvenliği ve Mevzuat idare modeli ile uyumluluğu sağlamak için önemlidir. Güvenlik grubu görünümü ile yapılandırılmış ağ güvenlik grubu ve güvenlik kuralları ve etkili güvenlik kuralları alabilir. Uygulanacak kurallar listesi ile açık olan ve ağ güvenlik açığı değerlendirmek bağlantı noktaları belirleyebilirsiniz.

[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) izleme ve bir ağ düzeyinde içinde için ve azure'dan koşullar tanılama sağlayan bölgesel bir hizmettir. Ağ Tanılama ve görselleştirme Ağ İzleyicisi ile kullanılabilen araçlar anlamak, tanılama ve Azure ağınızdaki serisidir yardımcı olur. Bu hizmet içeren paket yakalama, sonraki atlama IP akış, güvenlik grubu görünümü, NSG akış günlükleri doğrulayın. Senaryo düzeyi izleme kaynak tek tek ağ izleme aksine ağ kaynaklarına bir uçtan uca görünümünü sağlar.

![Azure Ağ İzleyicisi](./media/azure-operational-security/azure-operational-security-fig8.png)

Ağ İzleyicisi'ni şu anda aşağıdaki özellikleri içerir:

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">Denetim günlükleri</a>**-ağların yapılandırmasının bir parçası gerçekleştirilen işlemleri günlüğe kaydedilir. Bu günlükler Azure Portalı'nda görüntülenebilir veya Power BI gibi Microsoft araçları veya üçüncü taraf araçlarını kullanarak alınamıyor. Denetim günlükleri, portal, PowerShell'i, CLI ve Rest API kullanılabilir. Denetim günlükleri hakkında daha fazla bilgi için denetim işlemleri Resource Manager ile bakın. Denetim günlükleri, tüm ağ kaynaklarına yapılan işlemleri için kullanılabilir.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">IP akış doğrular </a>**  -paket izin verilen veya reddedilen denetimleri temel akış bilgi 5-tanımlama grubu paket parametrelerine (hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol). Paket bir ağ güvenlik grubu tarafından reddedilirse kural ve Paket reddedildi ağ güvenlik grubu döndürülür.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Sonraki atlama</a>**  -kullanıcı tanımlı yollar herhangi tanılamak olanak sağlayarak Azure ağ yapıda yönlendirilen paketleri yanlış için sonraki atlama belirler.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Güvenlik grubu görünümü</a>**  -bir VM üzerinde uygulanan etkili ve uygulanan güvenlik kuralları alır.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">NSG akış günlük</a>**  -akış günlükleri ağ güvenlik grupları için izin verilen ya da grubu güvenlik kuralları tarafından reddedilen trafiği ilgili günlükleri yakalamanıza olanak sağlar. Akış bir 5-tanımlama grubu bilgileriyle – kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol tanımlanır.

## <a name="azure-storage-analytics"></a>Azure Depolama Analizi

[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) toplu işlem istatistiklerini ve kapasite verilerini istekleriyle ilgili bir depolama birimi hizmeti dahil ölçümleri depolayabilirsiniz. İşlemler düzeyde her iki API işlem ve depolama hizmeti düzeyinde bildirilir ve kapasite depolama hizmet düzeyinde bildirilir. Ölçüm verilerini depolama hizmeti kullanım çözümlemek, depolama birimi hizmeti karşı yapılan istekleri ile sorunlarını tanılamak ve bir hizmeti kullanan uygulamaların performansını artırmak için kullanılabilir.

[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler veriler için bir depolama hesabı sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz. Storage Analytics günlüğe kaydetme için kullanılabilir [Blob, kuyruk ve tablo hizmetlerine](https://docs.microsoft.com/azure/storage/storage-introduction). Storage Analytics bir depolama hizmetine başarılı ve başarısız istekler hakkında ayrıntılı bilgi günlüğe kaydeder.

Bu bilgiler, istekleri ayrı ayrı izlemek ve depolama hizmeti ile ilgili sorunları tanılamak için kullanılabilir. İstekleri en iyi çaba ilkesine göre günlüğe kaydedilir. Hizmet uç noktası karşı yapılan istekleri varsa günlük girişleri oluşturulur. Örnek bir depolama hesabı Blob uç ancak kendi tablo veya kuyruğu uç noktaları etkinlik varsa yalnızca günlükleri için Blob hizmetiyle ilişkili oluşturulur.

Storage Analytics kullanmak için onu ayrı ayrı izlemek istediğiniz her hizmet için etkinleştirmelisiniz. İçinde etkinleştirebilirsiniz [Azure portal](https://portal.azure.com/); Ayrıntılar için bkz: [Azure portalında bir depolama hesabını izleme](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Storage Analytics REST API veya istemci kitaplığı yoluyla programlı olarak etkinleştirebilirsiniz. Storage Analytics her hizmet için ayrı ayrı etkinleştirmek için hizmet özelliklerini ayarlama işlemi kullanın.

Toplanan veriler (günlük için) iyi bilinen bir blob ve Blob hizmeti ve tablo hizmeti API'leri kullanılarak erişilebilecek (için ölçümleri), iyi bilinen tablolara depolanır.

Storage Analytics bir 20-TB depolama hesabınız için toplam sınırı bağımsızdır depolanan veri miktarına sahiptir. Tüm günlükler depolanmış [blok blobları](https://docs.microsoft.com/azure/storage/storage-analytics) $logs adlı bir kapsayıcıda, otomatik olarak oluşturulan depolama çözümlemeleri için bir depolama hesabı etkin olduğunda.

Storage Analytics tarafından gerçekleştirilen aşağıdaki eylemler Faturalanabilir şunlardır:

-   İstekleri günlüğe kaydetme için BLOB'ları oluşturmak için
-   Ölçümler için tablo varlıkları oluşturmak için istek sayısı.

> [!Note]
> Faturalama ve veri bekletme ilkeleri hakkında daha fazla bilgi için bkz: [depolama çözümlemeleri ve faturalama](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> En iyi performans için mümkün azaltma önlemek için sanal makineye bağlı yüksek oranda kullanılan disk sayısını sınırlamak istiyorsunuz. Tüm diskleri aynı anda yüksek oranda göstermesi değil, depolama hesabı daha büyük bir sayı disk destekleyebilir.

> [!Note]
> Depolama hesabı sınırları hakkında daha fazla bilgi için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/storage-scalability-targets).


Kimliği doğrulanmış ve anonim istek aşağıdaki türlerini günlüğe kaydedilir.

| Kimlik doğrulaması  | Anonim|
| :------------- | :-------------|
| Başarılı istekler | Başarılı istekler |
|İstek zaman aşımı, azaltma, ağ, yetkilendirme ve başka hatalar da dahil olmak üzere, başarısız oldu | Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istekleri |
| Başarılı ve başarısız istekleri dahil olmak üzere paylaşılan erişim imzası (SAS), kullanarak istekleri |İstemci ve sunucu zaman aşımı hataları |
|   Analiz verileri istekleri |    304 (değişiklik) hata koduyla başarısız olan GET istekleri |
| Storage Analytics kendisini günlük oluşturma veya silme gibi tarafından yapılan istekleri günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir liste belgelenen [depolama Analytics günlüğe yazılan işlemler ve durum iletileri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analytics günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) Konular. | Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam bir liste belgelenen [depolama Analytics günlüğe yazılan işlemler ve durum iletileri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analytics günlük biçimi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |
## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD kimlik yönetimi özelliklerini çok faktörlü kimlik doğrulaması, cihaz kaydı, Self Servis parola yönetimi, Self Servis Grup Yönetimi, ayrıcalıklı hesap yönetimi, rol tabanlı erişim dahil olmak üzere, tam bir paketi de içerir. Denetim, uygulama kullanımını izleme, zengin denetim ve güvenlik izleme ve uyarı.

-   Azure AD çok faktörlü kimlik doğrulama ve koşullu erişim ile uygulama güvenliği artırır.

-   Uygulama kullanımını izlemek ve işletmenizi raporlama ve izleme güvenliğiyle Gelişmiş tehditlere karşı koruyun.

Azure Active Directory (Azure AD), dizininize yönelik güvenlik, etkinlik ve denetim raporlarını içerir. [Azure Active Directory denetim raporu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) , müşterilerin kendi Azure Active Directory'de oluştu ayrıcalıklı Eylemler tanımlamak için yardımcı olur. Ayrıcalıklı Eylemler ayrıcalık değişiklikler (örneğin, rolü oluşturma veya parola sıfırlama), değişen ilke yapılandırmaları (örneğin, parola ilkelerinin) veya dizin yapılandırması (örneğin, etki alanı Federasyon ayarlarında yapılan değişiklikler) değişiklikleri içerir.

Raporlar, olay adı için değişiklik ve tarih ve saat (UTC) etkilenen hedef kaynak eylemi gerçekleştiren aktör denetim kaydını sağlar. Müşteriler kendi Azure Active Directory için denetim olayları listesini almak için [Azure portal](https://portal.azure.com/)açıklandığı gibi [denetim günlüklerini görüntülemek](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Kapsama dahil olan raporların listesi şu şekildedir:

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
| |Parola sıfırlama etkinliği |   | |



Bu raporlar veri SIEM sistemleri, Denetim ve iş zekası araçları gibi uygulamalarınıza yararlı olabilir. Azure AD raporlama [API'leri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) bir dizi API REST tabanlı verilerine programlı erişim sağlar. Bu API'leri çeşitli programlama dilleri ve Araçlar menüsünden çağırabilirsiniz.

Azure AD Denetim Raporu olayları 180 gün boyunca saklanır.

> [!Note]
> Raporlarda bekletme hakkında daha fazla bilgi için bkz: [Azure Active Directory rapor bekletme ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Müşteriler depolanırken ilgilenen kendi [olaylarını denetleme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) daha uzun bekletme dönemleri raporlama API'si düzenli olarak denetim olayları ayrı veri deposuna çıkarmak için kullanılabilir.

## <a name="summary"></a>Özet

Bu makale özetleri gizliliğinizi korumaya ve yazılım ve yardımcı hizmetleri sunarken, verilerinizi güvenli hale getirme, kuruluşunuzun BT altyapısı yönetin. Microsoft, başkalarının verilerini güvenilen bu güven sıkı güvenlik gerektiren tanır. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır. Güvenli hale getirme ve verileri koruma Microsoft'taki önceliğe sahiptir.

Bu makalede açıklanır

-   Nasıl veri toplanan, işlenen ve Operations Management Suite (OMS) güvenli.

-   Birden çok veri kaynağında olayları hızla çözümleyin. Güvenlik ihlalinden doğabilecek zararları azaltmak için güvenlik risklerini tanımlayın, tehdit ve saldırıların kapsamını ve etkisini anlayın.

-   Giden kötü amaçlı IP trafiğini ve kötü amaçlı tehdit türlerini görselleştirerek saldırı desenlerini tanımlayın. Güvenlik tutumunu platformdan bağımsız olarak tüm ortamınızın anlayın.

-   Güvenlik ve uyumluluk denetimi için gerekli tüm günlük ve olay verilerini yakalama. Güvenlik denetimi tamamlandı, aranabilir ve dışarı aktarılabilir günlüğü ve olay veri kümesi ile sağlamak için gerekli kaynakları ve saat bölme.

<ul>
<li>Güvenlikle ilgili olayları, Denetim ve varlıklarınızı yakından tutmak için ihlali analiz Topla:</li>
<ul>
<li>Güvenlik yaklaşımı</li>
<li>Önemli sorun</li>
<li>Özetleri tehditleri</li>
</ul>
</ul>

## <a name="next-steps"></a>Sonraki Adımlar

- [Tasarım ve işlem güvenliği](https://www.microsoft.com/trustcenter/security/designopsecurity)

Microsoft ile göz önünde bulundurularak, bulut altyapısı esnek ve saldırılara karşı korunuyor olmasına yardımcı olmak için Hizmetler ve yazılımları tasarlar.

- [Operations Management Suite | Güvenlik ve uyumluluk](https://www.microsoft.com/cloud-platform/security-and-compliance)

Microsoft güvenlik veri ve analiz daha akıllı ve etkili tehdit algılama gerçekleştirmek için kullanın.

- [Azure Güvenlik Merkezi planlama ve işlemler](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide) adımları ve kuruluşunuzun güvenlik gereksinimlerine ve bulut Yönetimi modeline göre Güvenlik Merkezi kullanımınızı iyileştirmek için izleyebileceğiniz bir dizi.

