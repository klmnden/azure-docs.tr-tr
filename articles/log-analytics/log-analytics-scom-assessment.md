---
title: "System Center Operations Manager ortamınızı Azure günlük analizi ile en iyi duruma getirme | Microsoft Docs"
description: "System Center Operations Manager değerlendirme çözüm, risk ve sunucu ortamlarınızın durumunu düzenli aralıklarla değerlendirmek için kullanabilirsiniz."
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
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4992d98397da409f7c1cfbdeb40fdb0cdd0d2f19
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-assessment-preview-solution"></a>Ortamınızı System Center Operations Manager değerlendirme (Önizleme) çözümü ile en iyi duruma getirme

![System Center Operations Manager değerlendirme simgesi](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

System Center Operations Manager değerlendirme çözüm, risk ve System Center Operations Manager server ortamlarınızın durumunu düzenli aralıklarla değerlendirmek için kullanabilirsiniz. Bu makalede, yükleme, yapılandırma ve olası sorunlar için düzeltme eylemleri yararlanabilmeniz çözümü kullanan yardımcı olur.

Bu çözüm önerileri dağıtılan sunucu altyapınızı belirli öncelikli listesi sağlar. Öneriler dört arasında ayrılır hızlı bir şekilde Yardım odak alanlarına riski anlamak ve düzeltme eylemlerini gerçekleştirin.

Önerilerin bilgi ve müşteri ziyaretleriniz binlerce Microsoft mühendisleri tarafından elde edilen deneyimlerden dayanır. Her bir öneri, bir sorun için neden önemli ve önerilen değişiklikleri uygulamak nasıl hakkında rehberlik sağlar.

Kuruluşunuz için en önemli ve ücretsiz ve sağlam bir risk ortam çalıştıran doğru ilerleme durumunuzu izlemenize odak alanlarına seçebilirsiniz.

Çözüm ekledik ve bir değerlendirme tamamlanan, Özet sonra odak alanlarına odaklanmak için bilgi gösterilir **System Center Operations Manager değerlendirme** altyapınız için Pano. Aşağıdaki bölümlerde bilgileri üzerinde nasıl kullanacağınızı **System Center Operations Manager değerlendirme** Pano, burada görüntüleyebilir ve ardından uygulayın Eylemler SCOM altyapınız için önerilen.

![System Center Operations Manager çözüm döşeme](./media/log-analytics-scom-assessment/scom-tile.png)

![System Center Operations Manager değerlendirme Panosu](./media/log-analytics-scom-assessment/scom-dashboard01.png)

## <a name="installing-and-configuring-the-solution"></a>Yükleme ve çözüm yapılandırılıyor

Çözüm, Microsoft System Operations Manager 2012 R2 ve 2012 SP1 ile çalışır.

Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın.

 - OMS içinde bir değerlendirme çözümü kullanmadan önce çözümü yüklenmiş olması gerekir. Çözümden yüklemek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) ya da'ndaki yönergeleri izleyerek [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).

 - Çözüm için çalışma alanına ekledikten sonra System Center Operations Manager değerlendirme döşemenin Panoda ek yapılandırma gerekli iletisi görüntüler. Kutucuğuna tıklayın ve sayfasında belirtilen yapılandırma adımlarını izleyin

 ![System Center Operations Manager Pano kutucuğu](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

 System Center Operations Manager yapılandırmasını OMS çözümde yapılandırma sayfasında belirtilen adımları izleyerek komut dosyası aracılığıyla gerçekleştirilebilir.

 Bunun yerine, değerlendirmesi SCOM konsolu üzerinden yapılandırmak için izleyin aşağıdaki adımları aynı sırada
1. [System Center Operations Manager değerlendirmesi için farklı çalıştır hesabı ayarlama](#operations-manager-run-as-accounts-for-oms)  
2. [System Center Operations Manager değerlendirme kuralı yapılandırma](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a>System Center Operations Manager değerlendirme veri toplama ayrıntıları

System Center Operations Manager değerlendirme WMI verilerini, kayıt defteri verilerini, olay günlüğü verilerini, Windows PowerShell, SQL sorguları, dosya bilgileri Toplayıcı etkinleştirdiğiniz kullanarak sunucu üzerinden Operations Manager veri toplar.

Aşağıdaki tabloda System Center Operations Manager değerlendirmesi için veri toplama yöntemi gösterir ve ne sıklıkta bir aracı tarafından toplanan veriler.

| Platform | Doğrudan Aracısı | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | | | | &#8226; | | yedi gün |

## <a name="operations-manager-run-as-accounts-for-oms"></a>Operations Manager hesapları OMS için Çalıştır

Yönetim paketlerine sağlamak iş yükleri için OMS derlemeleri değeri-hizmetlerini ekleyin. Her iş yükü, bir etki alanı hesabı gibi farklı güvenlik bağlamında yönetim paketlerini çalıştırmak için iş yüküne özgü ayrıcalıkları gerektirir. Kimlik bilgileri sağlamak için bir Operations Manager farklı çalıştır hesabı yapılandırın.

System Center Operations Manager değerlendirmesi için Operations Manager farklı çalıştırma hesabını ayarlamak için aşağıdaki bilgileri kullanın.

### <a name="set-the-run-as-account"></a>Farklı Çalıştır hesabı ayarlama

1. Operations Manager konsolunda, Git **Yönetim** sekmesi.
2. Altında **Çalıştır Yapılandırması**, tıklatın **hesapları**.
3. Farklı Çalıştır bir Windows hesabı oluşturma sihirbazı kullanarak, aşağıdaki hesabı, oluşturun. Hesabını kullanacak şekilde tanımlanır ve aşağıdaki önkoşullar sahip olur:

    >[!NOTE]
    Farklı Çalıştır hesabı gereksinimleri karşılaması gerekir:
    - (Tüm işlem yöneticisi rolleri - yönetim sunucusu, OpsMgr veritabanı, veri ambarı, raporlama, Web konsolu, ağ geçidi) ortamında tüm sunucularda yerel Administrators grubunun bir etki alanı hesabı üyesi
    - İşlem yöneticisi rolü incelenen yönetim grubu için
    - Yürütme [betik](#sql-script-to-grant-granular-permissions-to-the-run-as-account) hesabı Operations Manager tarafından kullanılan SQL örneğinde ayrıntılı izinleri vermek için.
      Not: Bu hesap sysadmin hakları zaten varsa, sonra komut dosyası yürütme atlayın.

4. Altında **dağıtım güvenliği**seçin **daha güvenli**.
5. Hesap dağıtılacağı yeri yönetim sunucusu belirtin.
3. Çalıştır yapılandırması için geri dönün ve tıklatın **profilleri**.
4. Arama *SCOM değerlendirme profili*.
5. Profil adı olmalıdır: *Microsoft System Center Advisor SCOM değerlendirme farklı çalıştır profili*.
6. Sağ tıklayın ve özelliklerini güncelleştirmek ve son oluşturduğunuz farklı çalıştır 3. adımda oluşturduğunuz hesabı ekleyin.

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a>Farklı Çalıştır hesabı ayrıntılı izinleri vermek için SQL komut dosyası

Farklı Çalıştır hesabının Operations Manager tarafından kullanılan SQL örneğinde gerekli izinleri vermek için aşağıdaki SQL betiğini yürütün.

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


### <a name="configure-the-assessment-rule"></a>Değerlendirme kuralı yapılandırma

System Center Operations Manager değerlendirme çözümün Yönetim Paketi, adında bir kural içerir *Microsoft System Center Advisor SCOM değerlendirme değerlendirme kural çalıştırma*. Bu kural, değerlendirme çalıştırmaktan sorumludur. Kuralı etkinleştirmek ve sıklığı yapılandırmak için aşağıdaki yordamları kullanın.

Varsayılan olarak, Microsoft System Center Advisor SCOM değerlendirme çalıştırmak değerlendirme kural devre dışıdır. Değerlendirme çalıştırmak için bir yönetim sunucusunda kural etkinleştirmeniz gerekir. Aşağıdaki adımları kullanın.

#### <a name="enable-the-rule-for-a-specific-management-server"></a>Belirli yönetim sunucusuna ilişkin kuralını etkinleştir

1. İçinde **yazma** arama kuralı için Operations Manager konsolunun çalışma *Microsoft System Center Advisor SCOM değerlendirme değerlendirme kural çalıştırma* içinde **kuralları** bölmesi.
2. Arama sonuçlarında metin içeren bir tanesini seçin *türü: Yönetim sunucusu*.
3. Kuralı sağ tıklatın ve ardından **geçersiz kılmaları** > **sınıfın belirli bir nesnesi için: Yönetim sunucusu**.
4.  Kullanılabilir yönetim sunucuları listesini kural çalıştırdığı yönetim sunucusu seçin.
5.  Geçersiz kılma değerine değiştirdiğinizden emin olun **True** için **etkin** parametre değeri.  
    ![parametresi geçersiz kıl](./media/log-analytics-scom-assessment/rule.png)

Bu pencerede hala, bir sonraki yordamı kullanarak çalışma sıklığını yapılandırın.

#### <a name="configure-the-run-frequency"></a>Çalışma sıklığını yapılandırma

Değerlendirme her 10.080 dakikadır (veya yedi gün) çalışmak üzere yapılandırılan varsayılan zaman aralığı. En küçük değerini 1440 dakika (veya bir gün) değerine geçersiz kılabilirsiniz. Değerin ardışık değerlendirme çalışmaları arasında gerekli en düşük zaman aralığı temsil eder. Aralık geçersiz kılmak için aşağıdaki adımları kullanın.

1. İçinde **yazma** arama kuralı için Operations Manager konsolunun çalışma *Microsoft System Center Advisor SCOM değerlendirme değerlendirme kural çalıştırma* içinde **kuralları** bölmesi.
2. Arama sonuçlarında metin içeren bir tanesini seçin *türü: Yönetim sunucusu*.
3. Kuralı sağ tıklatın ve ardından **kuralı geçersiz kıl** > **sınıfın tüm nesneleri için: Yönetim sunucusu**.
4. Değişiklik **aralığı** , istenen aralık değeri için parametre değeri. Aşağıdaki örnekte, değer 1440 dakika (bir gün) olarak ayarlanır.  
    ![aralığı parametresi](./media/log-analytics-scom-assessment/interval.png)  
    Değer 1440 dakikadan daha kısa bir süre için ayarlarsanız, kuralın bir gün aralığında çalışır. Bu örnekte, kural aralık değeri göz ardı eder ve bir gün sıklığında çalıştırır.


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

## <a name="use-assessment-focus-area-recommendations"></a>Değerlendirme odak alanı önerileri kullanın

OMS içinde bir değerlendirme çözümü kullanmadan önce çözümü yüklenmiş olması gerekir. Daha fazla bilgi için çözümleri yükleme hakkında bkz [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). Yüklendikten sonra OMS genel bakış sayfasında System Center Operations Manager değerlendirme döşeme kullanarak önerileri özetini görüntüleyebilirsiniz.

Altyapınız ve ardından-ayrıntıya önerileri için özetlenmiş uyumluluk değerlendirmesi görüntüleyin.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için öneriler görüntülemek ve düzeltici işlemleri için

1. Üzerinde **genel bakış** sayfasında, **System Center Operations Manager değerlendirme** döşeme.
2. Üzerinde **System Center Operations Manager değerlendirme** sayfasında odak alanı Kanatlar birinde özet bilgilerini inceleyin ve sonra bu odak alanı için öneriler görüntülemek için tıklatın.
3. Odak alanı sayfaları hiçbirinde ortamınız için öncelikli önerilerin görüntüleyebilirsiniz. Önerinin altında tıklatın **etkilenen nesneleri** öneri neden yapılan hakkında ayrıntıları görüntülemek için.  
    ![Odaklanılan alan](./media/log-analytics-scom-assessment/focus-area.png)
4. Önerilen düzeltici eylemleri gerçekleştirebilirsiniz **önerilen eylemleri**. Öğe ele, önerilen eylemler gerçekleştirilen ve uyumluluk puan artıracaktır sonraki değerlendirmeleri kaydeder. Düzeltilmiş öğeler görünür olarak **geçirilen nesneleri**.

## <a name="ignore-recommendations"></a>Öneriler yoksay

Yoksay istediğiniz önerileri varsa, öneriler değerlendirme sonuçlarında görünmesini engellemek için OMS kullanan bir metin dosyası oluşturabilirsiniz.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a>Yoksay istediğiniz önerileri tanımlamak için

1. Sunucunuzdan çalışma alanınıza oturum açın ve günlük arama açın. Ortamınızdaki bilgisayarları için başarısız olan liste önerileri için aşağıdaki sorguyu kullanın.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    Günlük arama sorgusu gösteren ekran görüntüsü aşağıda verilmiştir:  
    ![günlük araması](./media/log-analytics-scom-assessment/scom-log-search.png)

2. Yoksay istediğiniz önerileri seçin. Sonraki yordamda Recommendationıd için değerleri kullanacaksınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Oluşturma ve bir IgnoreRecommendations.txt metin dosyası kullanma

1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Her Recommendationıd ayrı bir satırda yoksay ve sonra dosyayı kaydedip kapatın için OMS istediğiniz her bir öneri için yazın veya yapıştırın.
3. Dosya aşağıdaki klasörde önerileri yoksaymak için OMS istediğiniz her bilgisayara yerleştirin.
4. Operations Manager yönetim sunucusunda - *SystemDrive*: \Program System Center 2012 R2\Operations Manager\Server.

### <a name="to-verify-that-recommendations-are-ignored"></a>Öneriler göz ardı edilir doğrulamak için

1. Sonraki değerlendirme çalıştığında, varsayılan olarak yedi günde zamanlanmış sonra belirtilen önerileri yoksayıldı işaretlenir ve değerlendirme Panoda görünmez.
2. Tüm yoksayılan öneriler listelemek için aşağıdaki günlük arama sorgularını kullanabilirsiniz.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

3. Daha sonra yoksayılan önerileri görmek istediğiniz karar verirseniz, tüm IgnoreRecommendations.txt dosyaları silin veya bunları RecommendationIDs kaldırabilirsiniz.

## <a name="system-center-operations-manager-assessment-solution-faq"></a>System Center Operations Manager değerlendirme çözümü hakkında SSS

*OMS çalışma alanıma değerlendirme çözümü ekledim. Ancak önerilerini görmek yok. Neden olmasın?* Çözüm eklendikten sonra aşağıdaki adımları görünümü OMS Panoda önerileri kullanın.  

- [System Center Operations Manager değerlendirmesi için farklı çalıştır hesabı ayarlama](#operations-manager-run-as-accounts-for-oms)  
- [System Center Operations Manager değerlendirme kuralı yapılandırma](#configure-the-assessment-rule)


*Ne sıklıkta değerlendirme çalıştıran yapılandırmak için yolu var mı?* Evet. Bkz: [çalışma sıklığını yapılandırma](#configure-the-run-frequency).

*System Center Operations Manager değerlendirme çözümü ekledim sonra başka bir sunucuya bulunmuşsa, değerlendirileceğini?* Evet, bulma sonra onu sonra itibaren--varsayılan olarak, her yedi günde değerlendirildiği.

*Veri toplama mu işlemin adı nedir?* AdvisorAssessment.exe

*Burada AdvisorAssessment.exe işlemi çalışıyor mu?* AdvisorAssessment.exe değerlendirme kural etkinleştirdiğiniz sistem sağlığı hizmeti yönetim sunucusunun altında çalışır. Bu işlemi kullanarak, tüm ortamınız keşfinin uzak veri toplulukta elde edilir.

*Ne kadar veri toplamaya yönelik sürer?* Veri toplama sunucusundaki yaklaşık bir saat sürer. Bu, pek çok Operations Manager örnekleri veya veritabanlarına sahip ortamlarda daha uzun sürebilir.

*Ne değerlendirme aralığını 1440 dakikadan daha kısa bir süre için ayarlarım?* Değerlendirme en fazla günde bir kez çalıştırmak üzere önceden yapılandırılmıştır. 1440 dakikadan daha kısa bir değer aralığı değeri geçersiz kılarsanız değerlendirme 1440 dakika aralık değeri kullanır.

*Önkoşul hataları olup olmadığını bilmek nasıl?* Ardından değerlendirme çalıştırdı ve sonuçları görmüyorsanız, değerlendirme için önkoşulların bazıları başarısız olabilir. Sorguları çalıştırabilirsiniz: `Type=Operation Solution=SCOMAssessment` ve `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` günlük başarısız ön koşullar görmek için Ara.

*Var olan bir `Failed to connect to the SQL Instance (….).` önkoşul hatası iletisi. Sorun nedir?* AdvisorAssessment.exe, toplar, işlem sistem sağlığı hizmeti yönetim sunucusunun altında çalışır. Değerlendirme bir parçası olarak, işlem Operations Manager veritabanı mevcut olduğu SQL Server'a bağlanmak çalışır. Güvenlik duvarı kuralları SQL Server örneği bağlantısı engellediğinizde bu hata oluşabilir.

*Hangi türde veri toplanır?* Aşağıdaki veri türlerini toplanır: - WMI verilerini - kayıt defteri - olay günlüğü verilerini - Operations Manager verilerini Windows PowerShell, SQL sorguları ve dosya bilgileri Toplayıcı aracılığıyla.

*Bir farklı çalıştır hesabı yapılandırmak neden var mı?* Bir Operations Manager sunucusu için çeşitli SQL sorguları çalıştırılır. Sırayla bunları çalıştırmak gerekli izinlere sahip bir farklı çalıştır hesabı kullanmanız gerekir. Ayrıca, yerel yönetici kimlik bilgileri WMI sorgusu için gereklidir.

*Neden yalnızca ilk 10 önerileri görüntülemek?* Görevlerin kapsamlı, zorlamayı listesi vermek yerine, Önceliklendirilmiş önerileri adresleme odaklanmak öneririz. Bunları çözün sonra ek öneriler kullanılabilir hale gelir. Ayrıntılı listesini görmek isterseniz, tüm önerileri günlük arama özelliğini kullanarak görüntüleyebilirsiniz.

*Bir öneri yoksaymak için yolu var mı?* Evet, bkz: [önerileri yoksay](#Ignore-recommendations).


## <a name="next-steps"></a>Sonraki adımlar

- [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı System Center Operations Manager Değerlendirme verileri ve öneriler görüntülemek için.
