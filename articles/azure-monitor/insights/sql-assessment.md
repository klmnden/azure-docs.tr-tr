---
title: Azure İzleyici ile SQL Server ortamınızın en iyi duruma getirme | Microsoft Docs
description: Azure İzleyici ile SQL sistem durumu denetimi çözümü, düzenli aralıklarla, ortamlarının sistem durumunu ve riskini değerlendirmek için kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: magoedte
ms.openlocfilehash: 94b23bc29c3c986e6a0cd74e0805b5d47ce35849
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62120656"
---
# <a name="optimize-your-sql-environment-with-the-sql-server-health-check-solution-in-azure-monitor"></a>Azure İzleyici'de SQL Server sistem durumu denetimi çözümü SQL ortamınızla en iyi duruma getirme

![SQL sistem durumunu denetleme simgesi](./media/sql-assessment/sql-assessment-symbol.png)

SQL sistem durumu denetimi çözümü, düzenli aralıklarla, server ortamlarının sistem durumunu ve riskini değerlendirmek için kullanabilirsiniz. Bu makalede, olası sorunlar için düzeltici eylemler yararlanabilmeniz çözümü yüklemesine yardımcı olur.

Bu çözüm, Önceliklendirilmiş öneriler dağıtılan sunucu altyapınızı belirli listesini sağlar. Öneriler altı arasında ayrılır, size hızlıca Yardım odak alanlarına odaklanmak riskini anlamak ve düzeltici eylemi gerçekleştirme.

Yapılan öneriler bilgi ve müşteri binlerce Microsoft mühendisinin göre kazanılan deneyimi temel alır. Her öneri, önerilen değişiklikleri uygulamak nasıl bir sorun için neden önemli ve ilgili yönergeleri sağlar.

Ücretsiz ve sağlam bir risk ortamında çalışan kendi ilerleme durumlarını izlemek ve kuruluşunuz için en önemli odak alanlarına odaklanmak seçebilirsiniz.

Çözüm ekledikten sonra değerlendirme tamamlandı, Özet bilgi odak alanlarına odaklanmak için gösterilir **SQL sistem durumu denetimi** ortamınızdaki altyapısı için Pano. Aşağıdaki bölümlerde bilgileri kullanmak üzere nasıl **SQL sistem durumu denetimi** Pano, burada görüntüleyebilir ve sonra gerçekleştirin Eylemler SQL Server altyapınız için önerilen.

![SQL sistem durumu denetimi kutucuk görüntüsü](./media/sql-assessment/sql-healthcheck-summary-tile.png)

