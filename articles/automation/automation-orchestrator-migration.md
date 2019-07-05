---
title: Azure otomasyonuna Orchestrator'dan geçiş
description: System Center Orchestrator'ı Azure Otomasyonu runbook'ları ve tümleştirme paketleri geçirmeyi açıklar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: eb5a77668cce96ef45a960908612b502f1520e25
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477598"
---
# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Azure Otomasyonu (Beta) Orchestrator'dan geçiş
Runbook'ları [System Center Orchestrator](https://technet.microsoft.com/library/hh237242.aspx) Azure automation'daki runbook'lar Windows PowerShell tabanlı, özellikle Orchestrator için yazılmış tümleştirme paketleri gerçekleştirilen etkinlikler temel alır.  [Grafik runbook'ları](automation-runbook-types.md#graphical-runbooks) Azure Otomasyonu'nda benzer bir görünümü Orchestrator runbook'ları için PowerShell cmdlet'leri, alt runbook'ları ve varlıkları temsil eden kendi etkinliklerle sahip.

[System Center Orchestrator geçiş Araç Seti](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) runbook'ları Orchestrator'dan Azure Otomasyonu dönüştürme yardımcı olan araçlar içerir.  Runbook'lar dönüştürme ek olarak, Windows PowerShell cmdlet'leri ile tümleştirme modülleri için runbook'ları kullanmak etkinlikleri tümleştirme paketleriyle dönüştürmelisiniz.  

Orchestrator runbook'ları Azure Otomasyonu'na ekleme dönüştürmek için temel işlemi aşağıda verilmiştir.  Bu adımların her biri, aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

1. İndirme [System Center Orchestrator geçiş Araç Seti](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) bu makalede ele alınan modülleri ve araçları içerir.
2. İçeri aktarma [standart etkinlikler modül](#standard-activities-module) Azure Otomasyonu ile.  Bu, dönüştürülen runbook'lar tarafından kullanılan standart Orchestrator etkinlikleri dönüştürülmüş sürümlerini içerir.
3. İçeri aktarma [System Center Orchestrator tümleştirme modülleri](#system-center-orchestrator-integration-modules) System Center erişmek, runbook'lar tarafından kullanılan bu tümleştirme paketleri için Azure Otomasyonu ile.
4. Özel ve üçüncü taraf tümleştirme paketleri kullanarak dönüştürme [tümleştirme paketi dönüştürücü](#integration-pack-converter) ve Azure Automation'a içeri aktarın.
5. Orchestrator runbook'ları kullanarak dönüştürme [Runbook dönüştürücü](#runbook-converter) ve Azure Automation'da yükleyin.
6. El ile Runbook dönüştürücü bu kaynakları dönüştürmez beri Azure Automation'da gerekli Orchestrator varlıklar oluşturun.
7. Yapılandırma bir [karma Runbook çalışanı](#hybrid-runbook-worker) yerel kaynaklara erişen dönüştürülmüş runbook'ları çalıştırmak için yerel veri merkezinizde.

## <a name="service-management-automation"></a>Hizmet Yönetim Otomasyonu
[Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx) (SMA) depolar ve Orchestrator gibi yerel veri merkezinizde runbook'ları çalıştıran ve Azure Otomasyonu aynı tümleştirme modülleri kullanır. [Runbook dönüştürücü](#runbook-converter) SMA'daki desteklenmez ancak Orchestrator runbook'ları grafik runbook'larına dönüştürür.  Yine de yükleyebilirsiniz [standart etkinlikler modül](#standard-activities-module) ve [System Center Orchestrator tümleştirme modülleri](#system-center-orchestrator-integration-modules) içine SMA, ancak kendiniz bağlanmalısınız [runbook'larınızı yeniden](https://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Karma Runbook Çalışanı
Orchestrator runbook'ları bir veritabanı sunucusunda depolanan ve hem de yerel veri merkezinizde runbook sunucularında çalıştırın.  Azure Otomasyonu runbook'ları Azure bulutunda depolanır ve kullanarak, yerel veri merkezi çalıştırılabilir bir [karma Runbook çalışanı](automation-hybrid-runbook-worker.md).  Runbook'ları Orchestrator'dan yerel sunucuda çalıştırmak için tasarlanmış olduğundan dönüştürülen genellikle nasıl çalışacağını budur.

## <a name="integration-pack-converter"></a>Tümleştirme paketi dönüştürücü
Tümleştirme paketi dönüştürücü kullanılarak oluşturulan tümleştirme paketleri dönüştürür [Orchestrator Integration Toolkit (OIT)](https://technet.microsoft.com/library/hh855853.aspx) tümleştirme modülleri Azure Automation'a aktarılabilirler Windows PowerShell tabanlı veya Hizmet Yönetimi Otomasyonu.  

Tümleştirme paketi dönüştürücü çalıştırdığınızda, bir tümleştirme paketi (.oıp) dosyasını seçmek sağlayacak bir sihirbaz ile sunulur.  Sihirbaz, tümleştirme Paketi'ndeki etkinlikler listeler ve hangi geçirilecektir seçmenizi sağlar.  Sihirbazı tamamladığınızda, her biri özgün tümleştirme paketindeki etkinlikler için karşılık gelen bir cmdlet'i içeren bir tümleştirme modülü oluşturur.

### <a name="parameters"></a>Parametreler
Tümleştirme paketi bir etkinliğe ilişkin herhangi bir özelliği tümleştirme modülü karşılık gelen cmdlet parametreleri dönüştürülür.  Windows PowerShell cmdlet'leri sahip bir dizi [ortak parametreleri](https://technet.microsoft.com/library/hh847884.aspx) tüm cmdlet'leri ile kullanılabilir.  Örneğin, Verbose parametresi işleyişi hakkında ayrıntılı bilgiler çıkarmak bir cmdlet neden olur.  Hiçbir cmdlet ortak parametresi ile aynı adda bir parametre gerekebilir.  Bir etkinlik ortak parametresi ile aynı ada sahip bir özellik varsa, bu sihirbazın parametresi için başka bir ad belirtin isteyip istemediğinizi sorar.

### <a name="monitor-activities"></a>İzleme etkinlikleri
Runbook'ları Orchestrator Başlangıç'ta izlemek bir [etkinliğini İzle](https://technet.microsoft.com/library/hh403827.aspx) ve sürekli olarak belirli bir olay tarafından çağrılacak bekleyen çalıştırın.  Tümleştirme paketi etkinlikleri tüm izleme dönüştürülmez için İzleyici runbook'ları, azure Otomasyonu desteklemez.  Bunun yerine, bir yer tutucu cmdlet etkinliğini izlemek için tümleştirme modülü oluşturulur.  Bu cmdlet için hiçbir işlevsellik olsa da, yüklenecek kullanan herhangi bir dönüştürülmüş runbook sağlar.  Bu runbook, Azure Automation'da çalıştırılacak mümkün olmayacaktır ancak bunu değiştirebilmek yüklenebilir.

### <a name="integration-packs-that-cannot-be-converted"></a>Dönüştürülemeyen tümleştirme paketleri
OIT ile oluşturulmamış tümleştirme paketleri ile tümleştirme paketi dönüştürücü dönüştürülemez. Bu aracı şu anda dönüştürülemeyen Microsoft tarafından sağlanan bazı tümleştirme paketleri vardır.  Dönüştürülen bu tümleştirme paketleri sürümleri olmuştur [indirme için sağlanan](#system-center-orchestrator-integration-modules) böylece Azure Automation veya Service Management Automation yüklenebilir.

## <a name="standard-activities-module"></a>Standart etkinlikler Modülü
Orchestrator içeren bir dizi [standart etkinlikler](https://technet.microsoft.com/library/hh403832.aspx) bir tümleştirme paketinde bulunmayan, ancak birçok runbook'lar tarafından kullanılır.  Standart etkinlikler, bu etkinliklerin her biri için eşdeğer bir cmdlet'i içeren bir tümleştirme modülü modülüdür.  Standart etkinlik kullanan dönüştürülmüş runbook'ların içe aktarmadan önce Azure Otomasyonu'nda Bu tümleştirme modülü yüklemeniz gerekir.

Dönüştürülen runbook'ları hizmetinin yanı sıra, standart etkinlikler modülündeki cmdlet'ler Orchestrator ile tanıdık bir kişi tarafından Azure automation'da yeni runbook'ları oluşturmak için kullanılabilir.  Standart etkinliklerin tümünü işlevselliğini cmdlet'leri ile gerçekleştirilebilir, ancak farklı bir şekilde çalışabilir.  Dönüştürülen standart etkinlikler modülündeki cmdlet'ler, ilgili etkinlikler aynı çalışma ve aynı parametreleri kullanın.  Bu, var olan Orchestrator runbook yazarı, bir Azure Otomasyonu runbook'ları geçmeyi yardımcı olabilir.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator tümleştirme modülleri
Microsoft'un sağladığı [tümleştirme paketleri](https://technet.microsoft.com/library/hh295851.aspx) System Center bileşenleri ve diğer ürünler otomatikleştirmek için runbook'ları oluşturmak için.  Bu tümleştirme paketleri bazıları OIT üzerinde şu anda dayanır ancak şu anda bilinen sorunları nedeniyle tümleştirme modülleri dönüştürülemez.  [System Center Orchestrator tümleştirme modülleri](https://www.microsoft.com/download/details.aspx?id=49555) dönüştürülmüş Azure Automation ve Service Management Automation aktarılabilen Bu tümleştirme paketleri sürümleri içerir.  

Bu araç RTM sürümü tarafından dönüştürülebilir OIT tümleştirme paketi dönüştürücü ile temel tümleştirme paketlerinin güncelleştirilmiş sürümleri yayımlanır.  Dönüştürme etkinlikleri tümleştirme paketlerindeki OIT üzerinde temel almayan kullanarak runbook'ları size yardımcı olması için yönergeler de verilmektedir.

## <a name="runbook-converter"></a>Runbook dönüştürücü
Orchestrator runbook'ları Runbook dönüştürücü dönüştürür [grafik runbook'ları](automation-runbook-types.md#graphical-runbooks) Azure Automation'a alınabilir.  

Runbook dönüştürücü adlı bir cmdlet ile bir PowerShell modülü uygulanan **ConvertFrom-SCORunbook** dönüştürme gerçekleştiren.  Aracı yüklediğinizde, cmdlet yükleyen bir PowerShell oturumu için bir kısayol oluşturun.   

Orchestrator runbook dönüştürmek ve Azure Automation'a içeri aktarılması için temel işlemi aşağıda verilmiştir.  Aşağıdaki bölümlerde, aracı kullanma ve dönüştürülen runbook'larla çalışma hakkında ayrıntılı bilgi sağlayan.

1. Bir veya daha fazla runbook'ları Orchestrator'dan içeri aktarın.
2. Tümleştirme modülleri runbook'taki tüm etkinlikler için edinin.
3. Orchestrator runbook'ları dışa aktarılan dosyada dönüştürün.
4. Günlükleri dönüştürme doğrulamak ve tüm gerekli el ile gerçekleştirilen görevleri belirlemek için bilgileri gözden geçirin.
5. Azure Otomasyonu runbook'ları içeri aktarma dönüştürülür.
6. Azure Otomasyonu'nda tüm gerekli varlıkları oluşturun.
7. Gerekli tüm etkinlikler değiştirmek için Azure Otomasyonu'nda runbook düzenleyin.

### <a name="using-runbook-converter"></a>Runbook dönüştürücü kullanma
Sözdizimi **ConvertFrom-SCORunbook** aşağıdaki gibidir:

```powershell
ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>
```

* RunbookPath - dönüştürmek için runbook'ları içeren dışarı aktarma dosyasının yolu.
* Modül - runbook'lar etkinlikler içeren tümleştirme modülleri listesini virgülle ayrılmış.
* OutputFolder - grafik runbook'ları oluşturmak için klasör yolu dönüştürülür.

Aşağıdaki örnek komut, runbook'ları olarak adlandırılan bir dışarı aktarma dosyası dönüştürür **MyRunbooks.ois_export**.  Bu runbook'ları, Active Directory ve Data Protection Manager tümleştirme paketlerini kullanın.

```powershell
ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"
```

### <a name="log-files"></a>Günlük dosyaları
Runbook dönüştürücü aşağıdaki günlük dosyalarına dönüştürülen runbook ile aynı konumda oluşturur.  Dosyalar zaten mevcutsa, ardından bunların son dönüştürme bilgileriyle üzerine yazılacak.

| Dosya | İçindekiler |
|:--- |:--- |
| Runbook dönüştürücü - Progress.log |Ayrıntılı adımlar başarıyla dönüştürüldü her etkinlik için bilgi ve uyarı dönüştürülmedi her etkinlik için de dahil olmak üzere dönüştürme. |
| Runbook dönüştürücü - Summary.log |Tüm uyarılar dahil olmak üzere son dönüştürme özeti ve dönüştürülen runbook için gerekli bir değişken oluşturma gibi gerçekleştirmeniz gereken görevler'kurmak izleyin. |

### <a name="exporting-runbooks-from-orchestrator"></a>Runbook'ları Orchestrator'dan içeri aktarma
Bir dışarı aktarma dosyasından bir veya daha fazla runbook'ları içeren Orchestrator Runbook dönüştürücü çalışır.  Dışa aktarma dosyasında Orchestrator her runbook için karşılık gelen bir Azure Otomasyonu runbook'u oluşturun.  

Orchestrator'ı bir runbook'u dışarı aktarmak, Runbook Designer'da runbook adına sağ tıklayın ve seçin **dışarı**.  Bir klasördeki tüm runbook'lar dışarı aktarmak için seçin ve klasör adına sağ tıklayın **dışarı**.

### <a name="runbook-activities"></a>Runbook etkinlikleri
Runbook dönüştürücü Azure automation'da karşılık gelen bir etkinliği Orchestrator runbook'taki her etkinlik dönüştürür.  Dönüştürülemeyen söz konusu etkinlikler için uyarı metni runbook'ta yer tutucu etkinlik oluşturulur.  Dönüştürülen runbook Azure Automation'a içeri aktardıktan sonra tüm bu etkinliklerin gerekli işlevleri gerçekleştiren geçerli etkinlikler ile değiştirmeniz gerekir.

Tüm Orchestrator etkinlikleri [standart etkinlikler modül](#standard-activities-module) dönüştürülür.  Bu modülde ancak olmayan ve dönüştürülmez bazı standart Orchestrator etkinlikleri vardır.  Örneğin, **Platform olayı Gönder** olayı Orchestrator için belirli olduğundan, hiçbir Azure Otomasyonu eşdeğeri yok.

[İzleme etkinliklerin](https://technet.microsoft.com/library/hh403827.aspx) için bunları Azure automation'da eşdeğeri olduğundan dönüştürülmez.  Özel durum izleme etkinliklerin olan [dönüştürülen tümleştirme paketleri](#integration-pack-converter) için yer tutucu etkinlik dönüştürülür.

Herhangi bir etkinlikten bir [dönüştürülen tümleştirme paketi](#integration-pack-converter) modülle yolunu sağlarsanız dönüştürülecek **modülleri** parametresi.  System Center tümleştirme paketleri için kullanabileceğiniz [System Center Orchestrator tümleştirme modülleri](#system-center-orchestrator-integration-modules).

### <a name="orchestrator-resources"></a>Orchestrator kaynakları
Runbook dönüştürücü, runbook'ları, diğer Orchestrator kaynakları sayaçları, değişkenleri veya bağlantıları gibi değil yalnızca dönüştürür.  Sayaçlar, Azure Automation'da desteklenmez.  Değişkenleri ve bağlantıları desteklenir ancak bunları el ile oluşturmanız gerekir.  Günlük dosyaları runbook böyle kaynaklar gerektiriyorsa bildirir ve dönüştürülen runbook'un düzgün çalışması Azure Otomasyonu'nda oluşturmak için gereken karşılık gelen kaynakları belirtin.

Örneğin, bir runbook bir etkinlik belirli bir değerde doldurmak için bir değişken kullanabilir.  Dönüştürülen runbook etkinlik dönüştürmek ve Orchestrator değişkeniyle aynı ada sahip Azure Otomasyonu değişken varlığı belirtin.  Bu, Not edilir **Runbook dönüştürücü - Summary.log** dönüştürme işleminden sonra oluşturulan dosya.  Runbook kullanmadan önce bu değişken varlığı Azure Automation'da el ile oluşturmanız gerekir.

### <a name="input-parameters"></a>Giriş parametreleri
Orchestrator runbook'ları ile giriş parametrelerini kabul **verileri Başlat** etkinlik.  Bu etkinlik, dönüştürülen runbook içeriyorsa, ardından bir [giriş parametresi](automation-graphical-authoring-intro.md#runbook-input-and-output) Azure Otomasyonu'nda runbook etkinliğinde her parametre için oluşturulur.  A [iş akışı betiği denetim](automation-graphical-authoring-intro.md#activities) etkinlik alır ve her bir parametre döndüren dönüştürülmüş runbook oluşturulur.  Giriş parametresi kullanan herhangi bir runbook'taki etkinlikler, çıktı varlığındaki bu etkinlik bakın.

Bu strateji kullanılır nedeni, en iyi Orchestrator runbook işlevleri yansıtmak üzere olmasıdır.  Yeni grafik runbook'lar etkinlikler doğrudan giriş parametreleri kullanarak bir Runbook girdi veri kaynağı başvurmanız gerekir.

### <a name="invoke-runbook-activity"></a>Runbook'u Çağır etkinliğine
Orchestrator runbook'ları ile diğer runbook'ları başlatmak **Runbook'u Çağır** etkinlik. Dönüştürülen runbook bu etkinliği içeriyorsa ve **tamamlanmasını bekle** seçeneği ayarlanır ve ardından dönüştürülmüş runbook'ta bir runbook etkinliği için oluşturulur.  Varsa **tamamlanmasını bekle** seçeneği ayarlı değil, ardından kullanan bir iş akışı betiği etkinlik oluşturulur **başlangıç AzureAutomationRunbook** runbook'u başlatın.  Dönüştürülen runbook Azure Automation'a içeri aktardıktan sonra bu etkinlik etkinliğinde belirtilen bilgileri ile değiştirmeniz gerekir.

## <a name="related-articles"></a>İlgili makaleler
* [System Center 2012 - Orchestrator](https://technet.microsoft.com/library/hh237242.aspx)
* [Hizmet Yönetimi Otomasyonu](https://technet.microsoft.com/library/dn469260.aspx)
* [Karma Runbook çalışanı](automation-hybrid-runbook-worker.md)
* [Orchestrator standart etkinlikleri](https://technet.microsoft.com/library/hh403832.aspx)
* [İndirme System Center Orchestrator geçiş Araç Seti](https://www.microsoft.com/en-us/download/details.aspx?id=47323)

