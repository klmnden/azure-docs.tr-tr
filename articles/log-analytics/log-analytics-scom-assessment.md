---
title: "System Center Operations Manager ortamınızı Azure günlük analizi ile en iyi duruma getirme | Microsoft Docs"
description: "System Center Operations Manager sistem durumu denetleyin çözüm, risk ve ortamlarınızın durumunu düzenli aralıklarla değerlendirmek için kullanabilirsiniz."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2017
ms.author: magoedte;banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3a66cc13d05c81de571e2710519ad9474304d656
ms.sourcegitcommit: b83781292640e82b5c172210c7190cf97fabb704
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-health-check-preview-solution"></a>Ortamınızı System Center Operations Manager sistem durumunu denetleyin (Önizleme) çözümü ile en iyi duruma getirme

![System Center Operations Manager sistem durumu denetleme simgesi](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

Düzenli bir aralıkta risk ve System Center Operations Manager yönetim grubunun sistem durumunu değerlendirmek için sistem Center Operations Manager sistem durumu denetimi çözüm kullanabilirsiniz. Bu makalede, yükleme, yapılandırma ve olası sorunlar için düzeltme eylemleri yararlanabilmeniz çözümü kullanan yardımcı olur.

Bu çözüm önerileri dağıtılan sunucu altyapınızı belirli öncelikli listesi sağlar. Öneriler dört arasında ayrılır hızlı bir şekilde Yardım odak alanlarına riski anlamak ve düzeltme eylemlerini gerçekleştirin.

Önerilerin bilgi ve müşteri ziyaretleriniz binlerce Microsoft mühendisleri tarafından elde edilen deneyimlerden dayanır. Her bir öneri, bir sorun için neden önemli ve önerilen değişiklikleri uygulamak nasıl hakkında rehberlik sağlar.

Kuruluşunuz için en önemli ve ücretsiz ve sağlam bir risk ortam çalıştıran doğru ilerleme durumunuzu izlemenize odak alanlarına seçebilirsiniz.

Çözüm ekledikten sonra bir değerlendirme gerçekleştirilen, Özet bilgi odak alanlarına odaklanmak için gösterilir **sistem Center Operations Manager sistem durumu denetimi** altyapınız için Pano. Bilgileri üzerinde nasıl kullanacağınızı aşağıdaki bölümlerde **sistem Center Operations Manager sistem durumu denetimi** Pano, burada görüntüleyebilir ve ardından uygulayın Eylemler Operations Manager ortamınız için önerilir.

![System Center Operations Manager çözüm döşeme](./media/log-analytics-scom-assessment/log-analytics-scom-healthcheck-tile.png)

![System Center Operations Manager sistem durumu denetleme Panosu](./media/log-analytics-scom-assessment/log-analytics-scom-healthcheck-dashboard-01.png)

## <a name="installing-and-configuring-the-solution"></a>Yükleme ve çözüm yapılandırılıyor

Çözüm, Microsoft System Operations Manager 2012 Service Pack (SP1 ile) 1 ve 2012 R2 çalışır.

Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın.

 - Günlük analizi sistem durumu denetimi çözüm kullanmadan önce çözümü yüklenmiş olması gerekir. Çözümden yüklemek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview).

 - Çözüm için çalışma alanına ekledikten sonra **sistem Center Operations Manager sistem durumu denetimi** panosundaki bölümünden bir ek yapılandırma gerekli iletisi görüntüler. Kutucuğuna tıklayın ve sayfasında belirtilen yapılandırma adımlarını izleyin

 ![System Center Operations Manager Pano kutucuğu](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

> [!NOTE]
> System Center Operations Manager yapılandırmasını yapılabilir günlük analizi çözümde yapılandırma sayfasında belirtilen adımları izleyerek bir komut dosyası kullanma.

 Operations Manager işletim Konsolu aracılığıyla değerlendirme yapılandırmak için aşağıdaki adımları aşağıdaki sırayla gerçekleştirin:
1. [Farklı Çalıştır hesabı sistem Center Operations Manager sistem durumu denetimi için ayarlama](#operations-manager-run-as-accounts-for-log-analytics)  
2. [System Center Operations Manager sistem durumu denetleyin kuralı yapılandırma](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a>System Center Operations Manager değerlendirme veri toplama ayrıntıları

System Center Operations Manager değerlendirme verilerini aşağıdaki kaynaklardan toplar: 

* Kayıt defteri
* Windows Yönetim Araçları (WMI)
* Olay günlüğü
* Dosya verileri
* Doğrudan Operations Manager'dan belirttiğiniz yönetim sunucusundan PowerShell ve SQL sorgularını kullanarak.  

Veri yönetim Sunucusu'nda toplanır ve günlük analizi için her yedi günde iletilir.  

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Operations Manager hesapları günlük analizi için Çalıştır

Günlük analizi derlemeleri yönetim paketlerine sağlamak iş yükleri için değer-hizmetlerini ekleyin. Her iş yükü, bir etki alanı kullanıcı hesabı gibi farklı güvenlik bağlamında yönetim paketlerini çalıştırmak için iş yüküne özgü ayrıcalıkları gerektirir. Bir Operations Manager farklı çalıştır hesabı ile ayrıcalıklı kimlik bilgilerini yapılandırın. Ek bilgi için bkz: [bir farklı çalıştır hesabının nasıl oluşturulacağını](https://technet.microsoft.com/library/hh321655(v=sc.12).aspx) Operations Manager belgelerinde. 

System Center Operations Manager sistem durumu denetle Operations Manager farklı çalıştır hesabı ayarlamak için aşağıdaki bilgileri kullanın.

### <a name="set-the-run-as-account"></a>Farklı Çalıştır hesabı ayarlama

Farklı Çalıştır hesabı aşağıdaki devam etmeden önce gereksinimleri karşılaması gerekir:

* Tüm Operations Manager rolüne - yönetim sunucusu, işletimsel barındıran SQL Server, veri ambarı ve ACS veritabanı destekleyen tüm sunucularda yerel Administrators grubunun üyesi olan bir etki alanı kullanıcı hesabı raporlama, Web konsolu ve ağ geçidi sunucusu.
* İşlem yöneticisi rolü incelenen yönetim grubu için
* Hesabın SQL sysadmin hakları yoksa sonra yürütün [betik](#sql-script-to-grant-granular-permissions-to-the-run-as-account) hesap birini veya tümünü Operations Manager veritabanları barındıran her SQL Server örneği üzerinde ayrıntılı izinleri vermek için. 

1. Operations Manager Konsolu'ndaki seçin **Yönetim** gezinti düğmesi.
2. Altında **Çalıştır Yapılandırması**, tıklatın **hesapları**.
3. İçinde **farklı çalıştır hesabı oluşturma** Sihirbazı, **giriş** sayfasında **sonraki**.
4. Üzerinde **Genel Özellikler** sayfasında, **Windows** içinde **farklı çalıştır hesabı türü:** listesi.
5. Bir görünen ad yazın **görünen adı** metin kutusuna ve isteğe bağlı olarak bir açıklama yazın **açıklama** kutusuna ve ardından **sonraki**. 
6. Üzerinde **dağıtım güvenliği** sayfasında, **daha güvenli**.
7. **Oluştur**'a tıklayın.  

Farklı Çalıştır hesabı oluşturulur, hedef yönetim sunucuları yönetim grubundaki gerekiyor ve iş akışları kimlik bilgilerini kullanarak çalışacak şekilde bir önceden tanımlanmış farklı çalıştır profili ile ilişkilendirilmiş.  

1. Altında **Çalıştır Yapılandırması**, **hesapları**, sonuçlar bölmesinde, daha önce oluşturduğunuz hesabını çift tıklatın.
2. Üzerinde **dağıtım** sekmesini tıklatın, **Ekle** için **seçilmiş bilgisayarlar** kutusunda ve hesaba dağıtmak için yönetim sunucusu ekleyin.  Tıklatın **Tamam** iki kez yaptığınız değişiklikleri kaydetmek için.
3. Altında **Çalıştır Yapılandırması**, tıklatın **profilleri**. 
4. Arama *SCOM değerlendirme profili*.
5. Profil adı olmalıdır: *Microsoft System Center Advisor SCOM değerlendirme farklı çalıştır profili*.
6. Sağ tıklayın ve özelliklerini güncelleştirmek ve son oluşturduğunuz farklı çalıştır daha önce oluşturduğunuz hesabı ekleyin.

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a>Farklı Çalıştır hesabı ayrıntılı izinleri vermek için SQL komut dosyası

Farklı Çalıştır hesabının Operations Manager işletimsel barındırma, veri ambarı ve ACS veritabanı tarafından kullanılan SQL Server örneği için gerekli izinleri vermek için aşağıdaki SQL betiğini yürütün.

```
-- Replace <UserName> with the actual user name being used as Run As Account.
USE master

-- Create login for the user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions to user.
GRANT VIEW SERVER STATE TO [UserName]
GRANT VIEW ANY DEFINITION TO [UserName]
GRANT VIEW ANY DATABASE TO [UserName]

-- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
-- NOTE: This command must be run anytime new databases are added to SQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT To [UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace the Operations Manager database name with the one in your environment
Use [OperationsManager];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager DatawareHouse database name with the one in your environment
Use [OperationsManagerDW];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager Audit Collection database name with the one in your environment
Use [OperationsManagerAC];
GRANT SELECT To [UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace the Operations Manager database name with the one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```

### <a name="configure-the-health-check-rule"></a>Sistem durumu onay kuralı yapılandırma

Adlı bir kural sistem Center Operations Manager sistem durumu denetimi çözümün Yönetim paketini içeren *Microsoft System Center Advisor SCOM değerlendirme değerlendirme kural çalıştırma*. Bu kural, sistem durumu denetimi çalıştırmaktan sorumludur. Kuralı etkinleştirmek ve sıklığı yapılandırmak için aşağıdaki yordamları kullanın.

Varsayılan olarak, Microsoft System Center Advisor SCOM değerlendirme çalıştırmak değerlendirme kural devre dışıdır. Sistem durumu denetimini çalıştırmak için bir yönetim sunucusunda kural etkinleştirmeniz gerekir. Aşağıdaki adımları kullanın.

#### <a name="enable-the-rule-for-a-specific-management-server"></a>Belirli yönetim sunucusuna ilişkin kuralını etkinleştir

1. İçinde **yazma** arama kuralı için Operations Manager işletim konsolunun çalışma *Microsoft System Center Advisor SCOM değerlendirme değerlendirme kural çalıştırma* içinde **kuralları** bölmesi.
2. Arama sonuçlarında metin içeren bir tanesini seçin *türü: Yönetim sunucusu*.
3. Kuralı sağ tıklatın ve ardından **geçersiz kılmaları** > **sınıfın belirli bir nesnesi için: Yönetim sunucusu**.
4.  Kullanılabilir yönetim sunucuları listesini kural çalıştırdığı yönetim sunucusu seçin.  Bu, aynı yönetim sunucusuna daha önce farklı çalıştır hesabıyla ilişkilendirmek üzere yapılandırılmış olması gerekir.
5.  Geçersiz kılma değerine değiştirdiğinizden emin olun **True** için **etkin** parametre değeri.<br><br> ![parametresi geçersiz kıl](./media/log-analytics-scom-assessment/rule.png)

    Hala Bu pencerede karşın, bir sonraki yordamı kullanarak çalışma sıklığını yapılandırın.

#### <a name="configure-the-run-frequency"></a>Çalışma sıklığını yapılandırma

Değerlendirme, varsayılan olarak her 10.080 dakikadır (veya yedi gün) çalışmak üzere yapılandırılır. En küçük değerini 1440 dakika (veya bir gün) değerine geçersiz kılabilirsiniz. Değerin ardışık değerlendirme çalışmaları arasında gerekli en düşük zaman aralığı temsil eder. Aralık geçersiz kılmak için aşağıdaki adımları kullanın.

1. İçinde **yazma** arama kuralı için Operations Manager konsolunun çalışma *Microsoft System Center Advisor SCOM değerlendirme değerlendirme kural çalıştırma* içinde **kuralları** bölüm.
2. Arama sonuçlarında metin içeren bir tanesini seçin *türü: Yönetim sunucusu*.
3. Kuralı sağ tıklatın ve ardından **kuralı geçersiz kıl** > **sınıfın tüm nesneleri için: Yönetim sunucusu**.
4. Değişiklik **aralığı** , istenen aralık değeri için parametre değeri. Aşağıdaki örnekte, değer 1440 dakika (bir gün) olarak ayarlanır.<br><br> ![aralığı parametresi](./media/log-analytics-scom-assessment/interval.png)<br>  

    Değer 1440 dakikadan daha kısa bir süre için ayarlarsanız, kural bir gün aralığı üzerinde çalışır. Bu örnekte, kural aralık değeri göz ardı eder ve bir gün sıklığında çalıştırır.


## <a name="understanding-how-recommendations-are-prioritized"></a>Önerilerin nasıl önceliklendirilir anlama

Yapılan her öneri öneri göreceli önemini tanımlayan bir ağırlıklı değer verilir. Yalnızca 10 en önemli öneriler gösterilir.

### <a name="how-weights-are-calculated"></a>Ağırlıkları nasıl hesaplanır

Ağırlık belirlemeyi üç anahtar faktörlerini temel alarak toplam değerler şunlardır:

- *Olasılık* tanımlanan bir sorunun sorunlara neden. Daha yüksek olasılık öneri için daha büyük bir genel puan karşılık gelir.
- *Etkisi* kuruluşunuzdaki bir soruna neden olursa sorun. Daha yüksek bir etkisi öneri için daha büyük bir genel puan karşılık gelir.
- *Çaba* öneriyi uygulamak için gereklidir. Daha yüksek çaba öneri için daha küçük bir genel puan karşılık gelir.

Her öneri ağırlıklı her odak alanı için kullanılabilir toplam puanı yüzdesi olarak ifade edilir. Örneğin, bu öneri uygulama kullanılabilirlik ve iş sürekliliği odak alanında bir öneri %5 puanı varsa, genel kullanılabilirlik ve iş sürekliliği puan tarafından %5 artırır.

### <a name="focus-areas"></a>Odak alanları

**Kullanılabilirlik ve iş sürekliliği** -bu odaklanılan alan hizmet kullanılabilirliği, altyapı ve iş koruması dayanıklılık için öneriler gösterir.

**Performans ve ölçeklenebilirlik** -bu odak alanı kuruluşunuzun yardımcı olacak öneriler gösterir BT altyapısı arttıkça, BT ortamınız geçerli performans gereksinimlerini karşıladığından ve altyapı değiştirmeye yanıt verebilmesini olduğundan emin olun gerekir.

**Yükseltme, geçiş ve dağıtım** -bu odak alanı, yükseltme, geçirme ve SQL Server mevcut altyapınızda dağıtmanıza yardımcı olacak öneriler gösterir.

**İşlemler ve izleme** -bu odak alanı BT işlemlerinizi kolaylaştırır, önleyici bakım uygulamak ve performansı en üst düzeye çıkarmanıza yardımcı olacak öneriler gösterir.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>Her odaklanılan alan % 100 puanlı hedefleyin?

Olmayabilir. Öneriler bilgi ve müşteri ziyaretleriniz binlerce arasında Microsoft mühendisleri tarafından elde edilen deneyimleri temel alır. Ancak, iki sunucu altyapılar aynıdır ve özel öneriler için daha az veya uygun olabilir. Örneğin, bazı güvenlik önerileri sanal makinelerinizi Internet'e açık değildir, daha az ilgili olabilir. Bazı kullanılabilirlik öneriler düşük öncelik geçici veri toplama ve raporlama sağladığı hizmetler için daha az uygun olmayabilir. Olgun bir iş için önemli olan sorunları bir başlangıcından daha az önemli olabilir. Hangi odak alanların önceliklerinizden olduğunu belirlemek ve, puanları zamanla nasıl değiştiğini adresindeki Ara isteyebilirsiniz.

Her öneri neden önemli olduğu hakkında yönergeler içerir. Öneri uygulama verilen BT hizmetlerinizi yapısını ve kuruluşunuzun iş gereksinimlerini sizin için uygun olup olmadığını değerlendirmek için bu kılavuzu kullanın.

## <a name="use-health-check-focus-area-recommendations"></a>Kullanım durumu Denetim odak alanı önerileri

Günlük analizi bir sistem durumu denetimi çözümü kullanmadan önce çözümü yüklenmiş olması gerekir. Daha fazla bilgi için çözümleri yükleme hakkında bkz [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). Yüklendikten sonra genel bakış sayfasında OMS portalında sistem Center Operations Manager sistem durumu denetimi döşeme kullanarak önerileri özetini görüntüleyebilirsiniz.

Altyapınız ve ardından-ayrıntıya önerileri için özetlenmiş uyumluluk değerlendirmesi görüntüleyin.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için öneriler görüntülemek ve düzeltici işlemleri için
1. Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com). 
2. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**.
3. Günlük analizi abonelikleri bölmesinde, bir çalışma alanını seçin ve ardından **OMS portalı** döşeme.  
4. Üzerinde **genel bakış** sayfasında, **sistem Center Operations Manager sistem durumu denetimi** döşeme.
5. Üzerinde **sistem Center Operations Manager sistem durumu denetimi** sayfasında odak alanı Kanatlar birinde özet bilgilerini inceleyin ve sonra bu odak alanı için öneriler görüntülemek için tıklatın.
6. Odak alanı sayfaları hiçbirinde ortamınız için öncelikli önerilerin görüntüleyebilirsiniz. Önerinin altında tıklatın **etkilenen nesneleri** öneri neden yapılan hakkında ayrıntıları görüntülemek için.<br><br> ![Odaklanılan alan](./media/log-analytics-scom-assessment/log-analytics-scom-healthcheck-dashboard-02.png)<br>
7. Önerilen düzeltici eylemleri gerçekleştirebilirsiniz **önerilen eylemleri**. Öğe ele, önerilen eylemler gerçekleştirilen ve uyumluluk puan artıracaktır sonraki değerlendirmeleri kaydeder. Düzeltilmiş öğeler görünür olarak **geçirilen nesneleri**.

## <a name="ignore-recommendations"></a>Öneriler yoksay

Yoksay istediğiniz önerileri varsa, öneriler değerlendirme sonuçlarında görünmesini engellemek için günlük analizi kullanan bir metin dosyası oluşturabilirsiniz.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a>Yoksay istediğiniz önerileri tanımlamak için
1. Günlük analizi çalışma sayfasında seçilen çalışma alanınız için Azure portalında tıklatın **günlük arama** döşeme.
2. Ortamınızdaki bilgisayarları için başarısız olan liste önerileri için aşağıdaki sorguyu kullanın.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), sonra da yukarıdaki sorguda şu şekilde değiştirir.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

    Günlük arama sorgusu gösteren ekran görüntüsü aşağıda verilmiştir:<br><br> ![günlük araması](./media/log-analytics-scom-assessment/scom-log-search.png)<br>

3. Yoksay istediğiniz önerileri seçin. Sonraki yordamda Recommendationıd için değerleri kullanacaksınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Oluşturma ve bir IgnoreRecommendations.txt metin dosyası kullanma

1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Her Recommendationıd ayrı bir satırda yoksay ve sonra dosyayı kaydedip kapatın için günlük analizi istediğiniz her bir öneri için yazın veya yapıştırın.
3. Dosya aşağıdaki klasörde önerileri yoksaymak için günlük analizi istediğiniz her bilgisayara yerleştirin.
4. Operations Manager yönetim sunucusunda - *SystemDrive*: \Program System Center 2012 R2\Operations Manager\Server.

### <a name="to-verify-that-recommendations-are-ignored"></a>Öneriler göz ardı edilir doğrulamak için

1. Sonraki değerlendirme çalıştığında, varsayılan olarak yedi günde zamanlanmış sonra belirtilen önerileri yoksayıldı işaretlenir ve sistem durumu denetimi Panoda görünmez.
2. Tüm yoksayılan öneriler listelemek için aşağıdaki günlük arama sorgularını kullanabilirsiniz.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    >[!NOTE]
    > Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), sonra da yukarıdaki sorguda şu şekilde değiştirir.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Ignore" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

3. Daha sonra yoksayılan önerileri görmek istediğiniz karar verirseniz, tüm IgnoreRecommendations.txt dosyaları silin veya bunları RecommendationIDs kaldırabilirsiniz.

## <a name="system-center-operations-manager-health-check-solution-faq"></a>System Center Operations Manager sistem durumu denetleme çözüm SSS

*Günlük analizi çalışma alanıma sistem durumu denetimi çözüm ekledim. Ancak önerilerini görmek yok. Neden olmasın?* Çözüm eklendikten sonra aşağıdaki adımları görünümü günlük analizi Panoda önerileri kullanın.  

- [Farklı Çalıştır hesabı sistem Center Operations Manager sistem durumu denetimi için ayarlama](#operations-manager-run-as-accounts-for-log-analytics)  
- [System Center Operations Manager sistem durumu denetleyin kuralı yapılandırma](#configure-the-health-check-rule)


*Ne sıklıkta denetimi çalıştırılır yapılandırmak için yolu var mı?* Evet. Bkz: [çalışma sıklığını yapılandırma](#configure-the-run-frequency).

*System Center Operations Manager değerlendirme çözümü ekledim sonra başka bir sunucuya bulunmuşsa, denetlenecek?* Evet, bulma sonra onu daha sonra varsayılan olarak yedi günde denetlenir.

*Veri toplama mu işlemin adı nedir?* AdvisorAssessment.exe

*Burada AdvisorAssessment.exe işlemi çalışıyor mu?* AdvisorAssessment.exe, sistem durumu onay kuralı etkinleştirdiğiniz management server'ın sistem sağlığı hizmeti işleminin altında çalışır. Bu işlemi kullanarak, tüm ortamınız keşfinin uzak veri toplulukta elde edilir.

*Ne kadar veri toplamaya yönelik sürer?* Veri toplama sunucusundaki yaklaşık bir saat sürer. Bu, pek çok Operations Manager örnekleri veya veritabanlarına sahip ortamlarda daha uzun sürebilir.

*Ne değerlendirme aralığını 1440 dakikadan daha kısa bir süre için ayarlarım?* Değerlendirme en fazla günde bir kez çalıştırmak üzere önceden yapılandırılmıştır. 1440 dakikadan daha kısa bir değer aralığı değeri geçersiz kılarsanız değerlendirme 1440 dakika aralık değeri kullanır.

*Önkoşul hataları olup olmadığını bilmek nasıl?* Ardından sistem durumu denetimi çalıştırdı ve sonuçları görmüyorsanız, bazı denetim için Önkoşullar başarısız olabilir. Sorguları çalıştırabilirsiniz: `Operation Solution=SCOMAssessment` ve `SCOMAssessmentRecommendation FocusArea=Prerequisites` günlük başarısız Önkoşullar görmek için Ara.

*Var olan bir `Failed to connect to the SQL Instance (….).` önkoşul hatası iletisi. Sorun nedir?* AdvisorAssessment.exe, toplar, işlem yönetim sunucusunda sistem sağlığı hizmeti işleminin altında çalışır. Sistem durumu denetimi bir parçası olarak, işlem Operations Manager veritabanı mevcut olduğu SQL sunucusuna bağlanmaya çalışır. Güvenlik duvarı kuralları SQL Server örneği bağlantısı engellediğinizde bu hata oluşabilir.

*Hangi türde veri toplanır?* Aşağıdaki veri türlerini toplanır: - WMI verilerini - kayıt defteri - olay günlüğü verilerini - Operations Manager verilerini Windows PowerShell, SQL sorguları ve dosya bilgileri Toplayıcı aracılığıyla.

*Bir farklı çalıştır hesabı yapılandırmak neden var mı?* Operations Manager ile çeşitli SQL sorguları çalıştırılır. Sırayla bunları çalıştırmak gerekli izinlere sahip bir farklı çalıştır hesabı kullanmanız gerekir. Ayrıca, yerel yönetici kimlik bilgileri WMI sorgusu için gereklidir.

*Neden yalnızca ilk 10 önerileri görüntülemek?* Görevlerin kapsamlı, zorlamayı listesi vermek yerine, Önceliklendirilmiş önerileri adresleme odaklanmak öneririz. Bunları çözün sonra ek öneriler kullanılabilir hale gelir. Ayrıntılı listesini görmek isterseniz, tüm önerileri günlük arama özelliğini kullanarak görüntüleyebilirsiniz.

*Bir öneri yoksaymak için yolu var mı?* Evet, bkz: [önerileri yoksay](#Ignore-recommendations).


## <a name="next-steps"></a>Sonraki adımlar

- [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı verileri System Center Operations Manager sistem durumunu denetleyin ve önerileri çözümleme hakkında bilgi edinmek için.
