---
title: "Azure yedekleme: Azure İzleyici ile Azure Backup'ı izleme"
description: Azure Backup iş yüklerini izleyebilir ve Azure İzleyicisi'ni kullanarak özel uyarılar oluşturabilirsiniz.
services: backup
author: pvrk
manager: shivamg
keywords: Log Analytics; Azure yedekleme; Uyarıları; Tanılama ayarları; Eylem grupları
ms.service: backup
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: pullabhk
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: e2d4a235737789f2f5852c00218427613db3d558
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786325"
---
# <a name="monitor-at-scale-by-using-azure-monitor"></a>Uygun ölçekte Azure İzleyicisi'ni kullanarak izleme

Azure Backup sağlar [yerleşik izleme ve uyarı özellikleri](backup-azure-monitoring-built-in-monitor.md) bir kurtarma Hizmetleri Kasası'nda. Bu özellikler, herhangi bir ek yönetim altyapısı kullanılabilir. Ancak, bu yerleşik hizmet aşağıdaki senaryolarda sınırlıdır:

- Abonelikler arasında birden çok kurtarma Hizmetleri kasaları verilerden izlerseniz
- Tercih edilen bildirim kanalını ise *değil* e-posta
- Kullanıcıların daha fazla senaryo için uyarılar istiyorsanız
- Azure'da, portalın içinde göstermez bir şirket içi bileşeni gibi System Center Data Protection Manager bilgilerini görüntülemek isteyip istemediğinizi [ **yedekleme işleri** ](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) veya [  **Yedekleme uyarıları**](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault)

## <a name="using-log-analytics-workspace"></a>Log Analytics çalışma alanını kullanma

> [!NOTE]
> Log Analytics çalışma alanına tanılama ayarları aracılığıyla Azure VM yedeklemeleri, Azure Backup aracısını, System Center Data Protection Manager, Azure sanal makinelerinde SQL yedekleme ve Azure dosya paylaşımı yedeklemelerini verileri ekleniyor. 

Uygun ölçekte izlemek için iki Azure Hizmetleri yeteneklerini gerekir. *Tanılama ayarları* birden çok Azure Resource Manager kaynakları başka bir kaynağına veri gönderen. *Log Analytics* kullanabileceğiniz Eylem grupları diğer bildirim kanallarına tanımlamak için özel uyarılar oluşturur. 

Aşağıdaki bölümlerde uygun ölçekte Azure Backup'ı izlemek için Log Analytics kullanma ayrıntılı olarak açıklanmaktadır.

### <a name="configure-diagnostic-settings"></a>Tanılama ayarlarını yapılandırma

Kurtarma Hizmetleri kasası gibi Azure Resource Manager kaynaklarını tanılama verisi olarak zamanlanmış işlemleri ve kullanıcı tetiklenen işlemleri hakkında bilgi kaydedin. 

İzleme bölümünde **tanılama ayarları** ve kurtarma Hizmetleri kasasının tanılama veri hedefini belirtin.

