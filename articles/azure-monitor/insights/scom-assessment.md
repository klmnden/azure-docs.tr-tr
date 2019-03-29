---
title: Azure Log Analytics ile System Center Operations Manager ortamınızı en iyi duruma getirme | Microsoft Docs
description: Sistem Center Operations Manager sistem durumu denetimi çözümü, düzenli aralıklarla, ortamlarının sistem durumunu ve riskini değerlendirmek için kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/25/2018
ms.author: magoedte
ms.openlocfilehash: 27b55af74a713c51655891df8c852ff44cd3744a
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621779"
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-health-check-preview-solution"></a>Ortamınızı System Center Operations Manager sistem durumunu denetleyin (Önizleme) çözümü ile en iyi duruma getirme

![System Center Operations Manager sistem durumu denetimi simgesi](./media/scom-assessment/scom-assessment-symbol.png)

Sistem Center Operations Manager sistem durumu denetimi çözümü, risk ve System Center Operations Manager yönetim grubunuzun sistem durumuna ilişkin düzenli aralıklarla değerlendirmek için kullanabilirsiniz. Bu makalede, yükleme, yapılandırma ve çözüm kullanabilir, böylece olası sorunlar için düzeltici eylemleri gerçekleştirebilirsiniz yardımcı olur.

Bu çözüm, Önceliklendirilmiş öneriler dağıtılan sunucu altyapınızı belirli listesini sağlar. Öneriler dört arasında ayrılır odak alanlarına odaklanmak hızlı bir şekilde yardımcı riskini anlamak ve düzeltici eylemi gerçekleştirme.

Yapılan öneriler bilgi ve müşteri binlerce Microsoft mühendisinin göre kazanılan deneyimi temel alır. Her öneri, önerilen değişiklikleri uygulamak nasıl bir sorun için neden önemli ve ilgili yönergeleri sağlar.

Ücretsiz ve sağlam bir risk ortamında çalışan kendi ilerleme durumlarını izlemek ve kuruluşunuz için en önemli odak alanlarına odaklanmak seçebilirsiniz.

Çözüm ekledikten sonra değerlendirme gerçekleştirilir, Özet bilgi odak alanlarına odaklanmak için gösterilir **sistem Center Operations Manager sistem durumu denetimi** altyapınız için Pano. Aşağıdaki bölümlerde bilgileri kullanmak üzere nasıl **sistem Center Operations Manager sistem durumu denetimi** Pano, burada görüntüleyebilir ve ardından Al eylemleri, Operations Manager ortamınız için önerilen.

![System Center Operations Manager çözüm kutucuğu](./media/scom-assessment/log-analytics-scom-healthcheck-tile.png)

![System Center Operations Manager sistem durumu denetimi Panosu](./media/scom-assessment/log-analytics-scom-healthcheck-dashboard-01.png)

## <a name="installing-and-configuring-the-solution"></a>Çözümünü yükleme ve yapılandırma

Çözüm, Microsoft System Center 2012 Operations Manager Hizmet Paketi 1, Microsoft System Center 2012 R2 Operations Manager, Microsoft System Center 2016 Operations Manager, Microsoft System Center 2016 Operations Manager ve Microsoft System ile çalışır Center Operations Manager 1807

Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

- Sistem durumu denetimi çözümü Log Analytics'te kullanabilmeniz için önce çözümü yüklenmiş olması gerekir. Çözüm yükleme [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview).

- Çözüm çalışma alanına ekledikten sonra **sistem Center Operations Manager sistem durumu denetimi** Panodaki kutucuk bir ek yapılandırma gerekli iletisi görüntüler. Kutucuğuna ve sayfasında belirtilen yapılandırma adımlarını izleyin

  ![System Center Operations Manager Pano kutucuğu](./media/scom-assessment/scom-configrequired-tile.png)

> [!NOTE]
> System Center Operations Manager yapılandırma yapılabilir Log analytics'te çözüm yapılandırma sayfasında belirtilen adımları izleyerek bir komut dosyası kullanma.

 Operations Manager işletim Konsolu aracılığıyla değerlendirmeyi yapılandırmak için aşağıdaki adımları aşağıdaki sırayla gerçekleştirin:
