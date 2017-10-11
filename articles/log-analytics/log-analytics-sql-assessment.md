---
title: "SQL Server ortamınızın Azure günlük analizi ile en iyi duruma getirme | Microsoft Docs"
description: "Azure günlük analizi ile risk ve SQL server ortamlarınızın durumunu düzenli aralıklarla değerlendirmek için SQL değerlendirme çözümü kullanabilirsiniz."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2aed3315fe60ace46dfb4176dc13aa417257b0c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-sql-server-environment-with-the-sql-assessment-solution-in-log-analytics"></a>SQL Server ortamınızın günlük analizi SQL değerlendirmesi çözümde ile en iyi duruma getirme

![SQL değerlendirmesi simgesi](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

SQL değerlendirme çözümü, risk ve sunucu ortamlarınızın durumunu düzenli aralıklarla değerlendirmek için kullanabilirsiniz. Bu makalede, olası sorunlar için düzeltme eylemleri yararlanabilmeniz çözümü yüklemenize yardımcı olur.

Bu çözüm önerileri dağıtılan sunucu altyapınızı belirli öncelikli listesi sağlar. Öneriler altı arasında ayrılır hızla yardımcı odak alanlarına riski anlamak ve düzeltme eylemlerini gerçekleştirin.

Önerilerin bilgi ve müşteri ziyaretleriniz binlerce Microsoft mühendisleri tarafından elde edilen deneyimlerden dayanır. Her bir öneri, bir sorun için neden önemli ve önerilen değişiklikleri uygulamak nasıl hakkında rehberlik sağlar.

Kuruluşunuz için en önemli ve ücretsiz ve sağlam bir risk ortam çalıştıran doğru ilerleme durumunuzu izlemenize odak alanlarına seçebilirsiniz.

Çözüm ekledik ve bir değerlendirme tamamlanan, Özet sonra odak alanlarına odaklanmak için bilgi gösterilir **SQL değerlendirmesi** Panosu ortamınızdaki altyapısı için. Aşağıdaki bölümlerde bilgileri üzerinde nasıl kullanacağınızı **SQL değerlendirmesi** Pano, burada görüntüleyebilir ve ardından uygulayın önerilen eylemler için SQL server altyapınızı.

![SQL değerlendirmesi döşeme görüntüsü](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![SQL değerlendirmesi Pano görüntüsü](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a>Yükleme ve çözüm yapılandırılıyor
SQL Server Standard, Developer ve Enterprise sürümleri için şu anda desteklenen tüm sürümleri ile SQL değerlendirmesi çalışır.

Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın.

* Aracıları SQL Server'ın yüklü olması sunucularda yüklü olması gerekir.
* SQL değerlendirme çözümü bir OMS aracısı olan her bilgisayarda yüklü .NET Framework 4'ün desteklenen bir sürümünü gerektirir.
* Çözümü yüklemek için kullanıcı bir yönetici ya da Azure aboneliğini katkıda bulunan Azure portal kullanırken olmalıdır. Buna ek olarak, kullanıcının OMS portalındaki OMS çalışma alanı katılımcısı veya yöneticisi rolünün üyesi olması gerekir.
* Operations Manager aracısı SQL değerlendirmesi ile kullanırken, bir Operations Manager Run-As hesabı kullanmanız gerekir. Bkz: [Operations Manager Çalıştır hesapları için OMS](#operations-manager-run-as-accounts-for-oms) altında daha fazla bilgi için.

  > [!NOTE]
  > MMA aracı Operations Manager Run-As hesaplarını desteklemez.
  >
  >
* Açıklanan işlemi kullanarak, OMS çalışma SQL değerlendirme çözümü eklemek [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). Başka bir yapılandırma işlemi gerekmez.

> [!NOTE]
> Çözüm ekledikten sonra aracıları sunucularıyla AdvisorAssessment.exe dosyası eklenir. Yapılandırma verilerini okuma ve işleme için bulutta OMS hizmetine gönderilir. Mantığı alınan verilere uygulanır ve bulut hizmeti verilerini kaydeder.

## <a name="sql-assessment-data-collection-details"></a>SQL değerlendirmesi veri toplama ayrıntıları
SQL değerlendirmesi WMI verilerini, kayıt defteri verilerini, performans verilerini ve etkinleştirdiğiniz aracıları kullanarak SQL Server dinamik yönetim görünümünü sonuçları toplar.

Aşağıdaki tablo, Operations Manager (SCOM) gerekli olup olmadığını ve nasıl aracılar için veri toplama yöntemleri yapılandırmayı gösterir. genellikle veri bir aracı tarafından toplanır.

| Platform | Doğrudan Aracısı | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  | &#8226; |7 gün |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Operations Manager hesapları OMS için Çalıştır
OMS günlük analizi toplamak ve OMS hizmetine veri göndermek için Operations Manager aracısı ve yönetim grubu kullanır. Bağlı Yönetim paketleri sağlamak iş yükleri için OMS derlemeleri değeri-hizmetlerini ekleyin. Her iş yükü, bir etki alanı hesabı gibi farklı güvenlik bağlamında yönetim paketlerini çalıştırmak için iş yüküne özgü ayrıcalıkları gerektirir. Bir Operations Manager farklı çalıştır hesabı yapılandırarak kimlik bilgilerini sağlamanız gerekir.

SQL değerlendirmesi için Operations Manager farklı çalıştırma hesabını ayarlamak için aşağıdaki bilgileri kullanın.

### <a name="set-the-run-as-account-for-sql-assessment"></a>SQL değerlendirmesi için farklı çalıştır hesabı ayarlama
 SQL Server Yönetim Paketi zaten kullanıyorsanız, bu farklı çalıştır hesabı kullanmanız gerekir.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>İşlemler konsolunda SQL farklı çalıştır hesabı yapılandırmak için
> [!NOTE]
> SCOM Aracısı'nı yerine OMS doğrudan Aracısı'nı kullanıyorsanız, Yönetim Paketi her zaman yerel sistem hesabı bağlamında çalışır. Adım 1-aşağıdaki 5 atlayın ve T-SQL veya Powershell örnek, NT AUTHORITY\SYSTEM kullanıcı adı olarak belirterek çalıştırın.
>
>

1. Operations Manager işletim konsolunu açın ve ardından **Yönetim**.
2. Altında **Çalıştır Yapılandırması**, tıklatın **profilleri**, açarak **OMS SQL değerlendirmesi farklı çalıştır profili**.
3. Üzerinde **farklı çalıştır hesapları** sayfasında, **Ekle**.
4. SQL Server için gereken kimlik bilgilerini içeren bir Windows farklı çalıştır hesabı seçin veya tıklatın **yeni** oluşturmak için.

   > [!NOTE]
   > Farklı Çalıştır hesap türü Windows olması gerekir. Farklı Çalıştır hesabı, aynı zamanda SQL Server örneklerini barındıran tüm Windows sunucularında yerel Administrators grubunun bir parçası olması gerekir.
   >
   >
5. **Kaydet** düğmesine tıklayın.
6. Değiştirin ve sonra aşağıdaki T-SQL örneği her SQL Server SQL değerlendirmesi gerçekleştirmek için farklı çalıştır hesabı için gerekli minimum izinleri vermek için örneğinde yürütün. Ancak, bir farklı çalıştır hesabı SQL Server örnekleri üzerindeki sysadmin sunucu rolünün bir parçası ise bu yapmanız gerekmez.

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
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a>Windows PowerShell kullanarak SQL farklı çalıştır hesabını yapılandırmak için
Bir PowerShell penceresi açın ve bilgilerinizle güncelleştirdikten sonra aşağıdaki betiği çalıştırın:

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a>Önerilerin nasıl önceliklendirilir anlama
Yapılan her öneri öneri göreceli önemini tanımlayan bir ağırlıklı değer verilir. Yalnızca on en önemli öneriler gösterilir.

### <a name="how-weights-are-calculated"></a>Ağırlıkları nasıl hesaplanır
Ağırlık belirlemeyi üç anahtar faktörlerini temel alarak toplam değerler şunlardır:

* *Olasılık* tanımlanan bir sorunun sorunlara neden. Daha yüksek olasılık öneri için daha büyük bir genel puan karşılık gelir.
* *Etkisi* kuruluşunuzdaki bir soruna neden olursa sorun. Daha yüksek bir etkisi öneri için daha büyük bir genel puan karşılık gelir.
* *Çaba* öneriyi uygulamak için gereklidir. Daha yüksek çaba öneri için daha küçük bir genel puan karşılık gelir.

Her öneri ağırlıklı her odak alanı için kullanılabilir toplam puanı yüzdesi olarak ifade edilir. Güvenlik ve uyumluluk odak alanında bir öneri %5 puanı varsa, örneğin, bu öneri uygulama, genel güvenlik ve uyumluluk puan tarafından %5 artacaktır.

### <a name="focus-areas"></a>Odak alanları
**Güvenlik ve Uyumluluk** -bu odak alanı olası güvenlik tehditlerini ve ihlallerinden, şirket ilkelerini ve teknik ve yasal uyumluluk gereksinimleri için öneriler gösterir.

**Kullanılabilirlik ve iş sürekliliği** -bu odaklanılan alan hizmet kullanılabilirliği, altyapı ve iş koruması dayanıklılık için öneriler gösterir.

**Performans ve ölçeklenebilirlik** -bu odak alanı kuruluşunuzun yardımcı olacak öneriler gösterir BT altyapısı arttıkça, BT ortamınız geçerli performans gereksinimlerini karşıladığından ve altyapı değiştirmeye yanıt verebilmesini olduğundan emin olun gerekir.

**Yükseltme, geçiş ve dağıtım** -bu odak alanı, yükseltme, geçirme ve SQL Server mevcut altyapınızda dağıtmanıza yardımcı olacak öneriler gösterir.

**İşlemler ve izleme** -bu odak alanı BT işlemlerinizi kolaylaştırır, önleyici bakım uygulamak ve performansı en üst düzeye çıkarmanıza yardımcı olacak öneriler gösterir.

**Değişiklik ve yapılandırma yönetimi** -bu odak alanı günlük işlemlerini korumak, değişiklikleri yok olumsuz altyapınızı etkiler, değişiklik denetim yordamları kurmak emin olun yardımcı olmak için izleme ve denetim için öneriler gösterir Sistem yapılandırması.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Her odaklanılan alan % 100 puanlı hedefleyin?
Olmayabilir. Öneriler bilgi ve müşteri ziyaretleriniz binlerce arasında Microsoft mühendisleri tarafından elde edilen deneyimleri temel alır. Ancak, iki sunucu altyapılar aynıdır ve özel öneriler için daha az veya uygun olabilir. Örneğin, bazı güvenlik önerileri sanal makinelerinizi Internet'e açık değildir, daha az ilgili olabilir. Bazı kullanılabilirlik öneriler düşük öncelik geçici veri toplama ve raporlama sağladığı hizmetler için daha az uygun olmayabilir. Olgun bir iş için önemli olan sorunları bir başlangıcından daha az önemli olabilir. Hangi odak alanların önceliklerinizden olduğunu belirlemek ve, puanları zamanla nasıl değiştiğini adresindeki Ara isteyebilirsiniz.

Her öneri neden önemli olduğu hakkında yönergeler içerir. Öneri uygulama verilen BT hizmetlerinizi yapısını ve kuruluşunuzun iş gereksinimlerini sizin için uygun olup olmadığını değerlendirmek için bu yönergeleri kullanmanız.

## <a name="use-assessment-focus-area-recommendations"></a>Değerlendirme odak alanı önerileri kullanın
OMS içinde bir değerlendirme çözümü kullanmadan önce çözümü yüklenmiş olması gerekir. Daha fazla bilgi için çözümleri yükleme hakkında bkz [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). Yüklendikten sonra OMS genel bakış sayfasında SQL değerlendirmesi döşeme kullanarak önerileri özetini görüntüleyebilirsiniz.

Altyapınız ve ardından-ayrıntıya önerileri için özetlenmiş uyumluluk değerlendirmesi görüntüleyin.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için öneriler görüntülemek ve düzeltici işlemleri için
1. Üzerinde **genel bakış** sayfasında, **SQL değerlendirmesi** döşeme.
2. Üzerinde **SQL değerlendirmesi** sayfasında odak alanı Kanatlar birinde özet bilgilerini inceleyin ve sonra bu odak alanı için öneriler görüntülemek için tıklatın.
3. Odak alanı sayfaları hiçbirinde ortamınız için öncelikli önerilerin görüntüleyebilirsiniz. Önerinin altında tıklatın **etkilenen nesneleri** öneri neden yapılan hakkında ayrıntıları görüntülemek için.  
    ![SQL değerlendirmesi önerileri görüntüsü](./media/log-analytics-sql-assessment/sql-assess-focus.png)
4. Önerilen düzeltici eylemleri gerçekleştirebilirsiniz **önerilen eylemleri**. Öğe ele, önerilen eylemler gerçekleştirilen ve uyumluluk puan artıracaktır sonraki değerlendirmeleri kaydeder. Düzeltilmiş öğeler görünür olarak **geçirilen nesneleri**.

## <a name="ignore-recommendations"></a>Öneriler yoksay
Yoksay istediğiniz önerileri varsa, OMS önerileri değerlendirme sonuçlarında görünmesini engellemek için kullanacağı bir metin dosyası oluşturabilirsiniz.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Göz ardı eder önerileri tanımlamak için
1. Sunucunuzdan çalışma alanınıza oturum açın ve günlük arama açın. Ortamınızdaki bilgisayarları için başarısız olan liste önerileri için aşağıdaki sorguyu kullanın.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   Günlük arama sorgusu gösteren ekran görüntüsü şöyledir: ![önerileri başarısız oldu](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)
2. Yoksay istediğiniz önerileri seçin. Sonraki yordamda Recommendationıd için değerleri kullanacaksınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Oluşturma ve bir IgnoreRecommendations.txt metin dosyası kullanma
1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Her Recommendationıd ayrı bir satırda yoksay ve sonra dosyayı kaydedip kapatın için OMS istediğiniz her bir öneri için yazın veya yapıştırın.
3. Dosya aşağıdaki klasörde önerileri yoksaymak için OMS istediğiniz her bilgisayara yerleştirin.
   * Microsoft Monitoring (doğrudan veya Operations Manager aracılığıyla bağlı) Agent - olan bilgisayarlarda *SystemDrive*: \Program izleme Agent\Agent
   * Operations Manager yönetim sunucusunda - *SystemDrive*: \Program System Center 2012 R2\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Öneriler göz ardı edilir doğrulamak için
1. Sonraki değerlendirme çalıştığında, varsayılan olarak 7 günde zamanlanmış sonra belirtilen önerileri yoksayıldı işaretlenir ve değerlendirme Panoda görünmez.
2. Tüm yoksayılan öneriler listelemek için aşağıdaki günlük arama sorgularını kullanabilirsiniz.

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. Daha sonra yoksayılan önerileri görmek istediğiniz karar verirseniz, tüm IgnoreRecommendations.txt dosyaları silin veya bunları RecommendationIDs kaldırabilirsiniz.

## <a name="sql-assessment-solution-faq"></a>SQL değerlendirme çözümü hakkında SSS
*Sıklıkla bir değerlendirme çalışıyor mu?*

* Değerlendirme 7 günde bir çalışır.

*Ne sıklıkta değerlendirme çalıştıran yapılandırmak için yolu var mı?*

* Şu anda değil.

*T SQL değerlendirme çözümü ekledikten sonra başka bir sunucuya belirlediyseniz değerlendirileceğini?*

* Evet, bunu daha sonra gelen her 7 günde değerlendirildiği bulunduktan sonra.

*Ne zaman bir sunucu kullanımdan alındığında değerlendirme kaldırılacak?*

* Bir sunucu için 3 hafta veri göndermez, kaldırılır.

*Veri toplama mu işlemin adı nedir?*

* AdvisorAssessment.exe

*Ne kadar süreyle verilerinin toplanmasını sürer?*

* Gerçek veri toplama sunucusundaki yaklaşık 1 saat sürer. Bu çok sayıda SQL örnekleri veya veritabanlarına sahip sunucularda uzun sürebilir.

*Hangi türde veri toplanır?*

* Aşağıdaki veri türlerini toplanır:
  * WMI
  * Kayıt defteri
  * Performans sayaçları
  * SQL Dinamik Yönetim görünümlerini (DMV).

*Verileri toplandığında yapılandırmak için yolu var mı?*

* Şu anda değil.

*Bir farklı çalıştır hesabı yapılandırmak neden var mı?*

* SQL Server için SQL sorguları az sayıda çalıştırılır. Bir farklı çalıştır hesabı SQL VIEW SERVER STATE izni olan çalışmasını sırayla kullanılması gerekir.  Ayrıca, WMI sorgusu için yerel yönetici kimlik bilgileri gereklidir.

*Neden yalnızca ilk 10 önerileri görüntülemek?*

* Görevler kapsamlı bir zorlamayı listesi vermek yerine, Önceliklendirilmiş önerileri adresleme odaklanmak öneririz. Bunları çözün sonra ek öneriler kullanılabilir hale gelir. Ayrıntılı listesini görmek isterseniz, OMS günlük arama özelliğini kullanarak tüm önerileri görüntüleyebilirsiniz.

*Bir öneri yoksaymak için yolu var mı?*

* Evet, bkz: [önerileri yoksay](#ignore-recommendations) yukarıdaki bölümde.

## <a name="next-steps"></a>Sonraki adımlar
* [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı SQL Değerlendirme verileri ve öneriler görüntülemek için.
