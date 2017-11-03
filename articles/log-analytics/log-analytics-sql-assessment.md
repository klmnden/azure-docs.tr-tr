---
title: "SQL Server ortamınızın Azure günlük analizi ile en iyi duruma getirme | Microsoft Docs"
description: "Azure günlük analizi ile risk ve ortamlarınızın durumunu düzenli aralıklarla değerlendirmek için SQL sistem durumu denetimi çözüm kullanabilirsiniz."
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
ms.date: 10/27/2017
ms.author: magoedte;banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ec66c322550ac3a7729dc1fddc8c026fb4ec1895
ms.sourcegitcommit: b83781292640e82b5c172210c7190cf97fabb704
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="optimize-your-sql-environment-with-the-sql-server-health-check-solution-in-log-analytics"></a>SQL ortamınızı günlük analizi SQL Server sistem durumu denetimi çözümde ile en iyi duruma getirme

![SQL sistem durumu denetimi simgesi](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

SQL sistem durumu denetimi çözüm, risk ve sunucu ortamlarınızın durumunu düzenli aralıklarla değerlendirmek için kullanabilirsiniz. Bu makalede, olası sorunlar için düzeltme eylemleri yararlanabilmeniz çözümü yüklemenize yardımcı olur.

Bu çözüm önerileri dağıtılan sunucu altyapınızı belirli öncelikli listesi sağlar. Öneriler altı arasında ayrılır hızla yardımcı odak alanlarına riski anlamak ve düzeltme eylemlerini gerçekleştirin.

Önerilerin bilgi ve müşteri ziyaretleriniz binlerce Microsoft mühendisleri tarafından elde edilen deneyimlerden dayanır. Her bir öneri, bir sorun için neden önemli ve önerilen değişiklikleri uygulamak nasıl hakkında rehberlik sağlar.

Kuruluşunuz için en önemli ve ücretsiz ve sağlam bir risk ortam çalıştıran doğru ilerleme durumunuzu izlemenize odak alanlarına seçebilirsiniz.

Çözüm ekledik ve bir değerlendirme tamamlanan, Özet sonra odak alanlarına odaklanmak için bilgi gösterilir **SQL sistem durumu denetimi** Panosu ortamınızdaki altyapısı için. Aşağıdaki bölümlerde bilgileri üzerinde nasıl kullanacağınızı **SQL sistem durumu denetimi** Pano, burada görüntüleyebilir ve ardından uygulayın önerilen eylemler için SQL Server altyapınızı.

![Döşeme SQL durumu Denetim görüntüsü](./media/log-analytics-sql-assessment/sql-healthcheck-summary-tile.png)

![Pano SQL durumu Denetim görüntüsü](./media/log-analytics-sql-assessment/sql-healthcheck-dashboard-01.png)

## <a name="prerequisites"></a>Ön koşullar

* SQL sistem durumu denetimi çözüm Microsoft İzleme Aracısı (yüklenmiş MMA) olan her bilgisayarda yüklü .NET Framework 4'ün desteklenen bir sürüm gerektirir.  MMA Aracısı System Center 2016 - Operations Manager ve Operations Manager 2012 R2 ile günlük analizi hizmeti tarafından kullanılır.  
* SQL Server 2012, 2014 ve 2016 sürüm çözümünü destekler.
* Azure portalında Azure Marketi'nden SQL sistem durumu denetimi çözümü eklemek için bir günlük analizi çalışma alanı.  Çözümü yüklemek için bir yönetici veya katkıda bulunan Azure aboneliğiniz olmalıdır. 

  > [!NOTE]
  > Çözüm ekledikten sonra aracıları sunucularıyla AdvisorAssessment.exe dosyası eklenir. Yapılandırma verileri okumak ve işlemek için günlük analizi hizmet olarak bulutta sonra gönderilir. Mantığı alınan verilere uygulanır ve bulut hizmeti verilerini kaydeder.
  >
  >

SQL Server sunucularda sistem durumu denetimi gerçekleştirmek için bunlar bir aracı ve günlük analizi aşağıdaki desteklenen yöntemlerden birini kullanarak bağlantı gerektirir:

1. Yükleme [Microsoft İzleme Aracısı'nı (MMA)](log-analytics-windows-agents.md) sunucu zaten System Center 2016 - Operations Manager veya Operations Manager 2012 R2 tarafından izleniyorsa değil.
2. System Center 2016 - Operations Manager veya Operations Manager 2012 R2 ile izlenir ve yönetim grubu günlük analizi hizmeti ile tümleşik olmayan, sunucu veri toplamak ve hala Hizmeti'ne iletmek için günlük analizi ile çok konaklı olabilir Operations Manager tarafından izlenen.  
3. Operations Manager yönetim grubunuzu hizmeti ile tümleşik çalışıyorsa, aksi takdirde, veri toplama için etki alanı denetleyicileri altındaki adımları izleyerek hizmeti tarafından eklemeniz gerekir [aracıyla yönetilen bilgisayarlar eklemek](log-analytics-om-agents.md#connecting-operations-manager-to-oms) etkinleştirdikten sonra Çalışma alanınızı çözümde.  

Hangi raporları bir Operations Manager yönetim grubu için veri toplar, SQL Server aracısında kendi atanmış yönetim sunucusuna iletir ve doğrudan yönetim sunucusundan günlük analizi hizmetine gönderilir.  Verileri Operations Manager veritabanları yazılmaz.  

SQL Server Operations Manager tarafından izlenen, bir Operations Manager farklı çalıştır hesabı yapılandırmanız gerekir. Bkz: [Operations Manager Çalıştır hesapları için günlük analizi](#operations-manager-run-as-accounts-for-log-analytics) altında daha fazla bilgi için.

## <a name="sql-health-check-data-collection-details"></a>Veri toplama ayrıntıları SQL sistem durumunu denetleyin
SQL sistem durumu denetimi etkinleştirdiğiniz aracısını kullanarak aşağıdaki kaynaklardan toplar: 

* Windows Yönetim Araçları (WMI) 
* Kayıt defteri 
* Performans sayaçları
* SQL Server dinamik yönetim görünümünü sonuçları 

Veri SQL Server'da toplanır ve günlük analizi için her yedi günde iletilir.

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Operations Manager hesapları günlük analizi için Çalıştır
Günlük analizi toplamak ve günlük analizi hizmetine veri göndermek için Operations Manager aracısı ve yönetim grubu kullanır. Günlük analizi derlemeleri sağlamak iş yükleri için yönetim paketlerinin bağlı değer-hizmetlerini ekleyin. Her iş yükü, bir etki alanı kullanıcı hesabı gibi farklı güvenlik bağlamında yönetim paketlerini çalıştırmak için iş yüküne özgü ayrıcalıkları gerektirir. Bir Operations Manager farklı çalıştır hesabı yapılandırarak kimlik bilgilerini sağlamanız gerekir.

SQL sistem durumu denetlemek için Operations Manager farklı çalıştır hesabı ayarlamak için aşağıdaki bilgileri kullanın.

### <a name="set-the-run-as-account-for-sql-health-check"></a>SQL sistem durumu denetlemek için farklı çalıştır hesabı ayarlama
 SQL Server Yönetim Paketi zaten kullanıyorsanız, bu farklı çalıştır yapılandırması kullanmalıdır.

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a>İşlemler konsolunda SQL farklı çalıştır hesabı yapılandırmak için
> [!NOTE]
> Varsayılan olarak Yönetim Paketi iş akışlarında, yerel sistem hesabı bağlamında çalışır. Microsoft izleme aracısı hizmetine doğrudan bağlı kullanmak yerine, doğrudan rapor bir Operations Manager yönetim grubu için 1-5'i aşağıdaki adımları atlayın ve durumunda T-SQL veya PowerShell örnek çalıştırmak, NT AUTHORITY\SYSTEM olarak belirtme Kullanıcı adı.
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
6. Değiştirin ve sonra aşağıdaki T-SQL örneği Çalıştır sistem durumu denetimi gerçekleştirmek hesabı için gerekli minimum izinleri vermek için her SQL Server örneğinde yürütün. Ancak, bir farklı çalıştır hesabı SQL Server örneği üzerinde sysadmin sunucu rolünün bir parçası ise bu yapmanız gerekmez.

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

## <a name="use-health-check-focus-area-recommendations"></a>Sistem durumu denetimi odak alanı önerileri kullanın
Günlük analizi değerlendirme çözümünü kullanmadan önce çözümü yüklenmiş olması gerekir.  Yüklendikten sonra çözüm sayfasında Azure Portalı'ndaki SQL sistem durumu denetimi döşeme kullanarak önerileri özetini görüntüleyebilirsiniz.

Altyapınız ve ardından-ayrıntıya önerileri için özetlenmiş uyumluluk değerlendirmesi görüntüleyin.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için öneriler görüntülemek ve düzeltici işlemleri için
1. Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com). 
2. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**.
3. Günlük analizi abonelikleri bölmesinde, bir çalışma alanını seçin ve ardından **OMS portalı** döşeme.  
4. Üzerinde **genel bakış** sayfasında, **SQL sistem durumu denetimi** döşeme. 
5. Üzerinde **sistem durumu denetimi** sayfasında odak alanı Kanatlar birinde özet bilgilerini inceleyin ve sonra bu odak alanı için öneriler görüntülemek için tıklatın.
6. Odak alanı sayfaları hiçbirinde ortamınız için öncelikli önerilerin görüntüleyebilirsiniz. Önerinin altında tıklatın **etkilenen nesneleri** öneri neden yapılan hakkında ayrıntıları görüntülemek için.<br><br> ![SQL sistem durumu denetimi önerileri görüntüsü](./media/log-analytics-sql-assessment/sql-healthcheck-dashboard-02.png)<br>
7. Önerilen düzeltici eylemleri gerçekleştirebilirsiniz **önerilen eylemleri**. Öğe ele, önerilen eylemler gerçekleştirilen ve uyumluluk puan artıracaktır sonraki değerlendirmeleri kaydeder. Düzeltilmiş öğeler görünür olarak **geçirilen nesneleri**.

## <a name="ignore-recommendations"></a>Öneriler yoksay
Yoksay istediğiniz önerileri varsa, OMS önerileri değerlendirme sonuçlarında görünmesini engellemek için kullanacağı bir metin dosyası oluşturabilirsiniz.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-will-ignore"></a>Göz ardı eder önerileri tanımlamak için
1. Günlük analizi çalışma sayfasında seçilen çalışma alanınız için Azure portalında tıklatın **günlük arama** döşeme.
2. Ortamınızdaki bilgisayarları için başarısız olan liste önerileri için aşağıdaki sorguyu kullanın.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Failed | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), sonra da yukarıdaki sorguda şu şekilde değiştirir.
    >
    > `SQLAssessmentRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

    Günlük arama sorgusu gösteren ekran görüntüsü aşağıda verilmiştir:<br><br> ![başarısız önerileri](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)<br>

3. Yoksay istediğiniz önerileri seçin. Sonraki yordamda Recommendationıd için değerleri kullanacaksınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Oluşturma ve bir IgnoreRecommendations.txt metin dosyası kullanma
1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Her Recommendationıd ayrı bir satırda yoksay ve sonra dosyayı kaydedip kapatın için günlük analizi istediğiniz her bir öneri için yazın veya yapıştırın.
3. Dosya aşağıdaki klasörde önerileri yoksaymak için günlük analizi istediğiniz her bilgisayara yerleştirin.
   * Microsoft Monitoring (doğrudan veya Operations Manager aracılığıyla bağlı) Agent - olan bilgisayarlarda *SystemDrive*: \Program izleme Agent\Agent
   * Operations Manager yönetim sunucusunda - *SystemDrive*: \Program System Center 2012 R2\Operations Manager\Server
   * Operations Manager 2016 yönetim sunucusundaki - *SystemDrive*: \Program System Center 2016\Operations Manager\Server

### <a name="to-verify-that-recommendations-are-ignored"></a>Öneriler göz ardı edilir doğrulamak için
1. Sonraki değerlendirme çalıştığında, varsayılan olarak 7 günde zamanlanmış sonra belirtilen önerileri yoksayıldı işaretlenir ve değerlendirme Panoda görünmez.
2. Tüm yoksayılan öneriler listelemek için aşağıdaki günlük arama sorgularını kullanabilirsiniz.

    ```
    Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), sonra da yukarıdaki sorguda şu şekilde değiştirir.
    >
    > `SQLAssessmentRecommendation | where RecommendationResult == "Ignored" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

3. Daha sonra yoksayılan önerileri görmek istediğiniz karar verirseniz, tüm IgnoreRecommendations.txt dosyaları silin veya bunları RecommendationIDs kaldırabilirsiniz.

## <a name="sql-health-check-solution-faq"></a>SQL sistem durumu denetimi çözüm SSS
*Sıklıkla bir sistem durumu denetimi çalışıyor mu?*

* Onay yedi günde bir çalışır.

*Ne sıklıkta denetimi çalıştırılır yapılandırmak için yolu var mı?*

* Şu anda değil.

*T SQL sistem durumu denetimi çözüm ekledikten sonra başka bir sunucuya bulunmuşsa, denetlenecek?*

* Evet, her yedi günde, ardından gelen işaretlenmesi bulunduktan sonra.

*Ne zaman bir sunucu kullanımdan alındığında sistem durumu denetiminin dışında kaldırılacak?*

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

* Görevler kapsamlı bir zorlamayı listesi vermek yerine, Önceliklendirilmiş önerileri adresleme odaklanmak öneririz. Bunları çözün sonra ek öneriler kullanılabilir hale gelir. Ayrıntılı listesini görmek isterseniz, tüm önerileri günlük analizi günlük arama özelliğini kullanarak görüntüleyebilirsiniz.

*Bir öneri yoksaymak için yolu var mı?*

* Evet, bkz: [önerileri yoksay](#ignore-recommendations) yukarıdaki bölümde.

## <a name="next-steps"></a>Sonraki adımlar
* [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı veri SQL sistem durumunu denetleyin ve önerileri çözümleme hakkında bilgi edinmek için.