1. [Sistem Center Operations Manager sistem durumu denetimi için farklı çalıştır hesabı ayarlayın](#operations-manager-run-as-accounts-for-log-analytics)  
2. Sistem Center Operations Manager sistem durumu denetimi kuralı yapılandırma

## <a name="system-center-operations-manager-health-check-data-collection-details"></a>System Center Operations Manager sistem durumu denetimi veri koleksiyonu ayrıntıları

Sistem Center Operations Manager sistem durumu denetimi çözümü, aşağıdaki kaynaklardan toplar:

* Kayıt Defteri
* Windows Yönetim Araçları (WMI)
* Olay günlüğü
* Dosya verileri
* PowerShell ve SQL sorguları, belirttiğiniz bir yönetim sunucusundan doğrudan Operations Manager'ı kullanma.  

Veriler yönetim Sunucusu'nda toplanır ve her yedi günde Log Analytics'e iletilir.  

## <a name="operations-manager-run-as-accounts-for-log-analytics"></a>Log Analytics için Operations Manager farklı çalıştırma hesapları

Log Analytics yönetim paketleri sağlamak iş yükleri için derlemelerinde değeri-hizmetlerini ekleyin. Her iş yükü, etki alanı kullanıcı hesabı gibi farklı güvenlik bağlamı yönetim paketlerini çalıştırmak için iş yüküne özgü ayrıcalıkları gerektirir. Bir Operations Manager farklı çalıştır hesabı, ayrıcalıklı kimlik bilgileriyle yapılandırın. Ek bilgi için bkz: [bir farklı çalıştır hesabının nasıl oluşturulacağını](https://technet.microsoft.com/library/hh321655(v=sc.12).aspx) Operations Manager belgelerinde.

Sistem Center Operations Manager sistem durumu denetimi için Operations Manager farklı çalıştır hesabı ayarlamak için aşağıdaki bilgileri kullanın.

### <a name="set-the-run-as-account"></a>Farklı Çalıştır hesabı ayarlayın

Farklı Çalıştır hesabı aşağıdaki devam etmeden önce gereksinimleri karşılaması gerekir:

* Tüm Operations Manager rolüne - yönetim sunucusu, işletimsel barındıran SQL Server, veri ambarı ve ACS veritabanı destekleyen tüm sunucularında yerel Administrators grubunun üyesi olan bir etki alanı kullanıcı hesabı raporlama, Web konsolu ile ağ geçidi sunucusu.
* İncelenen yönetim grubu için işlem yöneticisi rolü
* Hesaba SQL sysadmin hakları yoksa, ardından yürütme [betik](#sql-script-to-grant-granular-permissions-to-the-run-as-account) hesapta bir veya tüm Operations Manager veritabanlarını barındıran her SQL Server örneği için ayrıntılı izinler vermek için.

1. Operations Manager Konsolu'ndaki tanıyı seçin **Yönetim** gezinti düğmesi.
2. Altında **farklı çalıştır Yapılandırması**, tıklayın **hesapları**.
3. İçinde **Çalıştır hesabı oluştur** Sihirbazı'nda **giriş** sayfasında **sonraki**.
4. Üzerinde **Genel Özellikler** sayfasında **Windows** içinde **farklı çalıştır hesabı türü:** listesi.
5. Bir görünen ad yazın **görünen adı** metin kutusu ve isteğe bağlı olarak bir açıklama yazın **açıklama** kutusuna ve ardından **sonraki**.
6. Üzerinde **dağıtım güvenliği** sayfasında **daha güvenli**.
7. **Oluştur**’a tıklayın.  

Farklı Çalıştır hesabı oluşturulduğuna göre hedef yönetim grubundaki yönetim sunucularının gerekiyor ve iş akışları, kimlik bilgilerini kullanarak çalışacak şekilde bir önceden tanımlanmış farklı çalıştır profili ile ilişkilendirilmiş.  

1. Altında **farklı çalıştır Yapılandırması**, **hesapları**, sonuçlar bölmesinde, daha önce oluşturduğunuz hesaba çift tıklayın.
2. Üzerinde **dağıtım** sekmesinde **Ekle** için **seçilen bilgisayarlar** kutusunda ve hesaba dağıtmak için yönetim sunucusu ekleyin.  Tıklayın **Tamam** iki kez yaptığınız değişiklikleri kaydedin.
3. Altında **farklı çalıştır Yapılandırması**, tıklayın **profilleri**.
4. Arama *SCOM değerlendirmesi profili*.
5. Profil adı olması gerekir: *Microsoft System Center Operations Manager sistem durumu denetimi farklı çalıştır profili*.
6. Sağ tıklayın ve özelliklerini güncelleştirin ve son oluşturduğunuz farklı çalıştır daha önce oluşturduğunuz hesabı ekleyin.

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a>Farklı Çalıştır hesabı için ayrıntılı izinler vermek için SQL betiği

Operations Manager işletimsel barındırma, veri ambarı ve ACS veritabanı tarafından kullanılan SQL Server örneğinde farklı çalıştır hesabı için gerekli izinleri vermek için aşağıdaki SQL betiğini yürütün.

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

Adlı bir kural sistem Center Operations Manager sistem durumu denetimi çözümün Yönetim paketini içeren *Microsoft System Center Operations Manager sistem durumu denetleyin kural çalıştırma*. Bu kural, sistem durumu denetimini çalıştırmaktan sorumludur. Kuralı etkinleştirmek ve sıklığını yapılandırmak için aşağıdaki yordamları kullanın.

Varsayılan olarak, Microsoft System Center Operations Manager sistem durumu denetleyin kural çalıştırma devre dışıdır. Sistem durumu denetimini çalıştırmak için kural bir yönetim sunucusunda etkinleştirmeniz gerekir. Aşağıdaki adımları kullanın.

#### <a name="enable-the-rule-for-a-specific-management-server"></a>Belirli bir yönetim sunucusuna ilişkin kuralını etkinleştirme

1. İçinde **yazma** kuralını arayın Operations Manager işletim konsolunun çalışma *Microsoft System Center Operations Manager sistem durumu denetleyin kural çalıştırma* içinde **kuralları** bölmesi.
2. Metin içeren bir arama sonuçlarında seçin *türü: Yönetim sunucusu*.
3. Kurala sağ tıklayın ve ardından **geçersiz kılmalar** > **sınıfın belirli bir nesnesi için: Yönetim sunucusu**.
4.  Kullanılabilir yönetim sunucuları listesinde, kuralın çalıştırıldığı yönetim sunucularını seçin.  Bu, aynı yönetim sunucusuna daha önce farklı çalıştır hesabı ile ilişkilendirmek üzere yapılandırılmış olması gerekir.
5.  Geçersiz kılma değerine değiştirdiğinizden emin olun **True** için **etkin** parametre değeri.<br><br> ![parametre geçersiz kıl](./media/scom-assessment/rule.png)

    Yine de bu pencerede karşın, bir sonraki yordamı kullanarak çalışma sıklığını yapılandırın.

#### <a name="configure-the-run-frequency"></a>Çalışma sıklığını yapılandırma

Değerlendirme her 10.080 dakika (veya yedi gün) çalıştırmak için varsayılan olarak yapılandırılır. Değer 1440 dakika (veya bir gün) en düşük değerini geçersiz kılabilirsiniz. Değer, art arda gelen değerlendirme çalışmaları arasında gerekli en düşük zaman aralığı temsil eder. Aralık geçersiz kılmak için aşağıdaki adımları kullanın.

1. İçinde **yazma** kuralını arayın Operations Manager konsolunun çalışma *Microsoft System Center Operations Manager sistem durumu denetleyin kural çalıştırma* içinde **kuralları** bölümü.
2. Metin içeren bir arama sonuçlarında seçin *türü: Yönetim sunucusu*.
3. Kurala sağ tıklayın ve ardından **kuralı geçersiz kıl** > **sınıfın tüm nesneleri için: Yönetim sunucusu**.
4. Değişiklik **aralığı** parametre değeri, istenen aralığı değeri. Aşağıdaki örnekte, değer 1440 dakika (bir gün) olarak ayarlanır.<br><br> ![aralığı parametresi](./media/scom-assessment/interval.png)<br>  

    Değer 1440 dakikadan daha kısa bir süre için ayarlarsanız, kural tek günlük bir aralık üzerinde çalışır. Bu örnekte, kural aralık değeri yoksayar ve bir günlük bir sıklıkta çalıştırır.


## <a name="understanding-how-recommendations-are-prioritized"></a>Öneriler nasıl önceliklendirilir anlama

Yaptığınız her öneri, öneri göreceli önemini tanımlayan bir ağırlık değeri verilir. Yalnızca en önemli 10 önerileri gösterilir.

### <a name="how-weights-are-calculated"></a>Ağırlıklar nasıl hesaplanır

Ağırlık belirlemeyi üç anahtar etkenlere göre toplam değerler şunlardır:

- *Olasılık* tanımlanan bir sorunun sorunlara neden. Daha yüksek bir olasılık öneri için daha büyük bir genel puan karşılık gelir.
- *Etkisi* kuruluşunuzdaki bir soruna neden olursa sorunun. Daha yüksek bir etkisi öneri için daha büyük bir genel puan karşılık gelir.
- *Çaba* öneriyi uygulamak için gereklidir. Daha yüksek bir çaba öneri için daha küçük bir genel puan karşılık gelir.

Her öneri için hesaplamasının her odak alanı için kullanılabilir toplam puanı yüzdesi olarak ifade edilir. Örneğin, bu öneri uygulandıktan bir puan % 5, kullanılabilirlik ve iş sürekliliği odak alanında bir öneri varsa, genel kullanılabilirlik ve iş sürekliliği puanı tarafından %5 artırır.

### <a name="focus-areas"></a>Odak alanları

**Kullanılabilirlik ve iş sürekliliği** -hizmet kullanılabilirlik, dayanıklılık, altyapı ve işletme korumasına yönelik öneriler bu odak alanı gösterir.

**Performans ve ölçeklenebilirlik** -bu odak alanı kuruluşunuzun yardımcı olacak öneriler gösterir BT altyapısı arttıkça, BT ortamınızı geçerli performans gereksinimlerini karşıladığından ve değişen altyapı mümkün olduğundan emin olun gerekir.

**Yükseltme, geçiş ve dağıtım** -bu odak alanı yükseltme, geçirme ve SQL Server'ı mevcut altyapınızı dağıtmadan yardımcı olacak öneriler gösterir.

**İşlemler ve izleme** -BT işlemlerinizi kolaylaştırın, önleyici bakım uygulama ve performansı en üst düzeye yardımcı olacak öneriler bu odak alanı gösterir.

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a>% 100 her odak alanında puanlamak için indirmeyi amaçlamanız gerekir?

Olmayabilir. Öneriler bilgi ve müşteri binlerce Microsoft mühendisleri tarafından elde edilen deneyimler temel alır. Ancak, hiçbir iki sunucu altyapıları aynıdır ve öneriler için daha az veya uygun olabilir. Örneğin, bazı güvenlik önerilerini sanal makinelerinize Internet'e açık değildir, daha az ilgili olabilir. Bazı kullanılabilirlik önerisi düşük öncelikli işler için geçici veri toplama ve raporlama sağladığı hizmetler için daha uygun olabilir. Olgun bir işletmeye için önemli olan sorunları için bir başlangıç daha az önemli olabilir. Önceliklerinizle odak alanları tanımlamak ve puanları, zaman içinde nasıl değiştiğini ardından aramak isteyebilirsiniz.

Her öneri neden önemli olduğu ile ilgili yönergeler içerir. Öneri uygulandıktan verilen BT hizmetlerinizin doğasını ve kuruluşunuzun iş gereksinimlerini, sizin için uygun olup olmadığını değerlendirmek için bu kılavuzu kullanın.

## <a name="use-health-check-focus-area-recommendations"></a>Odak alanı önerileri kullanım durumunu denetleme

Log Analytics'te bir sistem durumu denetimi çözümü kullanabilmeniz için önce çözümü yüklenmiş olması gerekir. Daha fazla bilgi için çözüm yükleme hakkında bkz [yönetim çözümü yüklemek](../../azure-monitor/insights/solutions.md). Yüklendikten sonra sistem Center Operations Manager sistem durumu denetimi kutucuk kullanarak önerileri özetini görüntüleyebilirsiniz **genel bakış** Azure portalında çalışma sayfası.

Altyapınız ve ardından-ayrıntıya önerileri için Özet uyumluluk değerlendirmesi görüntüleyin.

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a>Odak alanı için önerileri görüntüleme ve düzeltme eylemi
1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Azure portalının sol alt köşesinde bulunan **Diğer hizmetler**'e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
3. Log Analytics abonelikleri bölmesinde, bir çalışma alanı seçin ve ardından **çalışma özeti** menü öğesi.  
4. Üzerinde **genel bakış** sayfasında **sistem Center Operations Manager sistem durumu denetimi** Döşe.
5. Üzerinde **sistem Center Operations Manager sistem durumu denetimi** sayfasında odak alanı dikey pencereleri biriyle özet bilgilerini gözden geçirin ve ardından bu odak alanı için önerileri görmek için tıklatın.
6. Herhangi bir odak alanı sayfalar üzerinde ortamınız için yapılan Önceliklendirilmiş öneriler görüntüleyebilirsiniz. Altında bir öneriye tıklayabilir **etkilenen nesneler** öneri neden yapılır hakkında ayrıntılı bilgi görüntülemek için.<br><br> ![Odak alanı](./media/scom-assessment/log-analytics-scom-healthcheck-dashboard-02.png)<br>
7. Önerilen düzeltici eylemleri gerçekleştirebilirsiniz **önerilen eylemleri**. Öğe ele alındığında, önerilen eylemlerin yapıldığını ve uyumluluk puanınız artıracaktır sonraki değerlendirmeler kaydeder. Düzeltilen öğeler görünür olarak **geçirilen nesneleri**.

## <a name="ignore-recommendations"></a>Öneriler yoksay

Yok saymak için istediğiniz önerilerini varsa, öneriler, değerlendirme sonuçlarında görüntülenmesini önlemek için Log Analytics kullanan bir metin dosyası oluşturabilirsiniz.

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a>Yok saymak için istediğiniz önerilerini tanımlamak için
1. Log Analytics çalışma sayfasında seçilen çalışma alanınız için Azure portalında **günlük araması** menü öğesi.
2. Ortamınızdaki bilgisayarları için başarısız olan liste öneriler için aşağıdaki sorguyu kullanın.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select Computer, RecommendationId, Recommendation | sort Computer
    ```

    >[!NOTE]
    > Çalışma alanınız için yükseltildiyse [yeni Log Analytics sorgu diline](../../azure-monitor/log-query/log-query-overview.md), yukarıdaki sorguda, şu şekilde değiştirilmesi gerekir.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Failed" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

    Günlük araması sorgusuna gösteren ekran görüntüsü aşağıda verilmiştir:<br><br> ![günlük araması](./media/scom-assessment/scom-log-search.png)<br>

3. Yok saymak için istediğiniz önerilerini seçin. Sonraki yordamda Recommendationıd için değerleri kullanacaksınız.

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a>Oluşturma ve bir IgnoreRecommendations.txt metin dosyası kullanma

1. IgnoreRecommendations.txt adlı bir dosya oluşturun.
2. Yapıştırın veya ayrı bir satıra yoksay ve sonra dosyayı kaydedip kapatın için Log Analytics'i istediğiniz her bir öneri için her Recommendationıd yazın.
3. Dosya, aşağıdaki klasörde, önerileri yok saymak için Log Analytics istediğiniz her bilgisayara yerleştirin.
4. Operations Manager yönetim sunucusunda - *SystemDrive*: \Program System Center 2012 R2\Operations Manager\Server.

### <a name="to-verify-that-recommendations-are-ignored"></a>Öneriler göz ardı edilir doğrulamak için

1. Sonraki değerlendirme çalışır, varsayılan olarak yedi günde bir zamanlanmış sonra belirtilen önerileri yoksayıldı işaretlenir ve sistem durumu denetimi Panoda görünmez.
2. Yoksayılan tüm önerilere listelemek için aşağıdaki günlük arama sorgularını kullanabilirsiniz.

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    >[!NOTE]
    > Çalışma alanınız için yükseltildiyse [yeni Log Analytics sorgu diline](../../azure-monitor/log-query/log-query-overview.md), yukarıdaki sorguda, şu şekilde değiştirilmesi gerekir.
    >
    > `SCOMAssessmentRecommendationRecommendation | where RecommendationResult == "Ignore" | sort by Computer asc | project Computer, RecommendationId, Recommendation`

3. Daha sonra yoksayılan önerileri görmek istediğinize karar verirseniz, IgnoreRecommendations.txt dosyaları kaldırın veya bunları RecommendationIDs kaldırabilirsiniz.

## <a name="system-center-operations-manager-health-check-solution-faq"></a>System Center Operations Manager sistem durumu denetimi çözümü ile ilgili SSS

*Sistem durumu denetimi çözümü için Log Analytics çalışma Alanım ekledim. Ancak, öneriler göremiyorum. Neden olmasın?* Çözüm ekledikten sonra aşağıdaki adımları görünümü Log Analytics Panoda önerileri kullanın.  

- [Sistem Center Operations Manager sistem durumu denetimi için farklı çalıştır hesabı ayarlayın](#operations-manager-run-as-accounts-for-log-analytics)  
- [Sistem Center Operations Manager sistem durumu denetimi kuralı yapılandırma](#configure-the-health-check-rule)


*Onay çalışma sıklığını yapılandırmak için bir yol var mı?* Evet. Bkz: [çalıştırma sıklığını yapılandırmak](#configure-the-run-frequency).

*Ben sistem Center Operations Manager sistem durumu denetimi çözümü ekledikten sonra başka bir sunucuya bulunmuşsa, denetlenecek?* Evet, bulunduktan sonra daha sonra varsayılan olarak yedi günde denetlenir.

*Veri koleksiyonu yapan işlemin adı nedir?* AdvisorAssessment.exe

*Burada AdvisorAssessment.exe işlem çalışıyor mu?* AdvisorAssessment.exe sistem durumu onay kuralı etkinleştirdiğiniz yönetim sunucusunun HealthService işlem altında çalışır. Bu işlem, tüm ortamınıza ilişkin bulma uzaktan veri toplama kullanılmasıdır.

*Ne kadar veri toplamaya yönelik sürer?* Veri toplama sunucusuna yaklaşık bir saat sürer. Bu pek çok Operations Manager örnek veya veritabanı olan ortamlarda daha uzun sürebilir.

*Peki değerlendirme aralığını 1440 dakikadan daha kısa bir süre için ayarlayabilirim?* Değerlendirme, en fazla günde bir kere çalışmak üzere önceden yapılandırılmıştır. Aralık değeri için bir değer 1440 dakikadan geçersiz kılarsanız değerlendirme 1440 dakika aralığı değeri kullanır.

*Önkoşul hataları olup olmadığını bilmek nasıl?* Ardından sistem durumu denetimini çalıştırıldı ve sonuçları göremiyorsanız, bazı Önkoşullar onay için başarısız olduğunu büyük olasılıkla olur. Sorgu yürütebilirsiniz: `Operation Solution=SCOMAssessment` ve `SCOMAssessmentRecommendation FocusArea=Prerequisites` Log Search başarısız önkoşullara bakın.

*Var olan bir `Failed to connect to the SQL Instance (….).` önkoşul hatası iletisi. Sorun nedir?* AdvisorAssessment.exe, verileri toplayan işlem HealthService işlem altında yönetim sunucusunda çalışır. Sistem durumu denetimi bir parçası olarak, işlem Operations Manager veritabanının mevcut olduğu SQL Server'a bağlanmak çalışır. Güvenlik duvarı kuralları SQL Server örneği bağlantısı engellediğinizde bu hata oluşabilir.

*Ne tür verilere toplanır?* Aşağıdaki veri türlerini toplanır: - WMI verilerini - kayıt defteri - olay günlüğü verilerini - Operations Manager verilerini Windows PowerShell, SQL sorguları ve dosya bilgileri Toplayıcı aracılığıyla.

*Farklı Çalıştır hesabı yapılandırmak neden gerekiyor?* Operations Manager ile çeşitli SQL sorguları çalıştırılır. Sırayla çalışmak üzere için gerekli izinlere sahip farklı çalıştır hesabı kullanmanız gerekir. Ayrıca, yerel yönetici kimlik bilgileri WMI Sorgu için gereklidir.

*Neden yalnızca ilk 10 önerileri görüntülensin mi?* Kapsamlı, büyük bir Görevler listesini vermek yerine, Önceliklendirilmiş öneriler adresleme odaklanmak öneririz. Bunları adres sonra ek öneriler kullanılabilir hale gelir. Ayrıntılı listesini görmek isterseniz, günlük arama özelliğini kullanarak tüm önerileri görüntüleyebilirsiniz.

*Bir öneri yok saymak için bir yol var mı?* Evet, bkz: [önerileri yoksay](#ignore-recommendations).


## <a name="next-steps"></a>Sonraki adımlar

- [Arama günlüklerini](../../azure-monitor/log-query/log-query-overview.md) ayrıntılı verileri System Center Operations Manager sistem durumunu denetleyin ve önerileri çözümleme hakkında bilgi edinmek için.