![Kurtarma Hizmetleri kasasının tanılama ayarı, log Analytics hedefleme](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

Bir Log Analytics çalışma alanı başka bir Abonelikteki hedefleyebilirsiniz. Tek bir yerde Aboneliklerdeki kasaları izlemek için birden çok kurtarma Hizmetleri kasaları için aynı Log Analytics çalışma alanını seçin. Azure Backup için Log Analytics çalışma alanına ilgili tüm bilgileri kanal için seçin **AzureBackupReport** günlük olarak.

> [!IMPORTANT]
> Yapılandırmasını tamamladıktan sonra ilk veri gönderimi tamamlanması 24 saat beklemeniz gerekir. Veri gönderimi ilk sonra bu makalenin sonraki bölümlerinde anlatıldığı gibi tüm olayları itilir [sıklığı bölüm](#diagnostic-data-update-frequency).

### <a name="deploy-a-solution-to-the-log-analytics-workspace"></a>Log Analytics çalışma alanına bir çözüm dağıtma

Verilerin Log Analytics çalışma alanı içinde sonra [GitHub şablon dağıtma](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) verileri görselleştirmek için Log Analytics için. Düzgün çalışma belirlemek için aynı kaynak grubunu, çalışma alanı adı ve çalışma alanı konumunu verin emin olun. Daha sonra bu şablon, çalışma alanı yükleyin.

> [!NOTE]
> Log Analytics çalışma alanınızda uyarılar, yedekleme işleri ya da geri yükleme işleri yoksa, bir "BadArgumentError" hata kodu portalında görebilirsiniz. Bu hatayı yoksayarak çözümü kullanmaya devam edin. Çalışma alanına akan ilgili verileri başlatır yazdıktan sonra aynı görselleştirmeler yansıtır ve hata artık görmez.

### <a name="view-azure-backup-data-by-using-log-analytics"></a>Log Analytics kullanarak Azure Backup verileri görüntüleme

Şablon dağıtıldıktan sonra Azure Backup izleme çözümü çalışma alanı Özet bölgede gösterilir. Özete dönmek için bu yollar birini izleyin:

- **Azure İzleyici**: İçinde **Insights** bölümünden **daha fazla** ilgili çalışma alanını seçin.
- **Log Analytics çalışma alanları**: İlgili çalışma alanını seçin ve ardından altındaki **genel**seçin **çalışma özeti**.

![Log Analytics izleme kutucuğu](media/backup-azure-monitoring-laworkspace/la-azurebackup-azuremonitor-tile.png)

İzleme kutucuğu seçtiğinizde, Tasarımcı şablon grafikler hakkında temel izleme verilerini bir dizi Azure Backup'tan açılır. Göreceğiniz grafiklerin bazıları şunlardır:

* Tüm yedekleme işleri

   ![Yedekleme işleri için log Analytics grafikleri](media/backup-azure-monitoring-laworkspace/la-azurebackup-allbackupjobs.png)

* Geri yükleme işlerini

   ![Geri yükleme işleri için log Analytics grafiği](media/backup-azure-monitoring-laworkspace/la-azurebackup-restorejobs.png)

* Azure kaynakları için yerleşik Azure yedekleme uyarıları

   ![Azure kaynakları için yerleşik Azure Backup uyarılar için log Analytics grafiği](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts.png)

* Şirket içi kaynaklar için yerleşik Azure yedekleme uyarıları

   ![Şirket içi kaynaklar için yerleşik Azure Backup uyarılar için log Analytics grafiği](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts-onprem.png)

* Etkin veri kaynakları

   ![Log Analytics grafiği etkin yedeklenen varlıklar için](media/backup-azure-monitoring-laworkspace/la-azurebackup-activedatasources.png)

* Bulut depolama kurtarma Hizmetleri kasası

   ![Kurtarma Hizmetleri kasası, bulut depolama için log Analytics grafiği](media/backup-azure-monitoring-laworkspace/la-azurebackup-cloudstorage-in-gb.png)

Bu grafik, şablonla sağlanır. Grafikleri düzenlemek veya gerekirse daha fazla grafik ekleyin.

> [!IMPORTANT]
> Şablon dağıtırken, aslında bir salt okunur kilidi oluşturuyorsunuz. Düzenleyip şablonu kaydetmek için kilit kaldırmanız gerekir. Bir kilit kaldırabilirsiniz **ayarları** Log Analytics çalışma Alanım bölümünde, **kilitler** bölmesi.

### <a name="create-alerts-by-using-log-analytics"></a>Log Analytics kullanarak uyarıları oluşturma

Azure İzleyici'de, kendi bir Log Analytics çalışma alanı Uyarılardaki oluşturabilirsiniz. Çalışma alanında, kullandığınız *Azure Eylem grupları* tercih edilen bildirim mekanizmanızı seçin. 

> [!IMPORTANT]
> Bu sorgu oluşturma maliyeti hakkında bilgi için [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/).

Grafiklerin açmak için seçin **günlükleri** Log Analytics çalışma Alanım bölümünde. İçinde **günlükleri** bölümünde sorguları Düzenle ve bunlar üzerinde uyarılar oluşturun.

![Log Analytics çalışma alanında bir uyarı oluştur](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

Seçtiğinizde, **yeni uyarı kuralı**, aşağıdaki görüntüde gösterildiği gibi Azure İzleyici uyarı oluşturma sayfası açılır. Burada Log Analytics çalışma alanı olarak işaretlenmiş kaynak ve eylem grubu tümleştirme sağlanmıştır.

![Log Analytics uyarı oluşturma sayfası](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

#### <a name="alert-condition"></a>Uyarı koşulu

Bir uyarının tanımlama özelliği, tetikleyici bir durumdur. Seçin **koşul** Kusto sorgusu otomatik olarak yüklemek için **günlükleri** sayfasında aşağıdaki görüntüde gösterildiği gibi. Burada koşul gereksinimlerinize uyacak şekilde düzenleyebilirsiniz. Daha fazla bilgi için [örnek Kusto sorgu](#sample-kusto-queries).

![Bir uyarı koşulu ayarlama](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Gerekirse, Kusto sorgusu düzenleyebilirsiniz. Eşik, süresini ve sıklığı seçin. Eşiğin ne zaman bir uyarı gerçekleştirilecektir belirler. Sorgu çalıştığı zaman penceresi dönemdir. Örneğin, eşik, 0'dan büyükse, süre 5 dakikadır ve sıklığı 5 dakikadır, sonra kural sorguyu 5 dakikada önceki 5 dakika boyunca gözden geçirme çalışır. Sonuç sayısı 0'dan büyükse seçili eylem grubu üzerinden bildirim alırsınız.

#### <a name="alert-action-groups"></a>Uyarı Eylem grupları

Bir bildirim kanalı belirtmek için bir eylem grubu kullanın. Kullanılabilir bildirim mekanizmaları altında görmek için **Eylem grupları**seçin **Yeni Oluştur**.

!["Eylem Grup Ekle" penceresinde kullanılabilir bildirim mekanizmaları](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

Tüm uyarı ve izleme ile tek başına Log Analytics gereksinimleri karşılayabilecek veya Log Analytics yerleşik bildirimlerini desteklemek üzere kullanabilirsiniz.

Daha fazla bilgi için bkz. [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak günlük uyarıları yönetin](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) ve [oluştur ve Eylem grupları Azure portalında yönetmek](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups).

### <a name="sample-kusto-queries"></a>Kusto sorgu örneği

Varsayılan grafikleri Kusto sorgu temel senaryolar için uyarılar oluşturun verin. Uyarı almak istediğiniz verileri almak için sorguları da değiştirebilirsiniz. Aşağıdaki örnek Kusto sorgularda yapıştırın **günlükleri** sayfasında ve sonra uyarılar üzerinde sorgular oluşturun:

* Tüm başarılı yedekleme işleri

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Completed"
    ````
    
* Tüm yedekleme işleri başarısız oldu

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Failed"
    ````
    
* Tüm başarılı Azure VM yedekleme işleri

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "VM" and BackupManagementType_s == "IaaSVM"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

* Tüm başarılı SQL günlük yedekleme işleri

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s == "Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "SQLDataBase" and BackupManagementType_s == "AzureWorkload"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

* Tüm başarılı Azure Backup Aracısı işleri

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "FileFolder" and BackupManagementType_s == "MAB"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

### <a name="diagnostic-data-update-frequency"></a>Tanılama verilerini güncelleştirme sıklığı

Tanılama verileri kasadan bazı gecikme ile Log Analytics çalışma alanına ekleniyor. Log Analytics çalışma alanı her olay ulaşan *20-30 dakika* sonra kurtarma Hizmetleri kasasından gönderilir. Daha fazla gecikme ayrıntılarını şunlardır:

- Oluşturuldukları hemen sonra tüm çözümlerinde yedekleme hizmetin yerleşik uyarılar itilir. Bu nedenle bunlar genellikle Log Analytics çalışma alanında 20-30 dakika sonra görünür.
- Tüm çözümler, kısa sürede onlar geçici yedekleme işleri ve geri yükleme işleri itilir *son*.
- SQL yedekleme dışındaki tüm çözümler için zamanlanan yedekleme işlerinin kısa sürede onlar itilir *son*.
- SQL yedekleme için günlüğü yedeklemeleri 15 dakikada gerçekleşebileceği için bilgi için günlükleri de dahil olmak üzere tüm tamamlanan zamanlanan yedekleme işlerinin, toplu ve 6 saatte bir gönderildi.
- Tüm çözümler arasında en az yedekleme öğesi, ilke, Kurtarma noktaları, depolama ve benzeri gibi diğer bilgiler gönderilir *günde bir kez.*
- Bir değişiklik (örneğin, ilke değiştirme veya düzenleme İlkesi) yedekleme yapılandırması, yedekleme ilgili tüm bilgileri bir anında iletme tetikler.

## <a name="using-the-recovery-services-vaults-activity-logs"></a>Kurtarma Hizmetleri kasasının etkinlik günlüklerini kullanma

> [!CAUTION]
> Aşağıdaki adımlar yalnızca geçerli *Azure VM yedeklemeleri.* Bu adımlar Azure Backup Aracısı, Azure dosyaları veya Azure SQL yedeklerini gibi çözümler için kullanamazsınız.

Etkinlik günlükleri, yedekleme başarısının gibi olayları bildirimi almak için de kullanabilirsiniz. Başlamak için aşağıdaki adımları izleyin:

1. Azure portalında oturum açın.
1. İlgili kurtarma Hizmetleri kasasını açın. 
1. Kasanın Özellikleri'nde açın **etkinlik günlüğü** bölümü.

Uygun günlük tanımlamak ve bir uyarı oluşturmak için:

1. Aşağıdaki görüntüde gösterilen filtreler uygulayarak başarılı yedeklemeler için etkinlik günlüklerini alıyorsunuz doğrulayın. Değişiklik **Timespan** kayıtları görüntülemek için gerekli olan değer.

   ![Azure VM yedeklemeleri için etkinlik günlüklerini bulmak için filtreleme](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

1. İlgili ayrıntıları görmek için işlem adı seçin.
1. Seçin **yeni uyarı kuralı** açmak için **oluşturma kuralı** sayfası. 
1. İçindeki adımları izleyerek bir uyarı oluşturmak [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Etkinlik günlüğü uyarıları yönetin](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log).

   ![Yeni uyarı kuralı](media/backup-azure-monitoring-laworkspace/new-alert-rule.png)

Kurtarma Hizmetleri kasası kaynağı aşağıdadır. Tüm etkinlik günlükleri bildirim almak istediğiniz kasaları için aynı adımları yinelemeniz gerekir. Etkinliklere göre bu uyarı için eşik, nokta veya sıklığı koşul olmaz. İlgili etkinlik günlüğü oluşturulur hemen sonra uyarı verilir.

## <a name="using-log-analytics-to-monitor-at-scale"></a>Uygun ölçekte izlemek için log Analytics kullanma

Etkinlik Günlükleri ve Log Analytics çalışma alanlarını Azure İzleyici'de oluşturulan tüm uyarıları görüntüleyebilirsiniz. Yalnızca açık **uyarılar** bölmesinde solda.

Etkinlik günlükleri aracılığıyla bildirimleri edinebilecek olsanız da ölçekli olarak izlemek için etkinlik günlüklerini yerine Log Analytics kullanarak önerilir. Bunu istememizin nedeni:

- **Sınırlı senaryoları**: Etkinlik günlükleri aracılığıyla bildirimleri yalnızca Azure VM yedeklemeleri için geçerlidir. Bildirimler her bir kurtarma Hizmetleri kasası için ayarlanmış olması gerekir.
- **Uygun tanımı**: Zamanlanmış yedekleme etkinlik, etkinlik günlükleri ile en son tanımlarla sığmıyor. Bunun yerine, ile uyumludur [tanılama günlükleri](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#what-you-can-do-with-diagnostic-logs). Etkinlik akan veri kanalı değişiklikleri oturum açtığınızda bu hizalama beklenmeyen etkilere neden olur.
- **Etkinlik günlüğü kanalındaki sorunları**: Kurtarma Hizmetleri kasalarında, yeni bir modeli Azure Backup'tan ekleniyor etkinlik günlüklerini izleyin. Ne yazık ki, bu değişiklik, Azure kamu, Azure Almanya ve Azure Çin 21Vianet etkinlik günlüklerinde nesil etkiler. Kullanıcılara bu bulut Hizmetleri oluşturun veya Azure İzleyici'de etkinlik günlüklerinden herhangi bir uyarıyı yapılandırmanız, tetiklenen uyarıları değildir. Ayrıca, tüm Azure bölgelerinde genel, bir kullanıcı [kurtarma Hizmetleri etkinlik günlüklerini Log Analytics çalışma alanınıza toplar](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs), bu günlükleri görünmez.

İzleme ve uygun ölçekte Azure Backup tarafından korunan tüm iş yükleriniz için uyarı için bir Log Analytics çalışma alanını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Özel sorgular oluşturmak için bkz [Log Analytics veri modeli](backup-azure-log-analytics-data-model.md).
