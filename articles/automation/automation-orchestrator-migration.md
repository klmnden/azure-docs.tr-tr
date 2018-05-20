---
title: Orchestrator'ı Azure Otomasyonu geçirme
description: System Center Orchestrator'ı Azure Otomasyonu runbook'ları ve tümleştirme paketleri geçirmeyi açıklar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f7050a034bea3a92376afbebb3b1489e61382a83
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Orchestrator'ı Azure Otomasyon (Beta) geçirme
Runbook'ları [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) Azure automation'daki runbook'lar Windows PowerShell tabanlı, özellikle Orchestrator için yazılmış tümleştirme paketleri etkinliklerden dayanır.  [Grafik runbook'lar](automation-runbook-types.md#graphical-runbooks) Azure Otomasyonu'nda PowerShell cmdlet'leri, alt runbook'ları ve varlıkları temsil eden kendi etkinliklerle Orchestrator runbook'ları için benzer bir görünüm vardır.

[System Center Orchestrator geçiş Araç Seti](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) runbook'lar Orchestrator'ı Azure Otomasyonu dönüştürme yardımcı olan araçlar içerir.  Runbook'ların kendileri dönüştürme ek olarak, runbook'ların etkinlikler içeren tümleştirme paketleri için Windows PowerShell cmdlet'leri ile tümleştirme modülleri dönüştürmeniz gerekir.  

Aşağıda, Orchestrator runbook'ları Azure Otomasyonu dönüştürme temel işlemidir.  Bu adımların her biri aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

1. Karşıdan [System Center Orchestrator geçiş Araç Seti](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) bu makalede açıklanan modülleri ve araçları içerir.
2. İçeri aktarma [standart etkinlikler modül](#standard-activities-module) Azure Otomasyonu içine.  Bu, dönüştürülmüş runbook'lar tarafından kullanılan standart Orchestrator etkinlikleri dönüştürülen sürümlerini içerir.
3. İçeri aktarma [System Center Orchestrator tümleştirme modülleri](#system-center-orchestrator-integration-modules) System Center erişim, runbook'lar tarafından kullanılan bu tümleştirme paketleri için Azure Automation içine.
4. Özel ve üçüncü taraf tümleştirme paketlerini kullanarak dönüştürme [tümleştirme paketi dönüştürücü](#integration-pack-converter) ve Azure Automation'a içeri aktarın.
5. Orchestrator runbook'ları kullanarak dönüştürme [Runbook dönüştürücü](#runbook-converter) ve Azure Otomasyonu'nda yükleyin.
6. El ile Runbook dönüştürücü bu kaynakları dönüştürmez beri Azure Otomasyonu'nda gerekli Orchestrator varlıklar oluşturun.
7. Yapılandırma bir [karma Runbook çalışanı](#hybrid-runbook-worker) yerel kaynaklara erişimini dönüştürülmüş runbook'ları çalıştırmak için yerel veri merkezinde.

## <a name="service-management-automation"></a>Hizmet Yönetim Otomasyonu
[Service Management Automation](http://technet.microsoft.com/library/dn469260.aspx) (SMA) depolar ve Orchestrator gibi yerel veri merkezinizdeki runbook'ları çalıştırır ve Azure Otomasyonu aynı tümleştirme modülleri kullanır. [Runbook dönüştürücü](#runbook-converter) Orchestrator runbook'ları için grafik runbook'lar ancak SMA'da desteklenmez dönüştürür.  Yüklemeye devam edebilirsiniz [standart etkinlikler modül](#standard-activities-module) ve [System Center Orchestrator tümleştirme modülleri](#system-center-orchestrator-integration-modules) SMA, içine ancak el ile [runbook'larınızı yeniden](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı
Orchestrator runbook'ları bir veritabanı sunucusunda depolanan ve runbook sunucuları, hem de yerel veri merkezinizde çalıştırın.  Azure Otomasyonu runbook'ları Azure bulutunda depolanabilir ve yerel veri merkezi kullanarak çalıştırabilirsiniz bir [karma Runbook çalışanı](automation-hybrid-runbook-worker.md).  Yerel sunucuda çalıştırmak için tasarlanmış bu yana Orchestrator'ı dönüştürülen runbook'ları genellikle nasıl çalışacağını budur.

## <a name="integration-pack-converter"></a>Tümleştirme paketi dönüştürücü
Tümleştirme paketi dönüştürücü kullanılarak oluşturulan tümleştirme paketleri dönüştürür [Orchestrator Integration Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx) Azure Otomasyonu veya Service Management Automation içine aktarılabilen bir Windows PowerShell dayalı tümleştirme modülleri için.  

Tümleştirme paketi dönüştürücü çalıştırdığınızda, bir tümleştirme paketi (.oıp) dosyası seçmenizi sağlayacak bir sihirbaz ile sunulur.  Sihirbaz tümleştirme paketinde, etkinlikleri listeler ve hangi geçirilecek seçmenize olanak sağlar.  Sihirbaz tamamlandıktan sonra her biri özgün tümleştirme paketindeki etkinlikler için karşılık gelen bir cmdlet'i içeren bir tümleştirme modülü oluşturur.

### <a name="parameters"></a>Parametreler
Herhangi bir etkinlikte tümleştirme paketi özelliklerini tümleştirme modülü içinde karşılık gelen cmdlet parametreleriyle dönüştürülür.  Windows PowerShell cmdlet'leri sahip bir dizi [ortak parametreler](http://technet.microsoft.com/library/hh847884.aspx) tüm cmdlet'ler ile kullanılabilir.  Örneğin, - Verbose parametresini çalışmasıyla ilgili ayrıntılı bilgileri çıkarmak bir cmdlet neden olur.  Hiçbir cmdlet ortak parametre olarak aynı ada sahip bir parametre gerekebilir.  Bir etkinlik ortak parametresi aynı ada sahip bir özellik varsa, bu sihirbazın parametresi için başka bir ad isteyip istemediğinizi sorar.

### <a name="monitor-activities"></a>İzleme etkinlikleri
İzleme Orchestrator başlangıç ile runbook'ları bir [etkinliğini İzle](http://technet.microsoft.com/library/hh403827.aspx) ve sürekli olarak belirli bir olay tarafından çağrılan bekleyen çalıştırın.  İzleyici etkinlikler tümleştirme paketindeki dönüştürülmeyecek şekilde azure Otomasyonu İzleyici runbook'ları desteklemez.  Bunun yerine, bir yer tutucu cmdlet etkinliğini izleme için tümleştirme modülü oluşturulur.  Bu cmdlet herhangi bir işlevselliğe sahiptir, ancak yüklenecek kullanan herhangi bir dönüştürülmüş runbook sağlar.  Bu runbook'un Azure Otomasyon çalıştırmak mümkün olmaz ancak değiştirmenize olanak sağlayan yüklenebilir.

### <a name="integration-packs-that-cannot-be-converted"></a>Dönüştürülemiyor tümleştirme paketleri
OIT ile oluşturulmayan tümleştirme paketleri ile tümleştirme paketi dönüştürücü dönüştürülemiyor. Bu aracı ile şu anda dönüştürülemez Microsoft tarafından sağlanan bazı tümleştirme paketleri de vardır.  Dönüştürülen bu tümleştirme paketleri sürümleri olmuştur [indirme için sağlanan](#system-center-orchestrator-integration-modules) böylece Azure Otomasyonu veya Service Management Automation yüklenebilir.

## <a name="standard-activities-module"></a>Standart etkinlikler Modülü
Orchestrator içeren bir dizi [standart etkinlikler](http://technet.microsoft.com/library/hh403832.aspx) bir tümleştirme paketinde bulunmayan ancak birçok runbook'lar tarafından kullanılır.  Standart etkinlikler, bu etkinliklerin her biri için eşdeğer bir cmdlet'i içeren bir tümleştirme modülü modülüdür.  Standart etkinlik kullanan dönüştürülmüş runbook'ları içe aktarmadan önce Azure Otomasyonu'nda Bu tümleştirme modülü yüklemeniz gerekir.

Dönüştürülen runbook'lar'ı desteklemenin yanında standart etkinlikler modüldeki cmdlet'ler Orchestrator ile tanıdık bir kişi tarafından Azure Automation, yeni runbook'ları oluşturmak için kullanılabilir.  Standart etkinliklerin tümünü işlevselliğini cmdlet'leriyle gerçekleştirilebilir, ancak bunlar farklı çalışabilir.  Dönüştürülen standart etkinlikler modüldeki cmdlet'ler, ilgili etkinlikler aynı çalışma ve aynı parametreleri kullanın.  Bu, var olan Orchestrator runbook yazarı Azure Otomasyon çalışma kitabı kendi geçiş aşamasında yardımcı olabilir.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator tümleştirme modülleri
Microsoft sağlar [tümleştirme paketleri](http://technet.microsoft.com/library/hh295851.aspx) System Center bileşenleri ve diğer ürünler otomatikleştirmek için runbook'ları oluşturmak için.  Bu tümleştirme paketleri bazıları OIT üzerinde şu anda temel alır ancak tümleştirme modülleri bilinen sorunları nedeniyle şu anda dönüştürülemiyor.  [System Center Orchestrator tümleştirme modülleri](https://www.microsoft.com/download/details.aspx?id=49555) Azure Automation ve Service Management Automation aktarılabilen Bu tümleştirme paketleri dönüştürülen sürümlerini içerir.  

Bu araç RTM sürümü tarafından dönüştürülebilir OIT tümleştirme paketi dönüştürücü ile göre tümleştirme paketlerinin güncelleştirilmiş sürümleri yayımlanır.  Yönergeler ayrıca OIT üzerinde dayanmayan tümleştirme paketlerindeki etkinlikleri kullanarak runbook'ları dönüştürme yardımcı olmak için sağlanır.

## <a name="runbook-converter"></a>Runbook dönüştürücü
Orchestrator runbook'larınızda Runbook dönüştürücü dönüştürür [grafik runbook'lar](automation-runbook-types.md#graphical-runbooks) Azure Automation'a alınabilir.  

Runbook dönüştürücü adlı bir cmdlet ile bir PowerShell modülü olarak uygulanan **ConvertFrom SCORunbook** dönüştürme gerçekleştirir.  Aracı yüklediğinizde, bir kısayol cmdlet yükleyen bir PowerShell oturumuna oluşturur.   

Aşağıdaki Orchestrator runbook'unu dönüştürmek ve Azure Automation'a içeri aktarmak için temel işlemidir.  Aşağıdaki bölümlerde ayrıntıları aracını kullanarak ve dönüştürülen runbook'larla çalışma hakkında daha fazla sağlar.

1. Bir veya daha fazla çalışma kitabı Orchestrator'ı verin.
2. Runbook'taki tüm etkinlikler için tümleştirme modülleri alın.
3. Orchestrator runbook'ları dışa aktarılan dosyada dönüştürün.
4. Dönüştürme doğrulamak ve tüm gerekli el ile görevler belirlemek için günlükleri bilgileri gözden geçirin.
5. Dönüştürülen runbook'ları Azure Automation'a aktarabilir.
6. Gerekli tüm varlıkları Azure Otomasyonu'nda oluşturun.
7. Gerekli etkinlikler değiştirmek için Azure automation'da runbook düzenleyin.

### <a name="using-runbook-converter"></a>Runbook dönüştürücü kullanma
Sözdizimi **ConvertFrom SCORunbook** aşağıdaki gibidir:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>

* RunbookPath - dönüştürmek için runbook'ları içeren dışa aktarma dosyasının yolu.
* Modül - runbook'lar etkinlikler içeren tümleştirme modülleri listesini virgülle ayrılmış.
* OutputFolder - grafik runbook'lar oluşturmak için klasör yolu dönüştürülür.

Aşağıdaki örnek komut olarak adlandırılan dışa aktarma dosyası runbook'ları dönüştürür **MyRunbooks.ois_export**.  Bu runbook'lar Active Directory ve Data Protection Manager tümleştirme paketlerini kullanın.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"


### <a name="log-files"></a>Günlük dosyaları
Runbook dönüştürücü aşağıdaki günlük dosyalarına dönüştürülmüş runbook ile aynı konumda oluşturur.  Dosyalar zaten varsa, ardından bunlar son dönüştürme bilgileriyle üzerine yazılır.

| Dosya | İçindekiler |
|:--- |:--- |
| Runbook dönüştürücü - Progress.log |Ayrıntılı adımlar başarıyla dönüştürülen her etkinlik için bilgi ve uyarı dönüştürülmez her etkinlik için de dahil olmak üzere dönüştürme. |
| Runbook dönüştürücü - Summary.log |Tüm uyarılar dahil olmak üzere son dönüştürme özeti ve dönüştürülen runbook için gerekli bir değişken oluşturma gibi gerçekleştirmek için gereken görevleri izleyin. |

### <a name="exporting-runbooks-from-orchestrator"></a>Orchestrator'ı runbook'ları dışarı aktarma
Runbook dönüştürücü ile runbook'ları bir veya daha fazla Orchestrator dışa aktarma dosyasından çalışır.  Dışa aktarma dosyasında her Orchestrator runbook için karşılık gelen bir Azure Otomasyonu runbook'u oluşturur.  

Orchestrator'ı bir runbook'u dışarı aktarmak, Runbook Designer'da runbook adını sağ tıklatın ve seçin **verme**.  Bir klasördeki tüm runbook'lar vermek için seçin ve klasör adını sağ tıklatın **verme**.

### <a name="runbook-activities"></a>Runbook etkinlikleri
Runbook dönüştürücü Azure Automation karşılık gelen bir etkinliği Orchestrator runbook'taki her etkinlik dönüştürür.  Dönüştürülemez bu etkinlikler için uyarı metni runbook'tan bir yer tutucu etkinlik oluşturulur.  Azure Automation'a dönüştürülmüş runbook içe aktardıktan sonra bu etkinlikler hiçbirini gerekli işlevselliği gerçekleştirmek geçerli etkinlikleri ile değiştirmeniz gerekir.

Tüm Orchestrator etkinlikleri [standart etkinlikler modül](#standard-activities-module) dönüştürülür.  Bu modülde ancak olmayan ve dönüştürülmez bazı standart Orchestrator etkinlikleri vardır.  Örneğin, **Platform olayı Gönder** olay Orchestrator, belirli olduğundan Azure Otomasyonu eşdeğeri vardır.

[İzleme etkinlikleri](https://technet.microsoft.com/library/hh403827.aspx) eşdeğeri bunları Azure Automation olduğundan dönüştürülmez.  İstisnadır izleme etkinlikleri [dönüştürülen tümleştirme paketleri](#integration-pack-converter) , dönüştürülmesi için yer tutucu etkinlik.

Herhangi bir etkinlikten bir [dönüştürülen tümleştirme paketi](#integration-pack-converter) ile tümleştirme modülü yoluna sağlarsanız, dönüştürülecek **modülleri** parametresi.  System Center tümleştirme paketleri için kullandığınız [System Center Orchestrator tümleştirme modülleri](#system-center-orchestrator-integration-modules).

### <a name="orchestrator-resources"></a>Orchestrator kaynakları
Runbook dönüştürücü, runbook'ları, diğer Orchestrator kaynakları sayaçları, değişkenleri veya bağlantıları gibi değil yalnızca dönüştürür.  Sayaçlar, Azure Automation'da desteklenmez.  Değişkenleri ve bağlantıları desteklenir, ancak el ile oluşturmanız gerekir.  Günlük dosyaları runbook gibi kaynakları gerektiriyorsa bildirir ve dönüştürülen runbook'un düzgün çalışması Azure automation'da oluşturmak için gereken ilgili kaynaklar belirtin.

Örneğin, bir runbook belirli bir değeri bir etkinlikte doldurmak için bir değişken kullanabilir.  Dönüştürülen runbook etkinlik dönüştürün ve Orchestrator değişken olarak aynı ada sahip Azure Otomasyonu değişken varlığı belirtin.  Bu, Not edilir **Runbook dönüştürücü - Summary.log** dönüştürmeden sonra oluşturulan dosyası.  Runbook kullanmadan önce bu değişken varlığı Azure Otomasyonu'nda el ile oluşturmanız gerekir.

### <a name="input-parameters"></a>Giriş parametreleri
Orchestrator runbook'ları ile giriş parametreleri kabul **verileri Başlat** etkinlik.  Bu etkinlik dönüştürülmekte olan runbook içeriyorsa, sonra bir [giriş parametresi](automation-graphical-authoring-intro.md#runbook-input-and-output) Azure Automation'da runbook etkinliğinde her parametre için oluşturulur.  A [iş akışı betiği denetim](automation-graphical-authoring-intro.md#activities) etkinlik alır ve her bir parametreyi döndüren dönüştürülmüş runbook oluşturulur.  Giriş parametresi kullanan tüm etkinliklerin çıktıyı bu etkinliğinden bakın.

Bu strateji kullanılır en iyi Orchestrator runbook'ta işlevselliği yansıtmak üzere nedenidir.  Yeni grafik runbook etkinliklerin doğrudan bir Runbook giriş veri kaynağı kullanarak parametreler girişine başvurmalıdır.

### <a name="invoke-runbook-activity"></a>Runbook'u Çağır etkinliğine
Orchestrator runbook'ları başlatmak diğer runbook'larla **Runbook'u Çağır** etkinlik. Dönüştürülen runbook bu etkinliği içeriyorsa ve **tamamlanmasını bekleyin** seçeneği ayarlanmış sonra runbook etkinliği dönüştürülmüş runbook'ta oluşturulur.  Varsa **tamamlanmasını bekleyin** seçeneği ayarlı değil, sonra kullanan bir iş akışı betiği etkinlik oluşturulur **başlangıç AzureAutomationRunbook** runbook'u başlatın.  Azure Automation'a dönüştürülmüş runbook içeri aktardıktan sonra etkinliğinde belirtilen bilgileri ile bu etkinliği değiştirmeniz gerekir.

## <a name="related-articles"></a>İlgili makaleler
* [System Center 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
* [Hizmet Yönetimi Otomasyonu](https://technet.microsoft.com/library/dn469260.aspx)
* [Karma Runbook çalışanı](automation-hybrid-runbook-worker.md)
* [Orchestrator standart etkinlikler](http://technet.microsoft.com/library/hh403832.aspx)
* [İndirme System Center Orchestrator geçiş Araç Seti](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
