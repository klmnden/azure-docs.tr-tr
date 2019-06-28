---
title: Azure sanal makinelerinizin durumunu anlama | Microsoft Docs
description: Bu makalede, sanal makineler için sanal makineler ve Azure İzleyici ile temel işletim sistemleri durumunu anlamak açıklar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/22/2019
ms.author: magoedte
ms.openlocfilehash: 2bf891f8cfecbb9e78e511dcee7ed1c61c170016
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67340138"
---
# <a name="understand-the-health-of-your-azure-virtual-machines"></a>Azure sanal makinelerinizin durumunu anlama

Azure İzleme alanı hizmetler belirli rolleri veya görevleri içerir, ancak işletim sistemlerinin (OSs) Azure sanal makinelerinde (VM'ler) barındırılan ayrıntılı sistem durumu Perspektifler sağlamaz. Farklı koşullar için Azure İzleyici kullanmanız mümkün olmakla birlikte, model ve sistem durumunu temel bileşenler veya Vm'leri genel durumunu göstermek için tasarlanmamıştır.

VM sistem durumu için Azure İzleyicisi'ni kullanarak bir Windows veya Linux konuk işletim sistemi performansını ve kullanılabilirliğini etkin bir şekilde izleyebilirsiniz. Sistem durumu özelliği bileşen sistem durumu ölçmek nasıl belirtir ve iyi durumda olmayan bir koşulu algıladığında bir uyarı gönderir anahtar bileşenler ve aralarındaki ilişkiler temsil eder, ölçütleri sağlar bir model kullanır.

Azure VM'deki temel işletim sistemi ve genel sistem durumu görüntüleme gözlemlenen iki perspektiften birini: doğrudan bir VM veya Azure İzleyici'den bir kaynak grubundaki tüm sanal makineler arasında.

Bu makalede, hızlı bir şekilde değerlendirmenize, araştırmak ve Vm'leri sistem durumu özelliği için Azure İzleyici tarafından algılanan sistem durumu sorunlarını gidermek gösterilmektedir.

VM'ler için Azure İzleyici yapılandırma hakkında daha fazla bilgi için bkz: [VM'ler için Azure İzleyici'ı etkinleştirme](vminsights-enable-overview.md).

## <a name="monitoring-configuration-details"></a>İzleme Yapılandırma Ayrıntıları

Bu bölümde, Azure Windows ve Linux Vm'leri izlemek için varsayılan durumu ölçütlerini özetlenmektedir. Tüm sistem durumu ölçütlerini bunlar düzgün çalışmayan bir durumu birisini algıladığında bir uyarı göndermek için önceden yapılandırılmış.

### <a name="windows-vms"></a>Windows VM'leri

- Kullanılabilir megabayt belleği
- Ortalama Disk yazma (Mantıksal Disk) saniyede
- Ortalama Disk yazma (Disk) saniyede
- Okuma başına ortalama Mantıksal Disk saniye
- Aktarım başına ortalama Mantıksal Disk saniye
- Okuma başına ortalama Disk saniye
- Aktarım başına ortalama Disk saniye
- Geçerli Disk Sırası Uzunluğu (Mantıksal Disk)
- Geçerli Disk Sırası Uzunluğu (Disk)
- Disk yüzde boşta kalma süresi
- Dosya sistemi hatası veya Bozulması
- Mantıksal Disk boş alan (%) Düşük
- Mantıksal Disk boş alan (MB) düşük
- Mantıksal Disk boş kalma süresi yüzdesi
- Saniye başına bellek sayfaları
- Yüzde bant kullanılan okuma
- Toplam yüzde bant kullanılan
- Yüzde bant kullanılan yazma
- Kullanımdaki kaydedilmiş bellek yüzdesi
- Disk yüzde boşta kalma süresi
- DHCP istemci hizmeti durumu
- DNS İstemcisi hizmeti durumu
- RPC hizmeti durumu
- Sunucu hizmeti durumu
- Toplam CPU kullanım yüzdesi
- Windows olay günlüğü hizmeti durumu
- Windows Güvenlik Duvarı hizmeti durumu
- Windows Uzaktan Yönetimi hizmetinin sistem durumu

### <a name="linux-vms"></a>Linux VM'leri

- Disk ortalaması Disk sn/Aktarım
- Disk ortalaması Disk sn/Okuma
- Disk ortalaması Disk sn/yazma
- Disk durumu
- Mantıksal Disk boş alan
- Mantıksal Disk % boş alan
- Mantıksal Disk % boş Inode'ları
- Ağ bağdaştırıcısı durumu
- Toplam yüzde işlemci zamanı
- İşletim sistemi kullanılabilir megabayt belleği

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Oturum açmak için Git [Azure portalında](https://portal.azure.com).

## <a name="introduction-to-azure-monitor-for-vms-health"></a>VM sistem durumu için Azure İzleyici'a giriş

Tek bir VM veya VM için sistem durumu özelliği kullanmadan önce bilgileri nasıl görüntülenir ve görselleştirmeler ne temsil anlamak önemlidir.

### <a name="view-health-directly-from-a-vm"></a>Doğrudan bir VM'den durumunu görüntüle

Bir Azure VM durumunu görüntülemek için seçin **Insights (Önizleme)** sol bölmesinde sanal makinenin. VM içgörüler sayfasında **sistem durumu** sekmesi varsayılan olarak açık olduğundan ve VM sistem durumu görünümünü gösterir.

![Vm'leri sistem durumuna genel bakış seçili Azure sanal makine için Azure İzleyici](./media/vminsights-health/vminsights-directvm-health.png)

İçinde **sistem durumu** sekmesindeki **Konuk VM sistem durumu**tablo sanal makinenin sistem durumunu gösterir ve VM sistem durumu uyarılarını toplam sayısı, iyi durumda olmayan bir bileşen tarafından oluşturulur.

Daha fazla bilgi için [uyarılar](#alerts).

Bir VM için tanımlanan sistem durumları aşağıdaki tabloda açıklanmıştır:

|Simge |Sistem durumu |Anlamı |
|-----|-------------|---------------|
| |Sorunsuz |İçinde tanımlanan sistem durumu koşullarını vm'dir. Bu durum, hiçbir sorun algılanmadı ve sanal Makineyi normal olarak çalışıyor var. gösterir. Üst döküm İzleyicisi sistem durumu toplar ve alt best-case veya iki katına durumunu yansıtır.|
| |Kritik |Durumu gösteren tanımlanan sistem durumu koşulu içinde değil veya daha fazla kritik sorunlar algılandı. Normal işlevselliğini geri yüklemek için bu sorunları ele alınması gerekir. Üst döküm İzleyicisi sistem durumunu toplar ve alt best-case veya iki katına durumunu yansıtır.|
| |Uyarı |Burada bir uyarı durumu ve diğer kritik duruma (üç sağlık durumu eşikleri yapılandırılabilir) gösterir veya ne zaman kritik olmayan bir sorunu öenmli sorunlara neden olabilir gösterir tanımlanan sistem durumu koşulu, iki eşikleri arasında durum şeklindedir Çözümlenmemiş. Bir veya daha fazla alt öğe varsa bir uyarı durumunda üst döküm İzleyicisi ile üst bir uyarı durumu yansıtır. Kritik durum ve başka bir alt uyarı durumundaki bir alt öğesidir üst döküm durumunun kritik olarak gösterilir.|
| |Bilinmiyor |Durum çeşitli nedenlerle hesaplanamıyor. Aşağıdaki bölümde ek ayrıntılar ve olası çözümleri sağlar. |

Bilinmeyen bir durumu aşağıdaki nedenlerle oluşabilir:

- Aracıyı yeniden yapılandırıldı ve VM'ler için Azure İzleyici etkin olduğunda, çalışma alanına rapor artık belirtilen. Çalışma alanı görmek bildirmek için aracıyı yapılandırmak için [ekleyerek veya kaldırarak bir çalışma alanı](../platform/agent-manage.md#adding-or-removing-a-workspace).
- Sanal makine silindi.
- VM'ler için Azure İzleyici ile ilişkili çalışma alanı silindi. Premier Destek avantajlarını varsa çalışma kurtarabilirsiniz. Git [Premier](https://premier.microsoft.com/) ve bir destek isteği açın.
- Çözüm bağımlılıkları silindi. Log Analytics çalışma alanınızın ServiceMap ve InfrastructureInsights çözümleri yeniden etkinleştirmek için bu çözümleri kullanarak yeniden [Azure Resource Manager şablonu](vminsights-enable-at-scale-powershell.md#install-the-servicemap-and-infrastructureinsights-solutions). Veya, Get Started sekmesinde bulunan çalışma alanı yapılandırma seçeneğini kullanın.
- Sanal makine kapatıldı.
- Azure VM Hizmet kullanılamıyor veya bakım gerçekleştirilir.
- Çalışma alanı [günlük verileri veya bekletme sınırı](../platform/manage-cost-storage.md) aşıldığı.

Seçin **görüntüleme durumu tanılama** durumu ölçütlerini, durum değişikliklerini ve diğer sorunları VM'le ilgili bileşenler izleme tarafından algılanan ilişkili bir sanal makinenin tüm bileşenleri gösteren bir sayfasını açın.

Daha fazla bilgi için [sistem tanılama](#health-diagnostics).

İçinde **sistem durumu** bölümünde, bir tablo gösterir performans bileşenlerin durumu ölçütlerini tarafından izlenen sistem durumu dökümü. Bu bileşenler dahil **CPU**, **bellek**, **Disk**, ve **ağ**. Bir bileşen seçtiğinizde, tüm izleme ölçütü ve söz konusu bileşen sistem durumu listeleyen bir sayfa açılır.

Windows çalıştıran bir Azure VM'den sistem durumu eriştiğinizde, ilk beş çekirdek Windows Hizmetleri sistem durumunu altında gösterilir **çekirdek sistem durumu Hizmetleri**. Hizmetlerden herhangi birinin seçilmesi, sistem sağlığı durumuna yanı sıra söz konusu bileşen için izleme durumu ölçütlerini listeleyen bir sayfa açılır.

Sistem durumu ölçütlerini adını seçerek özellik bölmesi açılır. Bu bölümde, karşılık gelen bir Azure İzleyici uyarı durumu ölçütlerini varsa dahil olmak üzere yapılandırma ayrıntılarını gözden geçirebilirsiniz.

Daha fazla bilgi için [sistem durumu tanılama ve çalışma durumu ölçütlerini](#health-diagnostics).

### <a name="aggregate-vm-perspective"></a>Toplam VM perspektifi

Bir kaynak grubundaki tüm Vm'leriniz için sistem durumu toplama görüntülemek için seçin **Azure İzleyici** portalı tıklayın ve ardından Gezinti listeden **sanal makineler (Önizleme)** .

![Azure İzleyici görünümünden izleme VM öngörüleri](./media/vminsights-health/vminsights-aggregate-health.png)

İçinde **abonelik** ve **kaynak grubu** açılan listeler, Vm'leri içeren uygun kaynak grubu seçin, gruba bildirilen sistem durumlarını görüntülemek için ilgili. Seçiminiz yalnızca sistem durumu özellik için geçerlidir ve aktarılmaz **performans** veya **harita** sekmeler.

**Sistem durumu** sekmesine aşağıdaki bilgileri sağlar:

* Kaç tane Vm'niz ne kadar iyi durumda olduğunu ve kritik veya sağlıksız durumda olduğu veya (Bilinmeyen durumu adlandırılır) veri gönderme değil.
* Hangi ve kaç tane sanal makine işletim sistemi tarafından düzgün çalışmayan bir durum raporlama.
* Kaç tane Vm'niz bir işlemci, disk, bellek veya ağ bağdaştırıcısı, sistem durumuna göre kategorilere algılanan bir sorun nedeniyle sağlıksız.
* Kaç tane Vm'niz hizmetiyle sistem durumuna göre kategorilere ayrılmış bir çekirdek işletim sistemi, algılanan bir sorun nedeniyle sağlıksız.

Üzerinde **sistem durumu** sekmesi, VM'yi ve uyarı ayrıntılarını gözden geçirin ve ilişkili bilgi makalelerini izleme durumu ölçütlerini tarafından algılanan kritik sorunları belirleyebilir. Bu makaleler, tanılama ve sorunları düzeltmeyi yardımcı olabilir. Açmak için önem derecelerinin birini seçin [tüm uyarıları](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) sayfası, önem derecesine göre filtrelendi.

**İşletim sistemine göre VM Dağıtım** listede Windows sürümünü veya sürümlerini birlikte Linux dağıtımı tarafından listelenen VM'ler gösterilmektedir. Her işletim sistemi kategorisinde, Vm'leri başka ayrılmıştır VM Durumu'na göre.

![VM Insights sanal makine dağıtım perspektifi](./media/vminsights-health/vminsights-vmdistribution-by-os.png)

Herhangi bir sütunu içeren seçin **VM sayısı**, **kritik**, **uyarı**, **sağlıklı**, veya **bilinmeyen**. Filtrelenmiş sonuç listesini görüntülemek **sanal makineler** seçili sütun eşleşmesi sayfası.

Örneğin, Red Hat Enterprise Linux sürüm 7.5 çalıştıran tüm sanal makineler gözden geçirmek için seçin **VM sayısı** değeri, işletim sistemi ve bu filtre ve mevcut sistem durumuna eşleşen sanal makineleri listeler.

![Red Hat Linux sanal makinelerinin örnek paketi](./media/vminsights-health/vminsights-rollup-vm-rehl-01.png)

İçinde **sanal makineler** sayfa adı sütunu altında bir sanal Makineyi seçerseniz **VM adı**, için yönlendirilirsiniz **VM örneği** sayfası. Bu sayfa uyarılar ve seçili VM etkileyen sistem durumu ölçütlerini sorunları daha fazla ayrıntı sağlar. Sistem durumu ayrıntıları seçerek filtre **sistem durumu** hangi bileşenlerin sağlıksız olduğunu görmek için sayfanın sol üst köşesindeki simgesi. Ayrıca, uyarı önem derecesine göre kategorilere sağlıksız bir bileşen tarafından gerçekleştirilen VM sistem durumu uyarılarını görüntüleyebilirsiniz.

Gelen **VM listesini** görünümünde, açmak için bir VM adı seçin **sistem durumu** seçtiyseniz, bu VM için benzer şekilde sayfasında **Insights (Önizleme)** VM'den doğrudan.

![Seçili Azure sanal makinesinin VM öngörüleri](./media/vminsights-health/vminsights-directvm-health.png)

**Insights (Önizleme)** sayfası VM ve Uyarılar için bir toplama sistem durumunu gösterir. Bu sistem durumu temsil eden sistem durumu kötü olarak değiştirilen sağlıklıdan harekete geçirilen VM sistem durumu uyarılarını ölçütlere göre önem derecesine göre sınıflandırılır. Seçme **kritik durumdaki VM'ler** kritik sistem durumu bir veya daha fazla VM'lerin listesini içeren bir sayfa açar.

Vm'leri programlardan biri sistem durumunu seçerek **sistem tanılama** VM görünümü. Bu görünümde, bir sistem durumu sorunu hangi durumu ölçütlerini yansıtacak şekilde belirleyebilirsiniz. Zaman **sistem tanılama** sayfası açıldığında, tüm sanal makine bileşenleri ve bunların ilişkili sistem ölçütü geçerli sistem durumunu gösterir.

Daha fazla bilgi için [sistem tanılama](#health-diagnostics).

Seçme **tüm sistem durumu ölçütlerini görüntülemek** bu özellik ile kullanılabilir tüm sistem durumu ölçütlerini listesini gösteren bir sayfa açar. Bilgileri daha fazla bağlı olarak aşağıdaki seçenekleri filtrelenebilir:

* **Tür**. Koşullar değerlendirmek ve izlenen bir VM genel sistem durumunun dökümünü almak için sistem durumu ölçütlerini üç tür vardır:
    - **Birim**. Bir VM bazı yönlerini ölçer. Bu sistem durumu ölçüt türü bileşeninin performansı belirlemek için bir performans sayacı yapay bir işlem gerçekleştirmek için bir betik çalıştırma veya hata veren bir olayı izlemek denetimi yapıyor olabilir. Filtre, birim için varsayılan olarak ayarlanır.
    - **Bağımlılık**. Farklı varlıklar arasındaki bir sistem durumu toplaması sağlar. Bu durumu ölçütlerini başka türde bir başarılı bir işlem kullanır varlık durumunu bağımlı bir varlık durumunu sağlar.
    - **Toplama**. Birleşik sistem durumu benzer durumu ölçütlerini sağlar. Birim ve bağımlılık sistem durumu ölçütü genellikle bir toplama sistem durumu ölçütü altında yapılandırılır. Bir varlıkta hedeflenen birçok farklı durumu ölçütlerini daha iyi bir genel düzenleme sağlamanın yanı sıra toplama sistem durumu ölçütü varlıkların farklı kategorileri için benzersiz bir sistem durumu sağlar.

* **Kategori**. Raporlama amacıyla benzer ölçütlerini gruplandırmak için kullanılan sistem durumu ölçütlerini türü. Bu kategoriler **kullanılabilirlik** ve **performans**.

Hangi örneklerinin sağlıksız olduğunu görmek için altında bir değer seçin **sağlıksız bileşen** sütun. Bu sayfada, bir tablo bir kritik sağlık durumunda olan bileşenler listelenir.

## <a name="health-diagnostics"></a>Sistem durumu tanılama

**Sistem tanılama** sayfa VM sistem durumu modeli görselleştirmenize olanak tanır. Bu sayfa, tüm sanal makine bileşenleri, ilişkili durumu ölçütlerini, durum değişikliklerini ve VM'le ilgili izlenen bileşenler tarafından tanımlanan diğer önemli sorunları listeler.

![Bir sanal makine için sistem durumu tanılama sayfası örneği](./media/vminsights-health/health-diagnostics-page-01.png)

Sistem durumu Tanılama, aşağıdaki yöntemleri kullanarak başlatın:

* Azure İzleyici'de toplama VM açısından tüm sanal makineler için toplama sistem durumuna göre:

    1. Üzerinde **sistem durumu** simgesini seçin, sayfa **kritik**, **uyarı**, **sağlıklı**, veya **bilinmeyen** Sistem durumu bölümünün altında **Konuk VM sistem durumu**.
    2. Filtrelenmiş kategori eşleşen tüm Vm'leri listeleyen sayfasına gidin.
    3. Bir değer seçin **sistem durumu** bu VM'ye kapsamlı sistem tanılama'yı açmak için sütun.

* Toplam VM açısından Azure İzleyici'de işletim sistemi tarafından. Altında **VM Dağıtım**, sütun değerlerini herhangi birini seçerek açılır **sanal makineler** sayfasında ve filtrelenmiş kategori eşleşen tabloda bir liste döndürür. Değerin altında seçerek **sistem durumu** sütun, seçili VM için sistem durumu tanılama açılır.
 
* Konuk sanal makinesinde sanal makineler için Azure İzleyici'den **sistem durumu** sekmesini seçerek **görüntüleme durumu tanılama**.

Sistem durumu tanılama sistem durumu bilgilerini iki kategoride düzenler: kullanılabilirlik ve performans.
 
Mantıksal disk, CPU ve benzeri gibi bir bileşen için tanımlanan tüm sistem durumu ölçütlerini iki kategorilere göre filtreleme olmadan görüntülenebilir. Bu görünüm tüm görünümde ölçütlerin veya seçtiğinizde, sonuçlar iki kategoriye göre filtreleme yoluyla olabilir **kullanılabilirlik** veya **performans**.

Ayrıca, ölçütleri kategorinin yanındaki görülebilir **durumu ölçütlerini** sütun. Ölçüt, seçilen kategori eşleşmiyorsa belirten bir ileti **seçilen kategori için sistem durumu ölçütü yok** görünür **durumu ölçütlerini** sütun.

Sistem durumu ölçütlerini durumu dört türlerinden biri tanımlanır: **Kritik**, **uyarı**, **sağlıklı**, ve **bilinmeyen**. İlk üç eşik değerlerini doğrudan izleyicilerin değiştirebileceğiniz anlamına gelir, yapılandırılabilir **durumu ölçütlerini** yapılandırma bölmesi. Bu aynı zamanda Azure İzleyici REST API'sini kullanarak mümkündür [güncelleştirme izleme işlemi](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update). **Bilinmeyen** yapılandırılabilir değildir ve belirli senaryolar için ayrılmıştır.

**Sistem tanılama** sayfasında üç ana bölümü vardır:

* Bileşen modeli
* Durum Ölçütleri
* Durum değişiklikleri

![Sistem durumu tanılama sayfası bölümleri](./media/vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Bileşen modeli

En soldaki sütunda **sistem tanılama** sayfasıdır **bileşen modeli**. VM ile ilişkili olan tüm bileşenleri bu sütun geçerli sistem durumları ile birlikte görüntülenir.

Aşağıdaki örnekte bulunan bileşenlerdir **Disk**, **Mantıksal Disk**, **İşlemci**, **bellek**, ve  **İşletim sistemi**. Bu bileşenlerin birden çok örneği bulundu ve bu sütunda görüntülenir.

Örneğin, aşağıdaki şekilde VM mantıksal disk iki örneği olduğunu gösterir. **C:** ve **D:** , iyi durumda olan:

![Sistem durumu tanılamada sunulan örnek bileşen modeli](./media/vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Sistem durumu ölçütü

Sistem durumu tanılama sayfası merkezi sütunda **durumu ölçütlerini**. VM için tanımlanan sistem durumu modeli, hiyerarşik bir ağaç şeklinde görüntülenir. Bir sanal makine için sistem durumu modeli, birim ve toplama durumu ölçütlerini oluşur.

![Sistem durumu tanılamada sunulan örnek durumu ölçütü](./media/vminsights-health/health-diagnostics-page-healthcriteria.png)

Sistem durumu ölçüt bir varlık ve benzeri durum Eşiği değeri olabilecek bir izlenen örneği sağlığını ölçer. Bir sistem durumu ölçütü, daha önce açıklandığı gibi yapılandırılabilir sağlık durumu eşikleri, iki veya üç sahiptir. Herhangi bir noktada, sistem durumu ölçütü potansiyel durumlarının sadece birinde olabilir.

Sistem durumu modeli genel hedef durumunu ve hedef bileşenler belirleyen ölçütleri tanımlar. Ölçüt hiyerarşisini gösterilen **durumu ölçütlerini** bölümünde **sistem tanılama** sayfası.

Sistem durumu Toplama İlkesi toplama durumu ölçütlerini yapılandırmasının bir parçasıdır (varsayılan değer kümesine **en kötü,** ). Bu özelliği bir parçası olarak çalışan sistem durumu ölçütlerini varsayılan kümesini bulabilirsiniz [izleme yapılandırma ayrıntılarını](#monitoring-configuration-details) bu makalenin.

Azure İzleyici REST API de kullanabilirsiniz [İzleyici örnekleri listesi kaynak tarafından](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitorinstances/listbyresource) tüm sistem durumu ölçütlerini listesini almak için. Bu ölçütler Azure VM kaynak karşı çalışan yapılandırma ayrıntılarını içerir.

**Birim** sistem durumu ölçüt türü sağ tarafındaki üç nokta bağlantısını seçerek değiştiren yapılandırmasına sahip olabilir. Seçin **ayrıntıları göster** yapılandırma bölmesini açmak için.

![Bir sistem durumu ölçütlerini örnek yapılandırma](./media/vminsights-health/health-diagnostics-vm-example-02.png)

Örnek kullanırsanız Seçilen sistem durumu ölçütlerini için yapılandırma bölmesinde **Disk başına saniyede ortalama yazma**, eşiği farklı bir sayısal değer ile yapılandırılabilir. Yalnızca değiştirebilir, yani bir iki durumlu izleme, olup **sağlıklı** için **uyarı**.

Diğer durumu ölçütlerini bazen üç durumu, uyarı ve kritik sistem durumu eşikleri değeri yapılandırabileceğiniz kullanın. Azure İzleyici REST API'sini kullanarak bir eşik değiştirebilirsiniz [İzleyici yapılandırması](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update).

>[!NOTE]
>Bir örneği için sistem durumu ölçütlerini yapılandırma değişikliklerini uygulama izlenen tüm örneklerine uygulanır. Örneğin, **-1 D: Disk** ve ardından **Disk başına saniyede ortalama yazma** eşiği değişikliği bulunan ve sanal makinede izlenen tüm örneklerine uygular.


![Bir sistem durumu ölçütlerini bir birim İzleyicisi örneği yapılandırma](./media/vminsights-health/health-diagnostics-criteria-config-01.png)

Sistem durumu ölçütleri hakkında daha fazla bilgi edinmek istiyorsanız, sorunları, nedenleri ve çözümlemeleri belirlemenize yardımcı olmak için Bilgi Bankası makaleleri ekledik. Seçin **bilgilerini görüntüleyin** sayfasında İlgili Bilgi Bankası makalesine bakın.

VM sistem durumu için Azure İzleyici ile eklenen tüm Bilgi Bankası makaleleri gözden geçirmek için bkz: [Azure İzleyici sistem durumu bilgisi belgeleri](https://docs.microsoft.com/azure/monitoring/infrastructure-health/).

### <a name="state-changes"></a>Durum değişiklikleri

En sağdaki sütunun **sistem tanılama** sayfasıdır **durum değişikliklerini**. Bu sütun, seçili durumu ölçütlerini ilişkilendirilmiş tüm durum değişikliklerini listeler **durumu ölçütlerini** bölüm veya bir VM'ye gelen seçildiyse sanal makinenin durumu değişikliği **bileşen modeli** veya  **Sistem durumu ölçütlerini** tablosunda sütun.

![Sistem durumu tanılamada sunulan örnek durum değişiklikleri](./media/vminsights-health/health-diagnostics-page-statechanges.png)

Aşağıdaki bölümde, durumu ölçütlerini durumu ve ilişkili süreyi gösterir. Bu bilgiler, en son durum sütununun üst kısmındaki gösterir.

### <a name="association-of-component-model-health-criteria-and-state-changes-columns"></a>Bileşen modeli, durumu ölçütlerini ve durum değişikliklerini sütunların ilişkilendirme

Üç sütun birbirleri ile birbirine bağlıdır. Bir örneğine seçtiğinizde **bileşen modeli** sütun **durumu ölçütlerini** sütuna o bileşen görünüme filtre. Gelenlere, **durum değişikliklerini** sütun, seçili durum ölçütlere göre güncelleştirilir.

![İzlenen örneği ve sonuçları seçme örneği](./media/vminsights-health/health-diagnostics-vm-example-01.png)

Örneğin, *Disk - 1 D:* altındaki listeden **bileşen modeli**, **durumu ölçütlerini** filtresi için *Disk - 1 D:* ve  **Durumu değiştiğinde** durum değişikliği kullanılabilirliğini gösterir *Disk - 1 D:* .

Güncelleştirilmiş bir durumu görmek için sistem durumu tanılama sayfası seçerek yenileyebilirsiniz **Yenile** bağlantı. Önceden tanımlanmış bir yoklama aralığı temel sistem durumu ölçütü'nın sistem durumu için bir güncelleştirme varsa, bu görevi bekleme sürelerinden kurtulun sağlar ve en son sistem durumu yansıtır. **Durumu ölçütlerini** seçili sistem durumuna bağlıdır sonuçları kapsam olanak tanıyan bir filtre: İyi durumda, uyarı, kritik, bilinmeyen ve tüm. **Son güncelleştirilen** sağ üst köşedeki zaman sistem durumu tanılama sayfası yenilenir son zamanı temsil eder.

## <a name="alerts"></a>Uyarılar

VM sistem durumu için Azure İzleyici ile tümleştirilir [Azure uyarıları](../../azure-monitor/platform/alerts-overview.md). Bir uyarı başlatır algılandığında, önceden tanımlanmış ölçütleri düzgün çalışmayan bir durumu sağlıklı durumdan değiştirdiğinizde. Uyarı önem derecesi 0 önem derecesi 4 aracılığıyla ile en üst düzey olarak önem derecesi 0'dan önem derecesine göre sınıflandırılır.

Uyarılar, uyarı tetiklendiğinde size bildirmek için bir eylem grubu ile ilişkili değildir. Abonelik sahibi içindeki adımları izleyerek bildirimleri yapılandırmalısınız [uyarılarını yapılandırma](#configure-alerts) bölümü.

VM sistem durumu Uyarıları önem derecesine göre kategorilere toplam sayısı üzerindeki kullanılabilir **sistem durumu** Pano altındaki **uyarılar** bölümü. Toplam uyarı sayısını veya bir önem derecesi düzeyine karşılık gelen sayısı seçtiğinizde **uyarılar** sayfası açılır ve seçiminizi eşleşen tüm uyarıları listeler.

Örneğin, satır karşılık gelen seçerseniz **önem derecesi düzeyi 1**, aşağıdaki görünümü görürsünüz:

![Tüm önem düzeyi 1 uyarı örneği](./media/vminsights-health/vminsights-sev1-alerts-01.png)

**Tüm uyarıları** sayfa seçiminizi eşleşen uyarıları göstermek için kapsamlı değildir. Ayrıca göre filtrelenir **kaynak türü** yalnızca VM kaynak tarafından gerçekleştirilen sistem durumu uyarıları göstermek için. Bu biçim sütununun altında Uyarı listesinden yansıtılan **hedef kaynak**, iyi durumda olmayan bir koşul sağlandığında Burada, Azure VM yükseltilmiş uyarıyı gösterir.

Bu görünümde dahil edilecek diğer kaynak türlerini veya hizmetler uyarılardan amaçlanmamıştır. Bu uyarılar, günlük sorguları veya Azure İzleyici varsayılan olarak normal şekilde görüntülediğiniz ölçüm uyarıları göre günlük uyarıları içerir [tüm uyarıları](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) sayfası.

Bu görünümde, sayfanın üst kısmındaki açılan menülerde değerleri seçerek filtreleyebilirsiniz.

|Sütun |Açıklama |
|-------|------------|
|Abonelik |Bir Azure aboneliği seçin. Yalnızca seçili Abonelikteki uyarılar görünümünde dahil edilir. |
|Kaynak Grubu |Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunun hedefleri olan uyarılar görünümünde dahil edilir. |
|Kaynak türü |Bir veya daha fazla kaynak türlerini seçin. Varsayılan olarak, yalnızca hedef uyarıları **sanal makineler** seçilir ve bu görünüme dahil. Bu sütun, yalnızca bir kaynak grubu belirttikten sonra kullanılabilir. |
|Resource |Bir kaynak seçin. Yalnızca bu kaynak bir hedef olarak uyarılarla Görünümü'nde dahil edilir. Bu sütun, yalnızca bir kaynak türü belirtildikten sonra kullanılabilir. |
|Severity |Bir uyarı önem derecesini seçin ya da seçin **tüm** her türlü önem derecesi, uyarı eklenecek. |
|İzleme koşulu |Bunlar tetiklenen veya yüklenmiş koşul artık etkin olduğunda sistem tarafından çözümlenen bir filtre uyarılarını izleme koşulu seçin. Ya da seçin **tüm** tüm koşulların uyarıları eklemek için. |
|Uyarı durumu |Bir uyarı durumu seçin **yeni**, **kabul**, **kapalı**, veya **tüm** tüm durumların uyarıları eklemek için. |
|İzleme hizmeti |Bir hizmet veya seçin **tüm** tüm hizmetleri dahil etmek için. Yalnızca VM ınsights uyarıları, bu özellik için desteklenir.|
|Zaman aralığı| Yalnızca seçili zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Desteklenen değerler şunlardır: son bir saat, son 24 saat, son 7 günde ve son 30 gün. |

Bir uyarıyı seçtiğinizde **uyarı ayrıntısı** sayfası görüntülenir. Bu sayfa, bir uyarının ayrıntılarını sağlar ve durumuna değiştirmenize izin verir.

Uyarıları yönetme hakkında daha fazla bilgi için bkz: [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme](../../azure-monitor/platform/alerts-metric.md).

>[!NOTE]
>Yeni uyarı durumu ölçütlerini veya değiştirme mevcut durumu uyarı kuralları Azure İzleyici'de portaldan göre oluşturulması şu anda desteklenmemektedir.


![Seçilen bir uyarının uyarı ayrıntıları bölmesine](./media/vminsights-health/alert-details-pane-01.png)

Bunları seçerek ve ardından bir veya birden çok uyarı için bir uyarı durumu değiştirebilirsiniz **durumunu değiştir** gelen **tüm uyarıları** sol üst köşedeki sayfa. Durumlardan biriyle seçin **uyarı durumunu değiştir** bölmesinde değişiklik açıklamasını ekleyin **yorum** alan ve ardından **Tamam** değişiklikleri işlemek için. Bilgiler doğrulanır ve değişiklikler uygulanır, altında ilerleme durumunu izlemek **bildirimleri** menüsünde.

### <a name="configure-alerts"></a>Uyarı yapılandırma
Bazı uyarı yönetim görevleri, Azure portalından yönetemezsiniz. Bu görevleri kullanarak gerçekleştirilmelidir [Azure İzleyici REST API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components). Bu avantajlar şunlardır:

- Bir uyarı durumu ölçütlerini için devre dışı bırakma veya etkinleştirme
- Sistem durumu ölçütlerini uyarılar için bildirimleri ayarlama

Her örnekte [ARMClient](https://github.com/projectkudu/armclient) Windows makinenizde. Bu yöntem ile ilgili bilgi sahibi değilseniz bkz [kullanarak ARMClient](../platform/rest-api-walkthrough.md#use-armclient).

#### <a name="enable-or-disable-an-alert-rule"></a>Etkinleştirmek veya devre dışı bir uyarı kuralı

Etkinleştirme veya devre dışı özel durumu ölçütlerini, özellik için bir uyarı **alertGeneration** ya da değeriyle değiştirilmelidir **devre dışı bırakılmış** veya **etkin**.

Tanımlamak için *Monitorıd* belirli bir sistem için aşağıdaki örnekte ölçütleri değerini sorgulamak nasıl ölçütlerini **LogicalDisk\Avg Disk başına saniye aktarım**:

1. Bir terminal penceresinde şunu yazın **armclient.exe oturum açma**. Bunun yapılması Azure'da oturum açmanız istenir.

2. Belirli bir sanal makine üzerinde etkin tüm sistem durumu ölçütü almak ve değerini belirlemek için aşağıdaki komutu girin *Monitorıd* özelliği:

    ```
    armclient GET "subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors?api-version=2018-08-31-preview”
    ```

    Aşağıdaki örnek, çıktısını gösterir *armclient GET* komutu. Değerini not *Monitorıd*. Bu değer, burada gerekir Kimliğini durumu ölçütlerini belirtin ve bir uyarı oluşturmak için kendi özelliğini değiştirmek sonraki adım için gereklidir.

    ```
    "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/monitors/ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "name": "ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "type": "Microsoft.WorkloadMonitor/virtualMachines/monitors"
    },
    {
      "properties": {
        "description": "Monitor the performance counter LogicalDisk\\Avg Disk Sec Per Transfer",
        "monitorId": "Microsoft_LogicalDisk_AvgDiskSecPerTransfer",
        "monitorName": "Microsoft.LogicalDisk.AvgDiskSecPerTransfer",
        "monitorDisplayName": "Average Logical Disk Seconds Per Transfer",
        "parentMonitorName": null,
        "parentMonitorDisplayName": null,
        "monitorType": "Unit",
        "monitorCategory": "PerformanceHealth",
        "componentTypeId": "LogicalDisk",
        "componentTypeName": "LogicalDisk",
        "componentTypeDisplayName": "Logical Disk",
        "monitorState": "Enabled",
        "criteria": [
          {
            "healthState": "Warning",
            "comparisonOperator": "GreaterThan",
            "threshold": 0.1
          }
        ],
        "alertGeneration": "Enabled",
        "frequency": 1,
        "lookbackDuration": 17,
        "documentationURL": "https://aka.ms/Ahcs1r",
        "configurable": true,
        "signalType": "Metrics",
        "signalName": "VMHealth_Avg. Logical Disk sec/Transfer"
      },
      "etag": null,
    ```

3. Değiştirmek için aşağıdaki komutu girin *alertGeneration* özelliği:

    ```
    armclient patch subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors/Microsoft_LogicalDisk_AvgDiskSecPerTransfer?api-version=2018-08-31-preview "{'properties':{'alertGeneration':'Disabled'}}"
    ```   

4. 2\. adımda özellik değerini ayarlamak doğrulamak için kullanılan GET komutu girin **devre dışı bırakılmış**.

#### <a name="associate-an-action-group-with-health-criteria"></a>Bir eylem grubu durumu ölçütlerini ile ilişkilendirme

VM sistem durumu için azure İzleyici, sağlıksız durum ölçütlerinden uyarılar oluşturulduğunda SMS ve e-posta bildirimleri destekler. Bildirimleri yapılandırmak için SMS veya e-posta bildirimleri göndermek için yapılandırılan bir eylem grubu adını not edin.

>[!NOTE]
>Bu eylem için bir bildirim almak istediğiniz izlenen her bir VM'ye karşı gerçekleştirilmesi gerekir. Bir kaynak grubundaki tüm sanal makineler için geçerli değildir.

1. Bir terminal penceresinde girin *armclient.exe oturum açma*. Bunun yapılması Azure'da oturum açmanız istenir.

2. Bir eylem grubu uyarı kuralları ile ilişkilendirmek için aşağıdaki komutu girin:
 
    ```
    $payload = "{'properties':{'ActionGroupResourceIds':['/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/microsoft.insights/actionGroups/actiongroupName']}}"
    armclient PUT https://management.azure.com/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings/default?api-version=2018-08-31-preview $payload
    ```

3. Doğrulamak için özelliğinin değeri **actionGroupResourceIds** başarıyla güncelleştirildi, aşağıdaki komutu girin:

    ```
    armclient GET "subscriptions/subscriptionName/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings?api-version=2018-08-31-preview"
    ```

    Çıktı aşağıdaki ölçütleri gibi görünmelidir:
    
    ```
    {
      "value": [
        {
          "properties": {
            "actionGroupResourceIds": [
              "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourceGroups/Lab/providers/microsoft.insights/actionGroups/Lab-IT%20Ops%20Notify"
            ]
          },
          "etag": null,
          "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/notificationSettings/default",
          "name": "notificationSettings/default",
          "type": "Microsoft.WorkloadMonitor/virtualMachines/notificationSettings"
        }
      ],
      "nextLink": null
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Sınırlamalar ve genel sanal makine performansını belirlemek için bkz. [görünümü Azure VM performansını](vminsights-performance.md).
- Bulunan uygulama bağımlılıkları hakkında bilgi edinmek için [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).