![SQL sistem durumu denetimi panosunun görüntüsü](./media/sql-assessment/sql-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Önkoşullar

* SQL sistem durumu denetimi çözümü, Microsoft Monitoring Agent (yüklenmiş MMA) olan her bilgisayarda yüklü .NET Framework 4'ün desteklenen bir sürümünü gerektirir.  MMA aracısını, System Center 2016 - Operations Manager ve Operations Manager 2012 R2 ve Azure İzleyici tarafından kullanılır.  
* Çözüm, SQL Server 2012, 2014 ve 2016 sürümü destekler.
* Azure portalında Azure Market'ten SQL sistem durumu denetimi çözümü eklemek için bir Log Analytics çalışma alanı.  Çözümü yüklemek için yönetici veya Azure aboneliğinde katkıda bulunan olması gerekir.

  > [!NOTE]
  > Çözüm ekledikten sonra aracılar sunucularıyla AdvisorAssessment.exe dosyası eklenir. Yapılandırma verilerini okuyun ve ardından Azure İzleyici bulutta işleme için gönderilen. Mantıksal alınan verilere uygulanır ve bulut hizmeti olan verileri kaydeder.
  >
  >

SQL Server sunucularda sistem durumu denetimi gerçekleştirmek için bunlar bir aracı ve Azure İzleyici'de aşağıdaki desteklenen yöntemlerden birini kullanarak bağlantı gerektirir:

1. Yükleme [Microsoft Monitoring Agent (MMA)](../../azure-monitor/platform/agent-windows.md) sunucusu zaten System Center 2016 - Operations Manager veya Operations Manager 2012 R2 tarafından izlenen değil ise.
2. System Center 2016 - Operations Manager veya Operations Manager 2012 R2 ile izlenir ve yönetim grubu, Azure İzleyici ile tümleşiktir değil, sunucu verileri toplamak ve yine de hizmete iletmek için Log Analytics ile birden çok girişli olabilir Operations Manager tarafından izlenen.  
3. Operations Manager yönetim grubunuzun hizmeti ile tümleşikse, aksi takdirde, etki alanı denetleyicileri için veri toplama altındaki adımları izleyerek hizmet eklemek ihtiyacınız [aracıyla yönetilen bilgisayarlar ekleme](../../azure-monitor/platform/om-agents.md#connecting-operations-manager-to-azure-monitor) seçeneğini etkinleştirdikten sonra çalışma alanınızda çözümün.  

Aracı SQL sunucunuzda bir Operations Manager yönetim grubu için hangi raporların verileri toplayan kendi atanan yönetim sunucusuna iletir ve doğrudan yönetim sunucusundan Azure İzleyicisi'ne gönderilir.  Operations Manager veritabanları için veriler yazılmaz.  

SQL Server Operations Manager tarafından izleniyorsa, bir Operations Manager farklı çalıştır hesabı yapılandırmanız gerekir. Bkz: [Operations Manager farklı çalıştırma hesapları için Azure İzleyici](#operations-manager-run-as-accounts-for-log-analytics) aşağıda daha fazla bilgi için.

## <a name="sql-health-check-data-collection-details"></a>Veri koleksiyonu ayrıntıları SQL sistem durumunu denetle
SQL sistem durumu denetimi veri etkinleştirdiğiniz aracısını kullanarak aşağıdaki kaynaklardan toplar:

* Windows Yönetim Araçları (WMI)
* Kayıt defteri
* Performans sayaçları
* SQL Server dinamik yönetim sonuçlarını görüntüle

Veriler SQL Server'da toplanır ve her yedi günde Log Analytics'e iletilir.

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Log Analytics için Operations Manager farklı çalıştırma hesapları
Log Analytics'i Operations Manager aracısı ve yönetim grubu toplamak ve Log Analytics hizmetine veri göndermek için kullanır. Log Analytics yapılar sağlamak iş yükleri için yönetim paketleri temel değer-hizmetlerini ekleyin. Her iş yükü, etki alanı kullanıcı hesabı gibi farklı güvenlik bağlamı yönetim paketlerini çalıştırmak için iş yüküne özgü ayrıcalıkları gerektirir. Bir Operations Manager farklı çalıştır hesabını yapılandırarak kimlik bilgilerini sağlamanız gerekir.

SQL sistem durumu denetlemek için Operations Manager farklı çalıştır hesabı ayarlamak için aşağıdaki bilgileri kullanın.

### <a name="set-the-run-as-account-for-sql-health-check"></a>SQL sistem durumu denetlemek için farklı çalıştır hesabı ayarlayın
 SQL Server Yönetim Paketi kullanıyorsanız, bu farklı çalıştır yapılandırması kullanmanız gerekir.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>İşletim konsolunda SQL farklı çalıştır hesabı yapılandırmak için
> [!NOTE]
> Varsayılan olarak, yönetim paketindeki iş akışları yerel sistem hesabı bağlamında çalışır. Microsoft Monitoring Agent hizmetine doğrudan bağlı kullanarak yerine, doğrudan raporlama bir Operations Manager yönetim grubu için 1-5'i aşağıdaki adımları atlayın ve T-SQL veya PowerShell örneği çalıştırın, NT AUTHORITY\SYSTEM olarak belirterek Kullanıcı adı.
>
>

1. Operations Manager'da işletim konsolunu açın ve ardından **Yönetim**.
2. Altında **farklı çalıştır Yapılandırması**, tıklayın **profilleri**açın **SQL değerlendirmesi farklı çalıştır profili**.
3. Üzerinde **farklı çalıştır hesapları** sayfasında **Ekle**.
4. SQL Server için gerekli kimlik bilgilerini içeren bir Windows farklı çalıştır hesabı seçin veya **yeni** oluşturmak için.

   > [!NOTE]
   > Farklı Çalıştır hesap türü, Windows olması gerekir. Farklı Çalıştır hesabı da SQL Server örneklerini barındıran tüm Windows sunucularında yerel Administrators grubunun bir parçası olması gerekir.
   >
   >
5. **Kaydet**’e tıklayın.
6. Değiştirin ve sonra aşağıdaki T-SQL örnek, farklı çalıştır sistem durumu denetimi gerçekleştirmek hesabı için gerekli minimum izinleri vermek için her SQL Server örneğinde yürütün. Ancak, farklı çalıştır hesabı SQL Server örneği üzerinde sysadmin sunucu rolünün bir parçası ise bunu yapmanız gerekmez.

```
    ---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```

#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Windows PowerShell kullanarak SQL farklı çalıştır hesabı yapılandırmak için
Bir PowerShell penceresi açın ve kendi bilgilerinizle güncelleştirdikten sonra aşağıdaki betiği çalıştırın:

```
    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Öneriler nasıl önceliklendirilir anlama
Yaptığınız her öneri, öneri göreceli önemini tanımlayan bir ağırlık değeri verilir. Yalnızca en önemli on önerileri gösterilir.

### <a name="how-weights-are-calculated"></a>Ağırlıklar nasıl hesaplanır
Ağırlık belirlemeyi üç anahtar etkenlere göre toplam değerler şunlardır:

* *Olasılık* tanımlanan bir sorunun sorunlara neden. Daha yüksek bir olasılık öneri için daha büyük bir genel puan karşılık gelir.
* *Etkisi* kuruluşunuzdaki bir soruna neden olursa sorunun. Daha yüksek bir etkisi öneri için daha büyük bir genel puan karşılık gelir.
* *Çaba* öneriyi uygulamak için gereklidir. Daha yüksek bir çaba öneri için daha küçük bir genel puan karşılık gelir.

Her öneri için hesaplamasının her odak alanı için kullanılabilir toplam puanı yüzdesi olarak ifade edilir. Örneğin, güvenlik ve uyumluluk odak alanında bir öneri %5 puanı varsa, bu öneriyi uygulamak, genel güvenlik ve uyumluluk puanı tarafından %5 artırmış olursunuz.

### <a name="focus-areas"></a>Odak alanları
**Güvenlik ve Uyumluluk** -bu odak alanı olası güvenlik tehditlerini ve ihlallerinden, şirket ilkelerini ve uyumluluk teknik, yasal ve kanuni gereksinimleri için öneriler gösterir.

**Kullanılabilirlik ve iş sürekliliği** -hizmet kullanılabilirlik, dayanıklılık, altyapı ve işletme korumasına yönelik öneriler bu odak alanı gösterir.

**Performans ve ölçeklenebilirlik** -bu odak alanı kuruluşunuzun yardımcı olacak öneriler gösterir BT altyapısı arttıkça, BT ortamınızı geçerli performans gereksinimlerini karşıladığından ve değişen altyapı mümkün olduğundan emin olun gerekir.

**Yükseltme, geçiş ve dağıtım** -bu odak alanı yükseltme, geçirme ve SQL Server'ı mevcut altyapınızı dağıtmadan yardımcı olacak öneriler gösterir.

**İşlemler ve izleme** -BT işlemlerinizi kolaylaştırın, önleyici bakım uygulama ve performansı en üst düzeye yardımcı olacak öneriler bu odak alanı gösterir.

**Değişiklik ve yapılandırma yönetimi** -bu odak alanı değişiklikleri yok olumsuz altyapınızı etkileyen değişiklik denetim yordamları kurmak emin olun, gündelik işlemleri korumak amacıyla ve izlemek ve denetlemek için öneriler gösterir Sistem yapılandırması.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>% 100 her odak alanında puanlamak için indirmeyi amaçlamanız gerekir?
Olmayabilir. Öneriler bilgi ve müşteri binlerce Microsoft mühendisleri tarafından elde edilen deneyimler temel alır. Ancak, hiçbir iki sunucu altyapıları aynıdır ve öneriler için daha az veya uygun olabilir. Örneğin, bazı güvenlik önerilerini sanal makinelerinize Internet'e açık değildir, daha az ilgili olabilir. Bazı kullanılabilirlik önerisi düşük öncelikli işler için geçici veri toplama ve raporlama sağladığı hizmetler için daha uygun olabilir. Olgun bir işletmeye için önemli olan sorunları için bir başlangıç daha az önemli olabilir. Önceliklerinizle odak alanları tanımlamak ve puanları, zaman içinde nasıl değiştiğini ardından aramak isteyebilirsiniz.

Her öneri neden önemli olduğu ile ilgili yönergeler içerir. Öneri uygulandıktan verilen BT hizmetlerinizin doğasını ve kuruluşunuzun iş gereksinimlerini, sizin için uygun olup olmadığını değerlendirmek için bu yönergeleri kullanmanız gerekir.

## <a name="use-health-check-focus-area-recommendations"></a>Sistem durumu denetimi odak alanı önerileri kullanın
Azure İzleyici'de bir değerlendirme çözümü kullanabilmeniz için önce çözümü yüklenmiş olmalıdır.  Yüklendikten sonra SQL sistem durumu denetimi kutucuk kullanarak önerileri özetini görüntüleyebilirsiniz **genel bakış** Azure portalında Azure İzleyici sayfası.

Altyapınız ve ardından-ayrıntıya önerileri için Özet uyumluluk değerlendirmesi görüntüleyin.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için önerileri görüntüleme ve düzeltme eylemi
1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **İzleyici** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **İzleyici**'yi seçin.
3. İçinde **Insights** menüsünde, select bölümünü **daha fazla**.  
4. Üzerinde **genel bakış** sayfasında **SQL sistem durumu denetimi** Döşe.
5. Üzerinde **sistem durumu denetimi** sayfasında odak alanı dikey pencereleri biriyle özet bilgilerini gözden geçirin ve ardından bu odak alanı için önerileri görmek için tıklatın.
6. Herhangi bir odak alanı sayfalar üzerinde ortamınız için yapılan Önceliklendirilmiş öneriler görüntüleyebilirsiniz. Altında bir öneriye tıklayabilir **etkilenen nesneler** öneri neden yapılır hakkında ayrıntılı bilgi görüntülemek için.<br><br> ![SQL sistem durumu denetimi önerileri görüntüsü](./media/sql-assessment/sql-healthcheck-dashboard-02.png)<br>
7. Önerilen düzeltici eylemleri gerçekleştirebilirsiniz **önerilen eylemleri**. Öğe ele alındığında, önerilen eylemlerin yapıldığını ve uyumluluk puanınız artıracaktır sonraki değerlendirmeler kaydeder. Düzeltilen öğeler görünür olarak **geçirilen nesneleri**.

## <a name="ignore-recommendations"></a>Öneriler yoksay
Yok saymak için istediğiniz önerilerini varsa, Azure İzleyici, öneriler, değerlendirme sonuçlarında görüntülenmesini önlemek için kullanacağı bir metin dosyası oluşturabilirsiniz.

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Göz ardı eder önerileri tanımlamak için
1. Azure İzleyici menüden **günlükleri**.
2. Ortamınızdaki bilgisayarları için başarısız olan liste öneriler için aşağıdaki sorguyu kullanın.

    ```
    SQLAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation
    ```
    Günlük sorgusu gösteren ekran görüntüsü aşağıda verilmiştir:<br><br> ![başarısız önerileri](./media/sql-assessment/sql-assess-failed-recommendations.png)<br>

3. Yok saymak için istediğiniz önerilerini seçin. Sonraki yordamda Recommendationıd için değerleri kullanacaksınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Oluşturma ve bir IgnoreRecommendations.txt metin dosyası kullanma
1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Yapıştırın veya Azure İzleyici'nın ayrı bir satıra yoksay ve sonra dosyayı kaydedip kapatın istediğiniz her bir öneri için her Recommendationıd yazın.
3. Dosya, aşağıdaki klasörde, önerileri yok saymak için Azure İzleyici istediğiniz her bilgisayara yerleştirin.
   * Bilgisayarlarda Microsoft izleme (doğrudan veya Operations Manager üzerinden bağlı) aracısı ile - *SystemDrive*: \Program Monitoring Agent\Agent
   * Operations Manager yönetim sunucusunda - *SystemDrive*: \Program System Center 2012 R2\Operations Manager\Server
   * Operations Manager 2016 yönetim sunucusunda - *SystemDrive*: \Program System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Öneriler göz ardı edilir doğrulamak için
1. Sonraki değerlendirme çalışır, varsayılan olarak 7 günde zamanlanmış sonra belirtilen önerileri yoksayıldı işaretlenir ve değerlendirme Panoda görünmez.
2. Yoksayılan tüm önerilere listelemek için aşağıdaki günlük arama sorgularını kullanabilirsiniz.

    ```
    SQLAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation
    ```
3. Daha sonra yoksayılan önerileri görmek istediğinize karar verirseniz, IgnoreRecommendations.txt dosyaları kaldırın veya bunları RecommendationIDs kaldırabilirsiniz.

## <a name="sql-health-check-solution-faq"></a>SQL sistem durumu denetimi çözümü ile ilgili SSS
*Sistem durumu denetimi ne kadar sıklıkla çalışır?*

* Denetim yedi günde bir çalıştırılır.

*Onay çalışma sıklığını yapılandırmak için bir yol var mı?*

* Şu anda değil.

*Ben SQL sistem durumu denetimi çözümü ekledikten sonra başka bir sunucuya bulunmuşsa, denetlenecek?*

* Evet, yedi günde bir, daha sonra gelen iade edilmeden bulununca.

*Ne zaman bir sunucu kullanımdan alındıktan hemen ise sistem durumu denetiminin dışında kaldırılacak?*

* Bir sunucu 3 haftaya kadar veri göndermez, kaldırılır.

*Veri koleksiyonu yapan işlemin adı nedir?*

* AdvisorAssessment.exe

*Ne kadar toplanacak veri için sürer?*

* Gerçek veri toplama sunucusuna yaklaşık 1 saat sürer. Bu çok sayıda SQL örnekleri veya veritabanlarına sahip sunucularda daha uzun sürebilir.

*Ne tür verilere toplanır?*

* Aşağıdaki veri türlerini toplanır:
  * WMI
  * Kayıt defteri
  * Performans sayaçları
  * SQL Dinamik Yönetim görünümlerini (DMV).

*Verileri toplandığında yapılandırmak için bir yol var mı?*

* Şu anda değil.

*Farklı Çalıştır hesabı yapılandırmak neden gerekiyor?*

* SQL Server için az sayıda SQL sorguları çalıştırılır. Farklı Çalıştır hesabı SQL VIEW SERVER STATE izni olan sırayla çalışmak üzere için kullanılmalıdır.  Ayrıca, WMI Sorgu için yerel yönetici kimlik bilgileri gereklidir.

*Neden yalnızca ilk 10 önerileri görüntülensin mi?*

* Büyük kapsamlı bir liste görevlerin vermek yerine, Önceliklendirilmiş öneriler adresleme odaklanmak öneririz. Bunları adres sonra ek öneriler kullanılabilir hale gelir. Ayrıntılı listesini görmek isterseniz, Log Analytics günlük araması'nı kullanarak tüm önerileri görüntüleyebilirsiniz.

*Bir öneri yok saymak için bir yol var mı?*

* Evet, bkz: [önerileri yoksay](#ignore-recommendations) yukarıdaki bölümde.

## <a name="next-steps"></a>Sonraki adımlar
* [Oturum sorguları](../log-query/log-query-overview.md) ayrıntılı veri SQL sistem durumunu denetleyin ve önerileri çözümleme hakkında bilgi edinmek için.
